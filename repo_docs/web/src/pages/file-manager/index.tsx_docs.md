# File Documentation: web/src/pages/file-manager/index.tsx

## File Metadata

- **Path**: `web/src/pages/file-manager/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 217
- **Characters**: 6,393
- **Size**: 6,393 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useFetchFileList } from '@/hooks/file-manager-hooks';
import { IFile } from '@/interfaces/database/file-manager';
import { formatDate } from '@/utils/date';
import { Button, Flex, Space, Table, Tag, Typography } from 'antd';
import { ColumnsType } from 'antd/es/table';
import ActionCell from './action-cell';
import FileToolbar from './file-toolbar';
import {
  useGetRowSelection,
  useHandleConnectToKnowledge,
  useHandleCreateFolder,
  useHandleMoveFile,
  useHandleUploadFile,
  useNavigateToOtherFolder,
  useRenameCurrentFile,
} from './hooks';

import FileUploadModal from '@/components/file-upload-modal';
import RenameModal from '@/components/rename-modal';
import SvgIcon from '@/components/svg-icon';
import { useTranslate } from '@/hooks/common-hooks';
import { formatNumberWithThousandsSeparator } from '@/utils/common-util';
import { getExtension } from '@/utils/document-util';
import ConnectToKnowledgeModal from './connect-to-knowledge-modal';
import FolderCreateModal from './folder-create-modal';
import styles from './index.less';
import FileMovingModal from './move-file-modal';

const { Text } = Typography;

const FileManager = () => {
  const { t } = useTranslate('fileManager');
  // const fileList = useSelectFileList();
  const { rowSelection, setSelectedRowKeys } = useGetRowSelection();
  const navigateToOtherFolder = useNavigateToOtherFolder();
  const {
    fileRenameVisible,
    fileRenameLoading,
    hideFileRenameModal,
    showFileRenameModal,
    initialFileName,
    onFileRenameOk,
  } = useRenameCurrentFile();
  const {
    folderCreateModalVisible,
    showFolderCreateModal,
    hideFolderCreateModal,
    folderCreateLoading,
    onFolderCreateOk,
  } = useHandleCreateFolder();
  const {
    fileUploadVisible,
    hideFileUploadModal,
    showFileUploadModal,
    fileUploadLoading,
    onFileUploadOk,
  } = useHandleUploadFile();
  const {
    connectToKnowledgeVisible,
    hideConnectToKnowledgeModal,
    showConnectToKnowledgeModal,
    onConnectToKnowledgeOk,
    initialValue,
    connectToKnowledgeLoading,
  } = useHandleConnectToKnowledge();
  const {
    showMoveFileModal,
    moveFileVisible,
    onMoveFileOk,
    hideMoveFileModal,
    moveFileLoading,
  } = useHandleMoveFile(setSelectedRowKeys);
  const { pagination, data, searchString, handleInputChange, loading } =
    useFetchFileList();
  const columns: ColumnsType<IFile> = [
    {
      title: t('name'),
      dataIndex: 'name',
      key: 'name',
      fixed: 'left',
      render(value, record) {
        return (
          <Flex gap={10} align="center">
            <SvgIcon
              name={`file-icon/${record.type === 'folder' ? 'folder' : getExtension(value)}`}
              width={24}
            ></SvgIcon>
            {record.type === 'folder' ? (
              <Button
                type={'link'}
                className={styles.linkButton}
                onClick={() => navigateToOtherFolder(record.id)}
              >
                <Text ellipsis={{ tooltip: value }}>{value}</Text>
              </Button>
            ) : (
              <Text ellipsis={{ tooltip: value }}>{value}</Text>
            )}
          </Flex>
        );
      },
    },
    {
      title: t('uploadDate'),
      dataIndex: 'create_time',
      key: 'create_time',
      render(text) {
        return formatDate(text);
      },
    },
    {
      title: t('size'),
      dataIndex: 'size',
      key: 'size',
      render(value) {
        return (
          formatNumberWithThousandsSeparator((value / 1024).toFixed(2)) + ' KB'
        );
      },
    },
    {
      title: t('knowledgeBase'),
      dataIndex: 'kbs_info',
      key: 'kbs_info',
      render(value) {
        return Array.isArray(value) ? (
          <Space wrap>
            {value?.map((x) => (
              <Tag color="blue" key={x.kb_id}>
                {x.kb_name}
              </Tag>
            ))}
          </Space>
        ) : (
          ''
        );
      },
    },
    {
      title: t('action'),
      dataIndex: 'action',
      key: 'action',
      render: (text, record) => (
        <ActionCell
          record={record}
          setCurrentRecord={(record: any) => {
            console.info(record);
          }}
          showRenameModal={showFileRenameModal}
          showMoveFileModal={showMoveFileModal}
          showConnectToKnowledgeModal={showConnectToKnowledgeModal}
          setSelectedRowKeys={setSelectedRowKeys}
        ></ActionCell>
      ),
    },
  ];

  return (
    <section className={styles.fileManagerWrapper}>
      <FileToolbar
        searchString={searchString}
        handleInputChange={handleInputChange}
        selectedRowKeys={rowSelection.selectedRowKeys as string[]}
        showFolderCreateModal={showFolderCreateModal}
        showFileUploadModal={showFileUploadModal}
        setSelectedRowKeys={setSelectedRowKeys}
        showMoveFileModal={showMoveFileModal}
      ></FileToolbar>
      <Table
        dataSource={data?.files}
        columns={columns}
        rowKey={'id'}
        rowSelection={rowSelection}
        loading={loading}
        pagination={pagination}
        scroll={{ scrollToFirstRowOnChange: true, x: '100%' }}
      />
      <RenameModal
        visible={fileRenameVisible}
        hideModal={hideFileRenameModal}
        onOk={onFileRenameOk}
        initialName={initialFileName}
        loading={fileRenameLoading}
      ></RenameModal>
      <FolderCreateModal
        loading={folderCreateLoading}
        visible={folderCreateModalVisible}
        hideModal={hideFolderCreateModal}
        onOk={onFolderCreateOk}
      ></FolderCreateModal>
      <FileUploadModal
        visible={fileUploadVisible}
        hideModal={hideFileUploadModal}
        loading={fileUploadLoading}
        onOk={onFileUploadOk}
      ></FileUploadModal>
      <ConnectToKnowledgeModal
        initialValue={initialValue}
        visible={connectToKnowledgeVisible}
        hideModal={hideConnectToKnowledgeModal}
        onOk={onConnectToKnowledgeOk}
        loading={connectToKnowledgeLoading}
      ></ConnectToKnowledgeModal>
      {moveFileVisible && (
        <FileMovingModal
          visible={moveFileVisible}
          hideModal={hideMoveFileModal}
          onOk={onMoveFileOk}
          loading={moveFileLoading}
        ></FileMovingModal>
      )}
    </section>
  );
};

export default FileManager;

```

## High-Level Overview

  // const fileList = useSelectFileList();

## Detailed Walkthrough


### Functions (1)

- `FileManager()`: Function definition

### Imports (18)

- `import { useFetchFileList } from '@/hooks/file-manager-hooks';`
- `import { IFile } from '@/interfaces/database/file-manager';`
- `import { formatDate } from '@/utils/date';`
- `import { Button, Flex, Space, Table, Tag, Typography } from 'antd';`
- `import { ColumnsType } from 'antd/es/table';`
- `import ActionCell from './action-cell';`
- `import FileToolbar from './file-toolbar';`
- `import {`
- `import FileUploadModal from '@/components/file-upload-modal';`
- `import RenameModal from '@/components/rename-modal';`

## Code Structure Analysis

- Total lines: 217
- Blank lines: 6 (2.8%)
- Comment lines: ~1 (0.5%)
- Code lines: ~210


## Dependencies and Imports

- `@/hooks/file-manager-hooks`
- `@/interfaces/database/file-manager`
- `@/utils/date`
- `antd`
- `antd/es/table`
- `./action-cell`
- `./file-toolbar`
- `@/components/file-upload-modal`
- `@/components/rename-modal`
- `@/components/svg-icon`
- `@/hooks/common-hooks`
- `@/utils/common-util`
- `@/utils/document-util`
- `./connect-to-knowledge-modal`
- `./folder-create-modal`
- `./index.less`
- `./move-file-modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/file-manager`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/file-manager/` directory
- Potential test file: `test_index.tsx`

## Keywords

./action-cell, ./connect-to-knowledge-modal, ./file-toolbar, ./folder-create-modal, ./index.less, ./move-file-modal, @/components/file-upload-modal, @/components/rename-modal, @/components/svg-icon, @/hooks/common-hooks, @/hooks/file-manager-hooks, @/interfaces/database/file-manager, @/utils/common-util, @/utils/date, @/utils/document-util, ActionCell, Array, Button, ColumnsType, ConnectToKnowledgeModal, FileManager, FileMovingModal, FileToolbar, FileUploadModal, Flex, FolderCreateModal, IFile, RenameModal, Space, SvgIcon, Table, Tag, Text, TypeScript, Typography, antd, antd/es/table, columns, fileList, navigateToOtherFolder

---
*Generated by RAGFlow Repository Documentation Generator*
