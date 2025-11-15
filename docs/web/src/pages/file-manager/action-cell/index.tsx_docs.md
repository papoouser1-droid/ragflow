# Documentation: web/src/pages/file-manager/action-cell/index.tsx

## File Metadata

- **Path**: `web/src/pages/file-manager/action-cell/index.tsx`
- **Size**: 3832 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/file-manager/action-cell/index.tsx`.

## Original Source Code

```tsx
import NewDocumentLink from '@/components/new-document-link';
import { useTranslate } from '@/hooks/common-hooks';
import { useDownloadFile } from '@/hooks/file-manager-hooks';
import { IFile } from '@/interfaces/database/file-manager';
import {
  getExtension,
  isSupportedPreviewDocumentType,
} from '@/utils/document-util';
import {
  DownloadOutlined,
  EditOutlined,
  EyeOutlined,
  LinkOutlined,
} from '@ant-design/icons';
import { Button, Space, Tooltip } from 'antd';
import { FolderInput, Trash2 } from 'lucide-react';
import { useHandleDeleteFile } from '../hooks';

interface IProps {
  record: IFile;
  setCurrentRecord: (record: any) => void;
  showRenameModal: (record: IFile) => void;
  showMoveFileModal: (ids: string[]) => void;
  showConnectToKnowledgeModal: (record: IFile) => void;
  setSelectedRowKeys(keys: string[]): void;
}

const ActionCell = ({
  record,
  setCurrentRecord,
  showRenameModal,
  showConnectToKnowledgeModal,
  setSelectedRowKeys,
  showMoveFileModal,
}: IProps) => {
  const documentId = record.id;
  const beingUsed = false;
  const { t } = useTranslate('fileManager');
  const { handleRemoveFile } = useHandleDeleteFile(
    [documentId],
    setSelectedRowKeys,
  );
  const { downloadFile, loading } = useDownloadFile();
  const extension = getExtension(record.name);
  const isKnowledgeBase = record.source_type === 'knowledgebase';

  const onDownloadDocument = () => {
    downloadFile({
      id: documentId,
      filename: record.name,
    });
  };

  const setRecord = () => {
    setCurrentRecord(record);
  };

  const onShowRenameModal = () => {
    setRecord();
    showRenameModal(record);
  };

  const onShowConnectToKnowledgeModal = () => {
    showConnectToKnowledgeModal(record);
  };

  const onShowMoveFileModal = () => {
    showMoveFileModal([documentId]);
  };

  return (
    <Space size={0}>
      {isKnowledgeBase || (
        <Tooltip title={t('addToKnowledge')}>
          <Button type="text" onClick={onShowConnectToKnowledgeModal}>
            <LinkOutlined size={20} />
          </Button>
        </Tooltip>
      )}

      {isKnowledgeBase || (
        <Tooltip title={t('rename', { keyPrefix: 'common' })}>
          <Button type="text" disabled={beingUsed} onClick={onShowRenameModal}>
            <EditOutlined size={20} />
          </Button>
        </Tooltip>
      )}
      {isKnowledgeBase || (
        <Tooltip title={t('move', { keyPrefix: 'common' })}>
          <Button
            type="text"
            disabled={beingUsed}
            onClick={onShowMoveFileModal}
            className="flex items-end"
          >
            <FolderInput className="size-4" />
          </Button>
        </Tooltip>
      )}
      {isKnowledgeBase || (
        <Tooltip title={t('delete', { keyPrefix: 'common' })}>
          <Button
            type="text"
            disabled={beingUsed}
            onClick={handleRemoveFile}
            className="flex items-end"
          >
            <Trash2 className="size-4" />
          </Button>
        </Tooltip>
      )}
      {record.type !== 'folder' && (
        <Tooltip title={t('download', { keyPrefix: 'common' })}>
          <Button
            type="text"
            disabled={beingUsed}
            loading={loading}
            onClick={onDownloadDocument}
          >
            <DownloadOutlined size={20} />
          </Button>
        </Tooltip>
      )}
      {isSupportedPreviewDocumentType(extension) && (
        <NewDocumentLink
          documentId={documentId}
          documentName={record.name}
          color="black"
        >
          <Tooltip title={t('preview')}>
            <Button type="text">
              <EyeOutlined size={20} />
            </Button>
          </Tooltip>
        </NewDocumentLink>
      )}
    </Space>
  );
};

export default ActionCell;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/file-manager/action-cell/index.tsx` is located in the `web/src/pages/file-manager/action-cell` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to action-cell.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
