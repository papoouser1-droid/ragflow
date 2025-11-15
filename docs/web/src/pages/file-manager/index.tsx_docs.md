# Documentation: web/src/pages/file-manager/index.tsx

## File Metadata

- **Path**: `web/src/pages/file-manager/index.tsx`
- **Size**: 6393 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/file-manager/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/file-manager/index.tsx` is located in the `web/src/pages/file-manager` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to file-manager.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [file-toolbar.tsx](file-toolbar.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
