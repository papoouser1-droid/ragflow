# File Documentation: web/src/pages/dataset/dataset/use-bulk-operate-dataset.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/use-bulk-operate-dataset.tsx`
- **Extension**: `.tsx`
- **Lines**: 132
- **Characters**: 3,358
- **Size**: 3,358 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  UseRowSelectionType,
  useSelectedIds,
} from '@/hooks/logic-hooks/use-row-selection';
import {
  useRemoveDocument,
  useRunDocument,
  useSetDocumentStatus,
} from '@/hooks/use-document-request';
import { IDocumentInfo } from '@/interfaces/database/document';
import { Ban, CircleCheck, CircleX, Play, Trash2 } from 'lucide-react';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { toast } from 'sonner';
import { DocumentType, RunningStatus } from './constant';

export function useBulkOperateDataset({
  rowSelection,
  setRowSelection,
  documents,
}: Pick<UseRowSelectionType, 'rowSelection' | 'setRowSelection'> & {
  documents: IDocumentInfo[];
}) {
  const { t } = useTranslation();
  const { selectedIds: selectedRowKeys } = useSelectedIds(
    rowSelection,
    documents,
  );

  const { runDocumentByIds } = useRunDocument();
  const { setDocumentStatus } = useSetDocumentStatus();
  const { removeDocument } = useRemoveDocument();

  const runDocument = useCallback(
    (run: number) => {
      const nonVirtualKeys = selectedRowKeys.filter(
        (x) =>
          !documents.some((y) => x === y.id && y.type === DocumentType.Virtual),
      );

      if (nonVirtualKeys.length === 0) {
        toast.error(t('Please select a non-empty file list'));
        return;
      }
      runDocumentByIds({
        documentIds: nonVirtualKeys,
        run,
        shouldDelete: false,
      });
    },
    [documents, runDocumentByIds, selectedRowKeys, t],
  );

  const handleRunClick = useCallback(() => {
    runDocument(1);
  }, [runDocument]);

  const handleCancelClick = useCallback(() => {
    runDocument(2);
  }, [runDocument]);

  const onChangeStatus = useCallback(
    (enabled: boolean) => {
      setDocumentStatus({ status: enabled, documentId: selectedRowKeys });
    },
    [selectedRowKeys, setDocumentStatus],
  );

  const handleEnableClick = useCallback(() => {
    onChangeStatus(true);
  }, [onChangeStatus]);

  const handleDisableClick = useCallback(() => {
    onChangeStatus(false);
  }, [onChangeStatus]);

  const handleDelete = useCallback(() => {
    const deletedKeys = selectedRowKeys.filter(
      (x) =>
        !documents
          .filter((y) => y.run === RunningStatus.RUNNING)
          .some((y) => y.id === x),
    );
    if (deletedKeys.length === 0) {
      toast.error(t('theDocumentBeingParsedCannotBeDeleted'));
      return;
    }

    return removeDocument(deletedKeys);
  }, [selectedRowKeys, removeDocument, documents, t]);

  const list = [
    {
      id: 'enabled',
      label: t('knowledgeDetails.enabled'),
      icon: <CircleCheck />,
      onClick: handleEnableClick,
    },
    {
      id: 'disabled',
      label: t('knowledgeDetails.disabled'),
      icon: <Ban />,
      onClick: handleDisableClick,
    },
    {
      id: 'run',
      label: t('knowledgeDetails.run'),
      icon: <Play />,
      onClick: handleRunClick,
    },
    {
      id: 'cancel',
      label: t('knowledgeDetails.cancel'),
      icon: <CircleX />,
      onClick: handleCancelClick,
    },
    {
      id: 'delete',
      label: t('common.delete'),
      icon: <Trash2 />,
      onClick: async () => {
        const code = await handleDelete();
        if (code === 0) {
          setRowSelection({});
        }
      },
    },
  ];

  return { list };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/use-bulk-operate-dataset.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 132 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useBulkOperateDataset`: Exported entity

### Functions (11)

- `useBulkOperateDataset()`: Function definition
- `runDocument()`: Function definition
- `nonVirtualKeys()`: Function definition
- `handleRunClick()`: Function definition
- `handleCancelClick()`: Function definition
- `onChangeStatus()`: Function definition
- `handleEnableClick()`: Function definition
- `handleDisableClick()`: Function definition
- `handleDelete()`: Function definition
- `deletedKeys()`: Function definition
- `list()`: Function definition

### Imports (8)

- `import {`
- `import {`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import { Ban, CircleCheck, CircleX, Play, Trash2 } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { toast } from 'sonner';`
- `import { DocumentType, RunningStatus } from './constant';`

## Code Structure Analysis

- Total lines: 132
- Blank lines: 14 (10.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~118


## Dependencies and Imports

- `@/interfaces/database/document`
- `lucide-react`
- `react`
- `react-i18next`
- `sonner`
- `./constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset/` directory
- Potential test file: `test_use-bulk-operate-dataset.tsx`

## Keywords

./constant, @/interfaces/database/document, Ban, CircleCheck, CircleX, DocumentType, IDocumentInfo, Pick, Play, Please, RUNNING, RunningStatus, Trash2, TypeScript, UseRowSelectionType, Virtual, code, deletedKeys, handleCancelClick, handleDelete, handleDisableClick, handleEnableClick, handleRunClick, list, lucide-react, nonVirtualKeys, onChangeStatus, react, react-i18next, runDocument, sonner, useBulkOperateDataset

---
*Generated by RAGFlow Repository Documentation Generator*
