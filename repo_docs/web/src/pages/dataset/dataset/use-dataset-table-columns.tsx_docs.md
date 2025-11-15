# File Documentation: web/src/pages/dataset/dataset/use-dataset-table-columns.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/use-dataset-table-columns.tsx`
- **Extension**: `.tsx`
- **Lines**: 202
- **Characters**: 5,976
- **Size**: 5,976 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FileIcon } from '@/components/icon-font';
import { Button } from '@/components/ui/button';
import { Checkbox } from '@/components/ui/checkbox';
import { Switch } from '@/components/ui/switch';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { useSetDocumentStatus } from '@/hooks/use-document-request';
import { IDocumentInfo } from '@/interfaces/database/document';
import { cn } from '@/lib/utils';
import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';
import { formatDate } from '@/utils/date';
import { ColumnDef } from '@tanstack/table-core';
import { ArrowUpDown, MonitorUp } from 'lucide-react';
import { useTranslation } from 'react-i18next';
import { DatasetActionCell } from './dataset-action-cell';
import { ParsingStatusCell } from './parsing-status-cell';
import { UseChangeDocumentParserShowType } from './use-change-document-parser';
import { UseRenameDocumentShowType } from './use-rename-document';
import { UseSaveMetaShowType } from './use-save-meta';

type UseDatasetTableColumnsType = UseChangeDocumentParserShowType &
  UseRenameDocumentShowType &
  UseSaveMetaShowType & { showLog: (record: IDocumentInfo) => void };

export function useDatasetTableColumns({
  showChangeParserModal,
  showRenameModal,
  showSetMetaModal,
  showLog,
}: UseDatasetTableColumnsType) {
  const { t } = useTranslation('translation', {
    keyPrefix: 'knowledgeDetails',
  });

  const { navigateToChunkParsedResult } = useNavigatePage();
  const { setDocumentStatus } = useSetDocumentStatus();

  const columns: ColumnDef<IDocumentInfo>[] = [
    {
      id: 'select',
      header: ({ table }) => (
        <Checkbox
          checked={
            table.getIsAllPageRowsSelected() ||
            (table.getIsSomePageRowsSelected() && 'indeterminate')
          }
          onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
          aria-label="Select all"
        />
      ),
      cell: ({ row }) => (
        <Checkbox
          checked={row.getIsSelected()}
          onCheckedChange={(value) => row.toggleSelected(!!value)}
          aria-label="Select row"
        />
      ),
      enableSorting: false,
      enableHiding: false,
    },
    {
      accessorKey: 'name',
      header: ({ column }) => {
        return (
          <Button
            variant="transparent"
            className="border-none"
            onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
          >
            {t('name')}
            <ArrowUpDown />
          </Button>
        );
      },
      meta: { cellClassName: 'max-w-[20vw]' },
      cell: ({ row }) => {
        const name: string = row.getValue('name');

        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div
                className="flex gap-2 cursor-pointer"
                onClick={navigateToChunkParsedResult(
                  row.original.id,
                  row.original.kb_id,
                )}
              >
                <FileIcon name={name}></FileIcon>
                <span className={cn('truncate')}>{name}</span>
              </div>
            </TooltipTrigger>
            <TooltipContent>
              <p>{name}</p>
            </TooltipContent>
          </Tooltip>
        );
      },
    },
    {
      accessorKey: 'create_time',
      header: ({ column }) => {
        return (
          <Button
            variant="transparent"
            className="border-none"
            onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
          >
            {t('uploadDate')}
            <ArrowUpDown />
          </Button>
        );
      },
      cell: ({ row }) => (
        <div className="lowercase">
          {formatDate(row.getValue('create_time'))}
        </div>
      ),
    },
    {
      accessorKey: 'source_from',
      header: t('source'),
      cell: ({ row }) => (
        <div className="text-text-primary">
          {row.original.source_type === 'local' ||
          row.original.source_type === '' ? (
            <div className="bg-accent-primary-5 w-6 h-6 rounded-full flex items-center justify-center">
              <MonitorUp className="text-accent-primary" size={16} />
            </div>
          ) : (
            <div className="w-6 h-6 flex items-center justify-center">
              {
                DataSourceInfo[
                  row.original.source_type as keyof typeof DataSourceInfo
                ].icon
              }
            </div>
          )}
        </div>
      ),
    },
    {
      accessorKey: 'status',
      header: t('enabled'),
      cell: ({ row }) => {
        const id = row.original.id;
        return (
          <Switch
            checked={row.getValue('status') === '1'}
            onCheckedChange={(e) => {
              setDocumentStatus({ status: e, documentId: id });
            }}
          />
        );
      },
    },
    {
      accessorKey: 'chunk_num',
      header: t('chunkNumber'),
      cell: ({ row }) => (
        <div className="capitalize">{row.getValue('chunk_num')}</div>
      ),
    },
    {
      accessorKey: 'run',
      header: t('Parse'),
      // meta: { cellClassName: 'min-w-[20vw]' },
      cell: ({ row }) => {
        return (
          <ParsingStatusCell
            record={row.original}
            showChangeParserModal={showChangeParserModal}
            showSetMetaModal={showSetMetaModal}
            showLog={showLog}
          ></ParsingStatusCell>
        );
      },
    },
    {
      id: 'actions',
      header: t('action'),
      enableHiding: false,
      cell: ({ row }) => {
        const record = row.original;

        return (
          <DatasetActionCell
            record={record}
            showRenameModal={showRenameModal}
          ></DatasetActionCell>
        );
      },
    },
  ];

  return columns;
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/use-dataset-table-columns.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 202 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useDatasetTableColumns`: Exported entity

### Functions (1)

- `useDatasetTableColumns()`: Function definition

### Imports (19)

- `import { FileIcon } from '@/components/icon-font';`
- `import { Button } from '@/components/ui/button';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import { Switch } from '@/components/ui/switch';`
- `import {`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { useSetDocumentStatus } from '@/hooks/use-document-request';`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import { cn } from '@/lib/utils';`
- `import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';`

## Code Structure Analysis

- Total lines: 202
- Blank lines: 8 (4.0%)
- Comment lines: ~1 (0.5%)
- Code lines: ~193


## Dependencies and Imports

- `@/components/icon-font`
- `@/components/ui/button`
- `@/components/ui/checkbox`
- `@/components/ui/switch`
- `@/hooks/logic-hooks/navigate-hooks`
- `@/hooks/use-document-request`
- `@/interfaces/database/document`
- `@/lib/utils`
- `@/pages/user-setting/data-source/contant`
- `@/utils/date`
- `@tanstack/table-core`
- `lucide-react`
- `react-i18next`
- `./dataset-action-cell`
- `./parsing-status-cell`
- `./use-change-document-parser`
- `./use-rename-document`
- `./use-save-meta`

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
- Potential test file: `test_use-dataset-table-columns.tsx`

## Keywords

./dataset-action-cell, ./parsing-status-cell, ./use-change-document-parser, ./use-rename-document, ./use-save-meta, @/components/icon-font, @/components/ui/button, @/components/ui/checkbox, @/components/ui/switch, @/hooks/logic-hooks/navigate-hooks, @/hooks/use-document-request, @/interfaces/database/document, @/lib/utils, @/pages/user-setting/data-source/contant, @/utils/date, @tanstack/table-core, ArrowUpDown, Button, Checkbox, ColumnDef, DataSourceInfo, DatasetActionCell, FileIcon, IDocumentInfo, MonitorUp, Parse, ParsingStatusCell, Select, Switch, Tooltip, TooltipContent, TooltipTrigger, TypeScript, UseChangeDocumentParserShowType, UseDatasetTableColumnsType, UseRenameDocumentShowType, UseSaveMetaShowType, columns, id, lucide-react, name, react-i18next, record, tanstack, useDatasetTableColumns

---
*Generated by RAGFlow Repository Documentation Generator*
