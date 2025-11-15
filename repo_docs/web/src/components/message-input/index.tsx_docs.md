# File Documentation: web/src/components/message-input/index.tsx

## File Metadata

- **Path**: `web/src/components/message-input/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 373
- **Characters**: 10,892
- **Size**: 10,892 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import {
  useDeleteDocument,
  useFetchDocumentInfosByIds,
  useRemoveNextDocument,
  useUploadAndParseDocument,
} from '@/hooks/document-hooks';
import { cn } from '@/lib/utils';
import { getExtension } from '@/utils/document-util';
import { formatBytes } from '@/utils/file-util';
import {
  CloseCircleOutlined,
  InfoCircleOutlined,
  LoadingOutlined,
} from '@ant-design/icons';
import type { GetProp, UploadFile } from 'antd';
import {
  Button,
  Card,
  Divider,
  Flex,
  Input,
  List,
  Space,
  Spin,
  Typography,
  Upload,
  UploadProps,
} from 'antd';
import get from 'lodash/get';
import { CircleStop, Paperclip, SendHorizontal } from 'lucide-react';
import {
  ChangeEventHandler,
  memo,
  useCallback,
  useEffect,
  useRef,
  useState,
} from 'react';
import FileIcon from '../file-icon';
import styles from './index.less';

type FileType = Parameters<GetProp<UploadProps, 'beforeUpload'>>[0];
const { Text } = Typography;

const { TextArea } = Input;

const getFileId = (file: UploadFile) => get(file, 'response.data.0');

const getFileIds = (fileList: UploadFile[]) => {
  const ids = fileList.reduce((pre, cur) => {
    return pre.concat(get(cur, 'response.data', []));
  }, []);

  return ids;
};

const isUploadSuccess = (file: UploadFile) => {
  const code = get(file, 'response.code');
  return typeof code === 'number' && code === 0;
};

interface IProps {
  disabled: boolean;
  value: string;
  sendDisabled: boolean;
  sendLoading: boolean;
  onPressEnter(documentIds: string[]): void;
  onInputChange: ChangeEventHandler<HTMLTextAreaElement>;
  conversationId: string;
  uploadMethod?: string;
  isShared?: boolean;
  showUploadIcon?: boolean;
  createConversationBeforeUploadDocument?(message: string): Promise<any>;
  stopOutputMessage?(): void;
}

const getBase64 = (file: FileType): Promise<string> =>
  new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.readAsDataURL(file as any);
    reader.onload = () => resolve(reader.result as string);
    reader.onerror = (error) => reject(error);
  });

const MessageInput = ({
  isShared = false,
  disabled,
  value,
  onPressEnter,
  sendDisabled,
  sendLoading,
  onInputChange,
  conversationId,
  showUploadIcon = true,
  createConversationBeforeUploadDocument,
  uploadMethod = 'upload_and_parse',
  stopOutputMessage,
}: IProps) => {
  const { t } = useTranslate('chat');
  const { removeDocument } = useRemoveNextDocument();
  const { deleteDocument } = useDeleteDocument();
  const { data: documentInfos, setDocumentIds } = useFetchDocumentInfosByIds();
  const { uploadAndParseDocument } = useUploadAndParseDocument(uploadMethod);
  const conversationIdRef = useRef(conversationId);

  const [fileList, setFileList] = useState<UploadFile[]>([]);

  const handlePreview = async (file: UploadFile) => {
    if (!file.url && !file.preview) {
      file.preview = await getBase64(file.originFileObj as FileType);
    }
  };

  const handleChange: UploadProps['onChange'] = async ({
    // fileList: newFileList,
    file,
  }) => {
    let nextConversationId: string = conversationId;
    if (createConversationBeforeUploadDocument) {
      const creatingRet = await createConversationBeforeUploadDocument(
        file.name,
      );
      if (creatingRet?.code === 0) {
        nextConversationId = creatingRet.data.id;
      }
    }
    setFileList((list) => {
      list.push({
        ...file,
        status: 'uploading',
        originFileObj: file as any,
      });
      return [...list];
    });
    const ret = await uploadAndParseDocument({
      conversationId: nextConversationId,
      fileList: [file],
    });
    setFileList((list) => {
      const nextList = list.filter((x) => x.uid !== file.uid);
      nextList.push({
        ...file,
        originFileObj: file as any,
        response: ret,
        percent: 100,
        status: ret?.code === 0 ? 'done' : 'error',
      });
      return nextList;
    });
  };

  const isUploadingFile = fileList.some((x) => x.status === 'uploading');

  const handlePressEnter = useCallback(async () => {
    if (isUploadingFile) return;
    const ids = getFileIds(fileList.filter((x) => isUploadSuccess(x)));

    onPressEnter(ids);
    setFileList([]);
  }, [fileList, onPressEnter, isUploadingFile]);

  const handleKeyDown = useCallback(
    async (event: React.KeyboardEvent<HTMLTextAreaElement>) => {
      // check if it was shift + enter
      if (event.key === 'Enter' && event.shiftKey) return;
      if (event.key !== 'Enter') return;
      if (sendDisabled || isUploadingFile || sendLoading) return;

      event.preventDefault();
      handlePressEnter();
    },
    [sendDisabled, isUploadingFile, sendLoading, handlePressEnter],
  );

  const handleRemove = useCallback(
    async (file: UploadFile) => {
      const ids = get(file, 'response.data', []);
      // Upload Successfully
      if (Array.isArray(ids) && ids.length) {
        if (isShared) {
          await deleteDocument(ids);
        } else {
          await removeDocument(ids[0]);
        }
        setFileList((preList) => {
          return preList.filter((x) => getFileId(x) !== ids[0]);
        });
      } else {
        // Upload failed
        setFileList((preList) => {
          return preList.filter((x) => x.uid !== file.uid);
        });
      }
    },
    [removeDocument, deleteDocument, isShared],
  );

  const handleStopOutputMessage = useCallback(() => {
    stopOutputMessage?.();
  }, [stopOutputMessage]);

  const getDocumentInfoById = useCallback(
    (id: string) => {
      return documentInfos.find((x) => x.id === id);
    },
    [documentInfos],
  );

  useEffect(() => {
    const ids = getFileIds(fileList);
    setDocumentIds(ids);
  }, [fileList, setDocumentIds]);

  useEffect(() => {
    if (
      conversationIdRef.current &&
      conversationId !== conversationIdRef.current
    ) {
      setFileList([]);
    }
    conversationIdRef.current = conversationId;
  }, [conversationId, setFileList]);

  return (
    <Flex
      gap={1}
      vertical
      className={cn(styles.messageInputWrapper, 'dark:bg-black')}
    >
      <TextArea
        size="large"
        placeholder={t('sendPlaceholder')}
        value={value}
        allowClear
        disabled={disabled}
        style={{
          border: 'none',
          boxShadow: 'none',
          padding: '0px 10px',
          marginTop: 10,
        }}
        autoSize={{ minRows: 2, maxRows: 10 }}
        onKeyDown={handleKeyDown}
        onChange={onInputChange}
      />
      <Divider style={{ margin: '5px 30px 10px 0px' }} />
      <Flex justify="space-between" align="center">
        {fileList.length > 0 && (
          <List
            grid={{
              gutter: 16,
              xs: 1,
              sm: 1,
              md: 1,
              lg: 1,
              xl: 2,
              xxl: 4,
            }}
            dataSource={fileList}
            className={styles.listWrapper}
            renderItem={(item) => {
              const id = getFileId(item);
              const documentInfo = getDocumentInfoById(id);
              const fileExtension = getExtension(documentInfo?.name ?? '');
              const fileName = item.originFileObj?.name ?? '';

              return (
                <List.Item>
                  <Card className={styles.documentCard}>
                    <Flex gap={10} align="center">
                      {item.status === 'uploading' ? (
                        <Spin
                          indicator={
                            <LoadingOutlined style={{ fontSize: 24 }} spin />
                          }
                        />
                      ) : item.status === 'error' ? (
                        <InfoCircleOutlined size={30}></InfoCircleOutlined>
                      ) : (
                        <FileIcon id={id} name={fileName}></FileIcon>
                      )}
                      <Flex vertical style={{ width: '90%' }}>
                        <Text
                          ellipsis={{ tooltip: fileName }}
                          className={styles.nameText}
                        >
                          <b> {fileName}</b>
                        </Text>
                        {item.status === 'error' ? (
                          t('uploadFailed')
                        ) : (
                          <>
                            {item.percent !== 100 ? (
                              t('uploading')
                            ) : !item.response ? (
                              t('parsing')
                            ) : (
                              <Space>
                                <span>{fileExtension?.toUpperCase()},</span>
                                <span>
                                  {formatBytes(
                                    getDocumentInfoById(id)?.size ?? 0,
                                  )}
                                </span>
                              </Space>
                            )}
                          </>
                        )}
                      </Flex>
                    </Flex>

                    {item.status !== 'uploading' && (
                      <span className={styles.deleteIcon}>
                        <CloseCircleOutlined
                          onClick={() => handleRemove(item)}
                        />
                      </span>
                    )}
                  </Card>
                </List.Item>
              );
            }}
          />
        )}
        <Flex
          gap={5}
          align="center"
          justify="flex-end"
          style={{
            paddingRight: 10,
            paddingBottom: 10,
            width: fileList.length > 0 ? '50%' : '100%',
          }}
        >
          {showUploadIcon && (
            <Upload
              onPreview={handlePreview}
              onChange={handleChange}
              multiple={false}
              onRemove={handleRemove}
              showUploadList={false}
              beforeUpload={() => {
                return false;
              }}
            >
              <Button type={'primary'} disabled={disabled}>
                <Paperclip className="size-4" />
              </Button>
            </Upload>
          )}
          {sendLoading ? (
            <Button onClick={handleStopOutputMessage}>
              <CircleStop className="size-5" />
            </Button>
          ) : (
            <Button
              type="primary"
              onClick={handlePressEnter}
              loading={sendLoading}
              disabled={sendDisabled || isUploadingFile || sendLoading}
            >
              <SendHorizontal className="size-5" />
            </Button>
          )}
        </Flex>
      </Flex>
    </Flex>
  );
};

export default memo(MessageInput);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/message-input/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 373 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (18)

- `getFileId()`: Function definition
- `getFileIds()`: Function definition
- `ids()`: Function definition
- `isUploadSuccess()`: Function definition
- `getBase64()`: Function definition
- `MessageInput()`: Function definition
- `handlePreview()`: Function definition
- `ret()`: Function definition
- `nextList()`: Function definition
- `isUploadingFile()`: Function definition
- `handlePressEnter()`: Function definition
- `ids()`: Function definition
- `handleKeyDown()`: Function definition
- `handleRemove()`: Function definition
- `ids()`: Function definition
- `handleStopOutputMessage()`: Function definition
- `getDocumentInfoById()`: Function definition
- `ids()`: Function definition

### Imports (13)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { getExtension } from '@/utils/document-util';`
- `import { formatBytes } from '@/utils/file-util';`
- `import {`
- `import type { GetProp, UploadFile } from 'antd';`
- `import {`
- `import get from 'lodash/get';`
- `import { CircleStop, Paperclip, SendHorizontal } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 373
- Blank lines: 27 (7.2%)
- Comment lines: ~4 (1.1%)
- Code lines: ~342


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/lib/utils`
- `@/utils/document-util`
- `@/utils/file-util`
- `antd`
- `lodash/get`
- `lucide-react`
- `../file-icon`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/message-input`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/message-input/` directory
- Potential test file: `test_index.tsx`

## Keywords

../file-icon, ./index.less, @/hooks/common-hooks, @/lib/utils, @/utils/document-util, @/utils/file-util, Array, Button, Card, ChangeEventHandler, CircleStop, CloseCircleOutlined, Divider, Enter, FileIcon, FileReader, FileType, Flex, GetProp, HTMLTextAreaElement, IProps, InfoCircleOutlined, Input, Item, KeyboardEvent, List, LoadingOutlined, MessageInput, Paperclip, Parameters, Promise, React, SendHorizontal, Space, Spin, Successfully, Text, TextArea, TypeScript, Typography, Upload, UploadFile, UploadProps, ant, antd, code, conversationIdRef, creatingRet, documentInfo, fileExtension...

---
*Generated by RAGFlow Repository Documentation Generator*
