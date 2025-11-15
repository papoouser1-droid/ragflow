# Documentation: web/src/pages/dataset/dataset/use-dataset-table-columns.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/use-dataset-table-columns.tsx`
- **Size**: 5976 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset/use-dataset-table-columns.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset/use-dataset-table-columns.tsx` is located in the `web/src/pages/dataset/dataset` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset.

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

- [constant.ts](constant.ts_docs.md)
- [dataset-action-cell.tsx](dataset-action-cell.tsx_docs.md)
- [dataset-table.tsx](dataset-table.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [parsing-card.tsx](parsing-card.tsx_docs.md)
- [parsing-status-cell.tsx](parsing-status-cell.tsx_docs.md)
- [set-meta-dialog.tsx](set-meta-dialog.tsx_docs.md)
- [use-bulk-operate-dataset.tsx](use-bulk-operate-dataset.tsx_docs.md)
- [use-change-document-parser.ts](use-change-document-parser.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
