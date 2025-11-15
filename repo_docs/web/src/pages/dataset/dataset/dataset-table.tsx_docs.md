# File Documentation: web/src/pages/dataset/dataset/dataset-table.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/dataset-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 227
- **Characters**: 6,743
- **Size**: 6,743 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/dataset-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 227 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `DatasetTable`: Exported entity

### Functions (2)

- `DatasetTable()`: Function definition
- `currentPagination()`: Function definition

### Imports (19)

- `import {`
- `import * as React from 'react';`
- `import { ChunkMethodDialog } from '@/components/chunk-method-dialog';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import {`
- `import { UseRowSelectionType } from '@/hooks/logic-hooks/use-row-selection';`
- `import { useFetchDocumentList } from '@/hooks/use-document-request';`
- `import { getExtension } from '@/utils/document-util';`
- `import { t } from 'i18next';`

## Code Structure Analysis

- Total lines: 227
- Blank lines: 14 (6.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~213


## Dependencies and Imports

- `react`
- `@/components/chunk-method-dialog`
- `@/components/rename-dialog`
- `@/components/ui/ragflow-pagination`
- `@/hooks/logic-hooks/use-row-selection`
- `@/hooks/use-document-request`
- `@/utils/document-util`
- `i18next`
- `lodash`
- `react`
- `../process-log-modal`
- `./hooks`
- `./set-meta-dialog`
- `./use-change-document-parser`
- `./use-dataset-table-columns`
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
- Potential test file: `test_dataset-table.tsx`

## Keywords

../process-log-modal, ./hooks, ./set-meta-dialog, ./use-change-document-parser, ./use-dataset-table-columns, ./use-rename-document, ./use-save-meta, @/components/chunk-method-dialog, @/components/rename-dialog, @/components/ui/ragflow-pagination, @/hooks/logic-hooks/use-row-selection, @/hooks/use-document-request, @/utils/document-util, ChunkMethodDialog, ColumnFiltersState, DatasetTable, DatasetTableProps, Pick, ProcessLogModal, RAGFlowPagination, React, RenameDialog, ReturnType, SetMetaDialog, SortingState, Table, TableBody, TableCell, TableHead, TableHeader, TableRow, TypeScript, UseRowSelectionType, VisibilityState, columns, currentPagination, i18next, lodash, react, table, tanstack

---
*Generated by RAGFlow Repository Documentation Generator*
