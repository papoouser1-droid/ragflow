# File Documentation: web/src/pages/agent/form/invoke-form/variable-table.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/variable-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 200
- **Characters**: 5,550
- **Size**: 5,550 bytes
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
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';
import { Pencil, Trash2 } from 'lucide-react';
import * as React from 'react';

import { TableEmpty } from '@/components/table-skeleton';
import { Button } from '@/components/ui/button';
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
import { cn } from '@/lib/utils';
import { useTranslation } from 'react-i18next';
import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';
import { VariableFormSchemaType } from './schema';

interface IProps {
  data: VariableFormSchemaType[];
  deleteRecord(index: number): void;
  showModal(index: number, record: VariableFormSchemaType): void;
  nodeId?: string;
}

export function VariableTable({
  data = [],
  deleteRecord,
  showModal,
  nodeId,
}: IProps) {
  const { t } = useTranslation();
  const { getLabel } = useGetVariableLabelOrTypeByValue(nodeId!);

  const [sorting, setSorting] = React.useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    [],
  );
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({});

  const columns: ColumnDef<VariableFormSchemaType>[] = [
    {
      accessorKey: 'key',
      header: t('flow.key'),
      meta: { cellClassName: 'max-w-30' },
      cell: ({ row }) => {
        const key: string = row.getValue('key');
        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div className="truncate">{key}</div>
            </TooltipTrigger>
            <TooltipContent>
              <p>{key}</p>
            </TooltipContent>
          </Tooltip>
        );
      },
    },
    {
      accessorKey: 'ref',
      header: t('flow.ref'),
      meta: { cellClassName: 'max-w-30' },
      cell: ({ row }) => {
        const ref: string = row.getValue('ref');
        const label = getLabel(ref);
        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div className="truncate">{label}</div>
            </TooltipTrigger>
            <TooltipContent>
              <p>{label}</p>
            </TooltipContent>
          </Tooltip>
        );
      },
    },
    {
      accessorKey: 'value',
      header: t('flow.value'),
      cell: ({ row }) => <div>{row.getValue('value')}</div>,
    },
    {
      id: 'actions',
      enableHiding: false,
      header: t('common.action'),
      cell: ({ row }) => {
        const record = row.original;
        const idx = row.index;

        return (
          <div>
            <Button
              className="bg-transparent text-foreground  hover:bg-muted-foreground hover:text-foreground"
              onClick={() => showModal(idx, record)}
            >
              <Pencil />
            </Button>
            <Button
              className="bg-transparent text-foreground  hover:bg-muted-foreground hover:text-foreground"
              onClick={() => deleteRecord(idx)}
            >
              <Trash2 />
            </Button>
          </div>
        );
      },
    },
  ];

  const table = useReactTable({
    data,
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    state: {
      sorting,
      columnFilters,
      columnVisibility,
    },
  });

  return (
    <div className="w-full">
      <div className="rounded-md border">
        <Table rootClassName="rounded-md">
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
          <TableBody>
            {table.getRowModel().rows?.length ? (
              table.getRowModel().rows.map((row) => (
                <TableRow
                  key={row.id}
                  data-state={row.getIsSelected() && 'selected'}
                >
                  {row.getVisibleCells().map((cell) => (
                    <TableCell
                      key={cell.id}
                      className={cn(cell.column.columnDef.meta?.cellClassName)}
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
    </div>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/invoke-form/variable-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 200 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `VariableTable`: Exported entity

### Functions (1)

- `VariableTable()`: Function definition

### Imports (11)

- `import {`
- `import { Pencil, Trash2 } from 'lucide-react';`
- `import * as React from 'react';`
- `import { TableEmpty } from '@/components/table-skeleton';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { useTranslation } from 'react-i18next';`
- `import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';`

## Code Structure Analysis

- Total lines: 200
- Blank lines: 10 (5.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~190


## Dependencies and Imports

- `lucide-react`
- `react`
- `@/components/table-skeleton`
- `@/components/ui/button`
- `@/lib/utils`
- `react-i18next`
- `../../hooks/use-get-begin-query`
- `./schema`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/invoke-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/invoke-form/` directory
- Potential test file: `test_variable-table.tsx`

## Keywords

../../hooks/use-get-begin-query, ./schema, @/components/table-skeleton, @/components/ui/button, @/lib/utils, Button, ColumnDef, ColumnFiltersState, IProps, Pencil, React, SortingState, Table, TableBody, TableCell, TableEmpty, TableHead, TableHeader, TableRow, Tooltip, TooltipContent, TooltipTrigger, Trash2, TypeScript, VariableFormSchemaType, VariableTable, VisibilityState, columns, idx, key, label, lucide-react, react, react-i18next, record, ref, table, tanstack

---
*Generated by RAGFlow Repository Documentation Generator*
