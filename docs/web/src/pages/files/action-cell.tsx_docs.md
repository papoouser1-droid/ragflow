# Documentation: web/src/pages/files/action-cell.tsx

## File Metadata

- **Path**: `web/src/pages/files/action-cell.tsx`
- **Size**: 5089 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/files/action-cell.tsx`.

## Original Source Code

```tsx
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import NewDocumentLink from '@/components/new-document-link';
import { Button } from '@/components/ui/button';
import { useDownloadFile } from '@/hooks/file-manager-hooks';
import { IFile } from '@/interfaces/database/file-manager';
import {
  getExtension,
  isSupportedPreviewDocumentType,
} from '@/utils/document-util';
import { CellContext } from '@tanstack/react-table';
import {
  ArrowDownToLine,
  Eye,
  FolderInput,
  FolderPen,
  Link2,
  Trash2,
} from 'lucide-react';
import { useCallback } from 'react';
import {
  UseHandleConnectToKnowledgeReturnType,
  UseRenameCurrentFileReturnType,
} from './hooks';
import { useHandleDeleteFile } from './use-delete-file';
import { UseMoveDocumentShowType } from './use-move-file';
import { isFolderType, isKnowledgeBaseType } from './util';

type IProps = Pick<CellContext<IFile, unknown>, 'row'> &
  Pick<UseHandleConnectToKnowledgeReturnType, 'showConnectToKnowledgeModal'> &
  Pick<UseRenameCurrentFileReturnType, 'showFileRenameModal'> &
  UseMoveDocumentShowType;

export function ActionCell({
  row,
  showConnectToKnowledgeModal,
  showFileRenameModal,
  showMoveFileModal,
}: IProps) {
  const record = row.original;
  const documentId = record.id;
  const { downloadFile } = useDownloadFile();
  const isFolder = isFolderType(record.type);
  const extension = getExtension(record.name);
  const isKnowledgeBase = isKnowledgeBaseType(record.source_type);

  const handleShowConnectToKnowledgeModal = useCallback(() => {
    showConnectToKnowledgeModal(record);
  }, [record, showConnectToKnowledgeModal]);

  const onDownloadDocument = useCallback(() => {
    downloadFile({
      id: documentId,
      filename: record.name,
    });
  }, [documentId, downloadFile, record.name]);

  const handleShowFileRenameModal = useCallback(() => {
    showFileRenameModal(record);
  }, [record, showFileRenameModal]);

  const handleShowMoveFileModal = useCallback(() => {
    showMoveFileModal([record.id]);
  }, [record, showMoveFileModal]);

  const { handleRemoveFile } = useHandleDeleteFile();

  const onRemoveFile = useCallback(() => {
    handleRemoveFile([documentId]);
  }, [handleRemoveFile, documentId]);

  return (
    <section className="flex gap-4 items-center text-text-sub-title-invert opacity-0 group-hover:opacity-100 transition-opacity">
      {isKnowledgeBase || (
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          size={'sm'}
          onClick={handleShowConnectToKnowledgeModal}
        >
          <Link2 />
        </Button>
      )}
      {isKnowledgeBase || (
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          size={'sm'}
          onClick={handleShowMoveFileModal}
        >
          <FolderInput />
        </Button>
      )}
      {isKnowledgeBase || (
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          size={'sm'}
          onClick={handleShowFileRenameModal}
        >
          <FolderPen />
        </Button>
      )}
      {isFolder || (
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          size={'sm'}
          onClick={onDownloadDocument}
        >
          <ArrowDownToLine />
        </Button>
      )}

      {isSupportedPreviewDocumentType(extension) && (
        <NewDocumentLink
          documentId={documentId}
          documentName={record.name}
          className="text-text-sub-title-invert"
        >
          <Button
            variant="transparent"
            className="border-none hover:bg-bg-card text-text-primary"
            size={'sm'}
          >
            <Eye />
          </Button>
        </NewDocumentLink>
      )}

      {/* <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <Button variant="transparent"
        className="border-none" size={'sm'}>
            <EllipsisVertical />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="end">
          <DropdownMenuItem onClick={handleShowMoveFileModal}>
            {t('common.move')}
          </DropdownMenuItem>
          <DropdownMenuSeparator />
          <DropdownMenuItem onClick={handleShowFileRenameModal}>
            {t('common.rename')}
          </DropdownMenuItem>
          <DropdownMenuSeparator />
          {isFolder || (
            <DropdownMenuItem onClick={onDownloadDocument}>
              {t('common.download')}
            </DropdownMenuItem>
          )}
        </DropdownMenuContent>
      </DropdownMenu> */}
      {isKnowledgeBase || (
        <ConfirmDeleteDialog onOk={onRemoveFile}>
          <Button
            variant="transparent"
            className="border-none hover:bg-bg-card text-text-primary"
            size={'sm'}
          >
            <Trash2 />
          </Button>
        </ConfirmDeleteDialog>
      )}
    </section>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/files/action-cell.tsx` is located in the `web/src/pages/files` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to files.

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

- [file-breadcrumb.tsx](file-breadcrumb.tsx_docs.md)
- [files-table.tsx](files-table.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [knowledge-cell.tsx](knowledge-cell.tsx_docs.md)
- [link-to-dataset-dialog.tsx](link-to-dataset-dialog.tsx_docs.md)
- [move-dialog.tsx](move-dialog.tsx_docs.md)
- [use-bulk-operate-file.tsx](use-bulk-operate-file.tsx_docs.md)
- [use-create-folder.ts](use-create-folder.ts_docs.md)
- [use-delete-file.ts](use-delete-file.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
