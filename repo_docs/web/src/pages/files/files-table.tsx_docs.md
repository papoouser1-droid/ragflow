# File Documentation: web/src/pages/files/files-table.tsx

## File Metadata

- **Path**: `web/src/pages/files/files-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 346
- **Characters**: 10,303
- **Size**: 10,303 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import {
  ColumnDef,
  ColumnFiltersState,
  SortingState,
  VisibilityState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';
import { ArrowUpDown } from 'lucide-react';
import * as React from 'react';

import { FileIcon } from '@/components/icon-font';
import { RenameDialog } from '@/components/rename-dialog';
import { TableEmpty, TableSkeleton } from '@/components/table-skeleton';
import { Button } from '@/components/ui/button';
import { Checkbox } from '@/components/ui/checkbox';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { UseRowSelectionType } from '@/hooks/logic-hooks/use-row-selection';
import { useFetchFileList } from '@/hooks/use-file-request';
import { IFile } from '@/interfaces/database/file-manager';
import { cn } from '@/lib/utils';
import { formatFileSize } from '@/utils/common-util';
import { formatDate } from '@/utils/date';
import { pick } from 'lodash';
import { useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { ActionCell } from './action-cell';
import { useHandleConnectToKnowledge, useRenameCurrentFile } from './hooks';
import { KnowledgeCell } from './knowledge-cell';
import { LinkToDatasetDialog } from './link-to-dataset-dialog';
import { UseMoveDocumentShowType } from './use-move-file';
import { useNavigateToOtherFolder } from './use-navigate-to-folder';
import { isFolderType, isKnowledgeBaseType } from './util';

type FilesTableProps = Pick<
  ReturnType<typeof useFetchFileList>,
  'files' | 'loading' | 'pagination' | 'setPagination' | 'total'
> &
  Pick<UseRowSelectionType, 'rowSelection' | 'setRowSelection'> &
  UseMoveDocumentShowType;

export function FilesTable({
  files,
  total,
  pagination,
  setPagination,
  loading,
  rowSelection,
  setRowSelection,
  showMoveFileModal,
}: FilesTableProps) {
  const [sorting, setSorting] = React.useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    [],
  );
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({});
  const { t } = useTranslation('translation', {
    keyPrefix: 'fileManager',
  });
  const navigateToOtherFolder = useNavigateToOtherFolder();
  const {
    connectToKnowledgeVisible,
    hideConnectToKnowledgeModal,
    showConnectToKnowledgeModal,
    initialConnectedIds,
    onConnectToKnowledgeOk,
    connectToKnowledgeLoading,
  } = useHandleConnectToKnowledge();
  const {
    fileRenameVisible,
    showFileRenameModal,
    hideFileRenameModal,
    onFileRenameOk,
    initialFileName,
    fileRenameLoading,
  } = useRenameCurrentFile();

  const columns: ColumnDef<IFile>[] = [
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
          disabled={!row.getCanSelect()}
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
            variant="ghost"
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
        const type = row.original.type;
        const id = row.original.id;
        const isFolder = isFolderType(type);

        const handleNameClick = () => {
          if (isFolder) {
            navigateToOtherFolder(id);
          }
        };

        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div className="flex gap-2">
                <span className="size-4">
                  <FileIcon name={name} type={type}></FileIcon>
                </span>
                <span
                  className={cn('truncate', { ['cursor-pointer']: isFolder })}
                  onClick={handleNameClick}
                >
                  {name}
                </span>
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
            variant="ghost"
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
      accessorKey: 'size',
      header: ({ column }) => {
        return (
          <Button
            variant="ghost"
            onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
          >
            {t('size')}
            <ArrowUpDown />
          </Button>
        );
      },
      cell: ({ row }) => (
        <div className="capitalize">{formatFileSize(row.getValue('size'))}</div>
      ),
    },
    {
      accessorKey: 'kbs_info',
      header: t('knowledgeBase'),
      cell: ({ row }) => {
        const value: IFile['kbs_info'] = row.getValue('kbs_info');
        return <KnowledgeCell value={value}></KnowledgeCell>;
      },
    },
    {
      id: 'actions',
      header: t('action'),
      enableHiding: false,
      enablePinning: true,
      cell: ({ row }) => {
        return (
          <ActionCell
            row={row}
            showConnectToKnowledgeModal={showConnectToKnowledgeModal}
            showFileRenameModal={showFileRenameModal}
            showMoveFileModal={showMoveFileModal}
          ></ActionCell>
        );
      },
    },
  ];

  const currentPagination = useMemo(() => {
    return {
      pageIndex: (pagination.current || 1) - 1,
      pageSize: pagination.pageSize || 10,
    };
  }, [pagination]);

  const table = useReactTable({
    data: files || [],
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    // getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    onRowSelectionChange: setRowSelection,

    manualPagination: true, //we're doing manual "server-side" pagination
    enableRowSelection(row) {
      return !isKnowledgeBaseType(row.original.source_type);
    },
    state: {
      sorting,
      columnFilters,
      columnVisibility,
      rowSelection,
      pagination: currentPagination,
    },
    rowCount: total ?? 0,
    debugTable: true,
  });

  return (
    <>
      <div className="w-full">
        <Table rootClassName="max-h-[calc(100vh-242px)] overflow-auto">
          <TableHeader>
            {table.getHeaderGroups().map((headerGroup) => (
              <TableRow key={headerGroup.id}>
                {headerGroup.headers.map((header) => {
                  return (
                    <TableHead key={header.id}>
                      {header.isPlaceholder
                        ? null
                        : flexRender(
                            header.column.columnDef.header,
                            header.getContext(),
                          )}
                    </TableHead>
                  );
                })}
              </TableRow>
            ))}
          </TableHeader>
          <TableBody className="max-h-96 overflow-y-auto">
            {loading ? (
              <TableSkeleton columnsLength={columns.length}></TableSkeleton>
            ) : table.getRowModel().rows?.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow
                  key={row.id}
                  data-state={row.getIsSelected() && 'selected'}
                  className="group"
                >
                  {row.getVisibleCells().map((cell) => (
                    <TableCell
                      key={cell.id}
                      className={cell.column.columnDef.meta?.cellClassName}
                    >
                      {flexRender(
                        cell.column.columnDef.cell,
                        cell.getContext(),
                      )}
                    </TableCell>
                  ))}
                </TableRow>
              ))
            ) : (
              <TableEmpty columnsLength={columns.length}></TableEmpty>
            )}
          </TableBody>
        </Table>
      </div>
      <div className="flex items-center justify-end py-4">
        <div className="space-x-2">
          <RAGFlowPagination
            {...pick(pagination, 'current', 'pageSize')}
            total={total}
            onChange={(page, pageSize) => {
              setPagination({ page, pageSize });
            }}
          ></RAGFlowPagination>
        </div>
      </div>
      {connectToKnowledgeVisible && (
        <LinkToDatasetDialog
          hideModal={hideConnectToKnowledgeModal}
          initialConnectedIds={initialConnectedIds}
          onConnectToKnowledgeOk={onConnectToKnowledgeOk}
          loading={connectToKnowledgeLoading}
        ></LinkToDatasetDialog>
      )}
      {fileRenameVisible && (
        <RenameDialog
          hideModal={hideFileRenameModal}
          onOk={onFileRenameOk}
          initialName={initialFileName}
          loading={fileRenameLoading}
        ></RenameDialog>
      )}
    </>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/files/files-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 346 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FilesTable`: Exported entity

### Functions (3)

- `FilesTable()`: Function definition
- `handleNameClick()`: Function definition
- `currentPagination()`: Function definition

### Imports (27)

- `import {`
- `import { ArrowUpDown } from 'lucide-react';`
- `import * as React from 'react';`
- `import { FileIcon } from '@/components/icon-font';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { TableEmpty, TableSkeleton } from '@/components/table-skeleton';`
- `import { Button } from '@/components/ui/button';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import {`

## Code Structure Analysis

- Total lines: 346
- Blank lines: 12 (3.5%)
- Comment lines: ~1 (0.3%)
- Code lines: ~333


## Dependencies and Imports

- `lucide-react`
- `react`
- `@/components/icon-font`
- `@/components/rename-dialog`
- `@/components/table-skeleton`
- `@/components/ui/button`
- `@/components/ui/checkbox`
- `@/components/ui/ragflow-pagination`
- `@/hooks/logic-hooks/use-row-selection`
- `@/hooks/use-file-request`
- `@/interfaces/database/file-manager`
- `@/lib/utils`
- `@/utils/common-util`
- `@/utils/date`
- `lodash`
- `react`
- `react-i18next`
- `./action-cell`
- `./hooks`
- `./knowledge-cell`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/files`.

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

- Other files in `web/src/pages/files/` directory
- Potential test file: `test_files-table.tsx`

## Keywords

./action-cell, ./hooks, ./knowledge-cell, ./link-to-dataset-dialog, ./use-move-file, ./use-navigate-to-folder, ./util, @/components/icon-font, @/components/rename-dialog, @/components/table-skeleton, @/components/ui/button, @/components/ui/checkbox, @/components/ui/ragflow-pagination, @/hooks/logic-hooks/use-row-selection, @/hooks/use-file-request, @/interfaces/database/file-manager, @/lib/utils, @/utils/common-util, @/utils/date, ActionCell, ArrowUpDown, Button, Checkbox, ColumnDef, ColumnFiltersState, FileIcon, FilesTable, FilesTableProps, IFile, KnowledgeCell, LinkToDatasetDialog, Pick, RAGFlowPagination, React, RenameDialog, ReturnType, Select, SortingState, Table, TableBody, TableCell, TableEmpty, TableHead, TableHeader, TableRow, TableSkeleton, Tooltip, TooltipContent, TooltipTrigger, TypeScript...

---
*Generated by RAGFlow Repository Documentation Generator*
