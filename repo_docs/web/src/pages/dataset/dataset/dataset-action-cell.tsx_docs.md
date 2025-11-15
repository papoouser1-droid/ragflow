# File Documentation: web/src/pages/dataset/dataset/dataset-action-cell.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/dataset-action-cell.tsx`
- **Extension**: `.tsx`
- **Lines**: 119
- **Characters**: 3,599
- **Size**: 3,599 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import { Button } from '@/components/ui/button';
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/components/ui/hover-card';
import { DocumentType } from '@/constants/knowledge';
import { useRemoveDocument } from '@/hooks/use-document-request';
import { IDocumentInfo } from '@/interfaces/database/document';
import { formatFileSize } from '@/utils/common-util';
import { formatDate } from '@/utils/date';
import { downloadDocument } from '@/utils/file-util';
import { Download, Eye, PenLine, Trash2 } from 'lucide-react';
import { useCallback } from 'react';
import { UseRenameDocumentShowType } from './use-rename-document';
import { isParserRunning } from './utils';

const Fields = ['name', 'size', 'type', 'create_time', 'update_time'];

const FunctionMap = {
  size: formatFileSize,
  create_time: formatDate,
  update_time: formatDate,
};

export function DatasetActionCell({
  record,
  showRenameModal,
}: { record: IDocumentInfo } & UseRenameDocumentShowType) {
  const { id, run, type } = record;
  const isRunning = isParserRunning(run);
  const isVirtualDocument = type === DocumentType.Virtual;

  const { removeDocument } = useRemoveDocument();

  const onDownloadDocument = useCallback(() => {
    downloadDocument({
      id,
      filename: record.name,
    });
  }, [id, record.name]);

  const handleRemove = useCallback(() => {
    removeDocument(id);
  }, [id, removeDocument]);

  const handleRename = useCallback(() => {
    showRenameModal(record);
  }, [record, showRenameModal]);

  return (
    <section className="flex gap-4 items-center text-text-sub-title-invert opacity-0 group-hover:opacity-100 transition-opacity">
      <Button
        variant="transparent"
        className="border-none hover:bg-bg-card text-text-primary"
        size={'sm'}
        disabled={isRunning}
        onClick={handleRename}
      >
        <PenLine />
      </Button>
      <HoverCard>
        <HoverCardTrigger>
          <Button
            variant="transparent"
            className="border-none hover:bg-bg-card text-text-primary"
            disabled={isRunning}
            size={'sm'}
          >
            <Eye />
          </Button>
        </HoverCardTrigger>
        <HoverCardContent className="w-[40vw] max-h-[40vh] overflow-auto">
          <ul className="space-y-2">
            {Object.entries(record)
              .filter(([key]) => Fields.some((x) => x === key))

              .map(([key, value], idx) => {
                return (
                  <li key={idx} className="flex gap-2">
                    {key}:
                    <div>
                      {key in FunctionMap
                        ? FunctionMap[key as keyof typeof FunctionMap](value)
                        : value}
                    </div>
                  </li>
                );
              })}
          </ul>
        </HoverCardContent>
      </HoverCard>

      {isVirtualDocument || (
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          onClick={onDownloadDocument}
          disabled={isRunning}
          size={'sm'}
        >
          <Download />
        </Button>
      )}
      <ConfirmDeleteDialog onOk={handleRemove}>
        <Button
          variant="transparent"
          className="border-none hover:bg-bg-card text-text-primary"
          size={'sm'}
          disabled={isRunning}
        >
          <Trash2 />
        </Button>
      </ConfirmDeleteDialog>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/dataset-action-cell.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 119 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `DatasetActionCell`: Exported entity

### Functions (4)

- `DatasetActionCell()`: Function definition
- `onDownloadDocument()`: Function definition
- `handleRemove()`: Function definition
- `handleRename()`: Function definition

### Imports (13)

- `import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { DocumentType } from '@/constants/knowledge';`
- `import { useRemoveDocument } from '@/hooks/use-document-request';`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import { formatFileSize } from '@/utils/common-util';`
- `import { formatDate } from '@/utils/date';`
- `import { downloadDocument } from '@/utils/file-util';`
- `import { Download, Eye, PenLine, Trash2 } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 119
- Blank lines: 11 (9.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~108


## Dependencies and Imports

- `@/components/confirm-delete-dialog`
- `@/components/ui/button`
- `@/constants/knowledge`
- `@/hooks/use-document-request`
- `@/interfaces/database/document`
- `@/utils/common-util`
- `@/utils/date`
- `@/utils/file-util`
- `lucide-react`
- `react`
- `./use-rename-document`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset`.

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

- Other files in `web/src/pages/dataset/dataset/` directory
- Potential test file: `test_dataset-action-cell.tsx`

## Keywords

./use-rename-document, ./utils, @/components/confirm-delete-dialog, @/components/ui/button, @/constants/knowledge, @/hooks/use-document-request, @/interfaces/database/document, @/utils/common-util, @/utils/date, @/utils/file-util, Button, ConfirmDeleteDialog, DatasetActionCell, DocumentType, Download, Eye, Fields, FunctionMap, HoverCard, HoverCardContent, HoverCardTrigger, IDocumentInfo, Object, PenLine, Trash2, TypeScript, UseRenameDocumentShowType, Virtual, handleRemove, handleRename, isRunning, isVirtualDocument, lucide-react, onDownloadDocument, react

---
*Generated by RAGFlow Repository Documentation Generator*
