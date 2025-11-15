# Documentation: web/src/pages/dataset/dataset/dataset-table.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/dataset-table.tsx`
- **Size**: 6743 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset/dataset-table.tsx`.

## Original Source Code

```tsx
'use client';

import {
  ColumnFiltersState,
  SortingState,
  VisibilityState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';
import * as React from 'react';

import { ChunkMethodDialog } from '@/components/chunk-method-dialog';
import { RenameDialog } from '@/components/rename-dialog';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import { UseRowSelectionType } from '@/hooks/logic-hooks/use-row-selection';
import { useFetchDocumentList } from '@/hooks/use-document-request';
import { getExtension } from '@/utils/document-util';
import { t } from 'i18next';
import { pick } from 'lodash';
import { useMemo } from 'react';
import ProcessLogModal from '../process-log-modal';
import { useShowLog } from './hooks';
import { SetMetaDialog } from './set-meta-dialog';
import { useChangeDocumentParser } from './use-change-document-parser';
import { useDatasetTableColumns } from './use-dataset-table-columns';
import { useRenameDocument } from './use-rename-document';
import { useSaveMeta } from './use-save-meta';

export type DatasetTableProps = Pick<
  ReturnType<typeof useFetchDocumentList>,
  'documents' | 'setPagination' | 'pagination' | 'loading'
> &
  Pick<UseRowSelectionType, 'rowSelection' | 'setRowSelection'>;

export function DatasetTable({
  documents,
  pagination,
  setPagination,
  rowSelection,
  setRowSelection,
}: DatasetTableProps) {
  const [sorting, setSorting] = React.useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    [],
  );
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({});

  const {
    changeParserLoading,
    onChangeParserOk,
    changeParserVisible,
    hideChangeParserModal,
    showChangeParserModal,
    changeParserRecord,
  } = useChangeDocumentParser();

  const {
    renameLoading,
    onRenameOk,
    renameVisible,
    hideRenameModal,
    showRenameModal,
    initialName,
  } = useRenameDocument();

  const {
    showSetMetaModal,
    hideSetMetaModal,
    setMetaVisible,
    setMetaLoading,
    onSetMetaModalOk,
    metaRecord,
  } = useSaveMeta();
  const { showLog, logInfo, logVisible, hideLog } = useShowLog(documents);

  const columns = useDatasetTableColumns({
    showChangeParserModal,
    showRenameModal,
    showSetMetaModal,
    showLog,
  });

  const currentPagination = useMemo(() => {
    return {
      pageIndex: (pagination.current || 1) - 1,
      pageSize: pagination.pageSize || 10,
    };
  }, [pagination]);

  const table = useReactTable({
    data: documents,
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    onRowSelectionChange: setRowSelection,
    manualPagination: true, //we're doing manual "server-side" pagination
    state: {
      sorting,
      columnFilters,
      columnVisibility,
      rowSelection,
      pagination: currentPagination,
    },
    rowCount: pagination.total ?? 0,
  });

  return (
    <div className="w-full">
      <Table rootClassName="max-h-[calc(100vh-222px)]">
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
        <TableBody className="relative">
          {table.getRowModel().rows?.length ? (
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
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </TableCell>
                ))}
              </TableRow>
            ))
          ) : (
            <TableRow>
              <TableCell colSpan={columns.length} className="h-24 text-center">
                No results.
              </TableCell>
            </TableRow>
          )}
        </TableBody>
      </Table>
      <div className="flex items-center justify-end  py-4 absolute bottom-3 right-3">
        <div className="space-x-2">
          <RAGFlowPagination
            {...pick(pagination, 'current', 'pageSize')}
            total={pagination.total}
            onChange={(page, pageSize) => {
              setPagination({ page, pageSize });
            }}
          ></RAGFlowPagination>
        </div>
      </div>
      {changeParserVisible && (
        <ChunkMethodDialog
          documentId={changeParserRecord.id}
          parserId={changeParserRecord.parser_id}
          pipelineId={changeParserRecord.pipeline_id}
          parserConfig={changeParserRecord.parser_config}
          documentExtension={getExtension(changeParserRecord.name)}
          onOk={onChangeParserOk}
          visible={changeParserVisible}
          hideModal={hideChangeParserModal}
          loading={changeParserLoading}
        ></ChunkMethodDialog>
      )}

      {renameVisible && (
        <RenameDialog
          visible={renameVisible}
          onOk={onRenameOk}
          loading={renameLoading}
          hideModal={hideRenameModal}
          initialName={initialName}
        ></RenameDialog>
      )}

      {setMetaVisible && (
        <SetMetaDialog
          hideModal={hideSetMetaModal}
          loading={setMetaLoading}
          onOk={onSetMetaModalOk}
          initialMetaData={metaRecord.meta_fields}
        ></SetMetaDialog>
      )}
      {logVisible && (
        <ProcessLogModal
          title={t('knowledgeDetails.fileLogs')}
          visible={logVisible}
          onCancel={() => hideLog()}
          logInfo={logInfo}
        />
      )}
    </div>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset/dataset-table.tsx` is located in the `web/src/pages/dataset/dataset` directory.

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
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [parsing-card.tsx](parsing-card.tsx_docs.md)
- [parsing-status-cell.tsx](parsing-status-cell.tsx_docs.md)
- [set-meta-dialog.tsx](set-meta-dialog.tsx_docs.md)
- [use-bulk-operate-dataset.tsx](use-bulk-operate-dataset.tsx_docs.md)
- [use-change-document-parser.ts](use-change-document-parser.ts_docs.md)
- [use-create-empty-document.ts](use-create-empty-document.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
