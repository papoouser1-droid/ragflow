# Documentation: web/src/pages/files/files-table.tsx

## File Metadata

- **Path**: `web/src/pages/files/files-table.tsx`
- **Size**: 10303 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/files/files-table.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/files/files-table.tsx` is located in the `web/src/pages/files` directory.

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

- [action-cell.tsx](action-cell.tsx_docs.md)
- [file-breadcrumb.tsx](file-breadcrumb.tsx_docs.md)
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
