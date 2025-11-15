# File Documentation: web/src/components/file-upload-modal/index.tsx

## File Metadata

- **Path**: `web/src/components/file-upload-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 192
- **Characters**: 4,657
- **Size**: 4,657 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { InboxOutlined } from '@ant-design/icons';
import {
  Checkbox,
  Flex,
  Modal,
  Progress,
  Segmented,
  Tabs,
  TabsProps,
  Upload,
  UploadFile,
  UploadProps,
} from 'antd';
import { Dispatch, SetStateAction, useState } from 'react';

import styles from './index.less';

const { Dragger } = Upload;

const FileUpload = ({
  directory,
  fileList,
  setFileList,
  uploadProgress,
}: {
  directory: boolean;
  fileList: UploadFile[];
  setFileList: Dispatch<SetStateAction<UploadFile[]>>;
  uploadProgress?: number;
}) => {
  const { t } = useTranslate('fileManager');
  const props: UploadProps = {
    multiple: true,
    onRemove: (file) => {
      const index = fileList.indexOf(file);
      const newFileList = fileList.slice();
      newFileList.splice(index, 1);
      setFileList(newFileList);
    },
    beforeUpload: (file: UploadFile) => {
      setFileList((pre) => {
        return [...pre, file];
      });

      return false;
    },
    directory,
    fileList,
    progress: {
      strokeWidth: 2,
    },
  };

  return (
    <>
      <Progress percent={uploadProgress} showInfo={false} />
      <Dragger {...props} className={styles.uploader}>
        <p className="ant-upload-drag-icon">
          <InboxOutlined />
        </p>
        <p className="ant-upload-text">{t('uploadTitle')}</p>
        <p className="ant-upload-hint">{t('uploadDescription')}</p>
        {false && <p className={styles.uploadLimit}>{t('uploadLimit')}</p>}
      </Dragger>
    </>
  );
};

interface IFileUploadModalProps
  extends IModalProps<
    { parseOnCreation: boolean; directoryFileList: UploadFile[] } | UploadFile[]
  > {
  uploadFileList?: UploadFile[];
  setUploadFileList?: Dispatch<SetStateAction<UploadFile[]>>;
  uploadProgress?: number;
  setUploadProgress?: Dispatch<SetStateAction<number>>;
}

const FileUploadModal = ({
  visible,
  hideModal,
  loading,
  onOk: onFileUploadOk,
  uploadFileList: fileList,
  setUploadFileList: setFileList,
  uploadProgress,
  setUploadProgress,
}: IFileUploadModalProps) => {
  const { t } = useTranslate('fileManager');
  const [value, setValue] = useState<string | number>('local');
  const [parseOnCreation, setParseOnCreation] = useState(false);
  const [currentFileList, setCurrentFileList] = useState<UploadFile[]>([]);
  const [directoryFileList, setDirectoryFileList] = useState<UploadFile[]>([]);

  const clearFileList = () => {
    if (setFileList) {
      setFileList([]);
      setUploadProgress?.(0);
    } else {
      setCurrentFileList([]);
    }
    setDirectoryFileList([]);
  };

  const onOk = async () => {
    if (uploadProgress === 100) {
      hideModal?.();
      return;
    }

    const ret = await onFileUploadOk?.(
      fileList
        ? { parseOnCreation, directoryFileList }
        : [...currentFileList, ...directoryFileList],
    );
    return ret;
  };

  const afterClose = () => {
    clearFileList();
  };

  const items: TabsProps['items'] = [
    {
      key: '1',
      label: t('file'),
      children: (
        <FileUpload
          directory={false}
          fileList={fileList ? fileList : currentFileList}
          setFileList={setFileList ? setFileList : setCurrentFileList}
          uploadProgress={uploadProgress}
        ></FileUpload>
      ),
    },
    {
      key: '2',
      label: t('directory'),
      children: (
        <FileUpload
          directory
          fileList={directoryFileList}
          setFileList={setDirectoryFileList}
          uploadProgress={uploadProgress}
        ></FileUpload>
      ),
    },
  ];

  return (
    <>
      <Modal
        title={t('uploadFile')}
        open={visible}
        onOk={onOk}
        onCancel={hideModal}
        confirmLoading={loading}
        afterClose={afterClose}
      >
        <Flex gap={'large'} vertical>
          <Segmented
            options={[
              { label: t('local'), value: 'local' },
              { label: t('s3'), value: 's3' },
            ]}
            block
            value={value}
            onChange={setValue}
          />
          {value === 'local' ? (
            <>
              <Checkbox
                checked={parseOnCreation}
                onChange={(e) => setParseOnCreation(e.target.checked)}
              >
                {t('parseOnCreation')}
              </Checkbox>
              <Tabs defaultActiveKey="1" items={items} />
            </>
          ) : (
            t('comingSoon', { keyPrefix: 'common' })
          )}
        </Flex>
      </Modal>
    </>
  );
};

export default FileUploadModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/file-upload-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 192 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (6)

- `FileUpload()`: Function definition
- `newFileList()`: Function definition
- `FileUploadModal()`: Function definition
- `clearFileList()`: Function definition
- `onOk()`: Function definition
- `afterClose()`: Function definition

### Imports (6)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { InboxOutlined } from '@ant-design/icons';`
- `import {`
- `import { Dispatch, SetStateAction, useState } from 'react';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 192
- Blank lines: 15 (7.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~177


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@ant-design/icons`
- `react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/file-upload-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/file-upload-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

./index.less, @/hooks/common-hooks, @/interfaces/common, @ant-design/icons, Checkbox, Dispatch, Dragger, FileUpload, FileUploadModal, Flex, IFileUploadModalProps, IModalProps, InboxOutlined, Modal, Progress, Segmented, SetStateAction, Tabs, TabsProps, TypeScript, Upload, UploadFile, UploadProps, afterClose, ant, clearFileList, index, items, newFileList, onOk, props, react, ret

---
*Generated by RAGFlow Repository Documentation Generator*
