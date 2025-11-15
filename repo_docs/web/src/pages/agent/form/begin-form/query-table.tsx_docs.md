# File Documentation: web/src/pages/agent/form/begin-form/query-table.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/begin-form/query-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 195
- **Characters**: 5,367
- **Size**: 5,367 bytes
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
import { BeginQuery } from '../../interface';

interface IProps {
  data: BeginQuery[];
  deleteRecord(index: number): void;
  showModal(index: number, record: BeginQuery): void;
}

export function QueryTable({ data = [], deleteRecord, showModal }: IProps) {
  const { t } = useTranslation();

  const [sorting, setSorting] = React.useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    [],
  );
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({});

  const columns: ColumnDef<BeginQuery>[] = [
    {
      accessorKey: 'key',
      header: t('flow.key'),
      meta: { cellClassName: 'max-w-30' },
      cell: ({ row }) => {
        const key: string = row.getValue('key');
        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div className="truncate ">{key}</div>
            </TooltipTrigger>
            <TooltipContent>
              <p>{key}</p>
            </TooltipContent>
          </Tooltip>
        );
      },
    },
    {
      accessorKey: 'name',
      header: t('flow.name'),
      meta: { cellClassName: 'max-w-30' },
      cell: ({ row }) => {
        const name: string = row.getValue('name');
        return (
          <Tooltip>
            <TooltipTrigger asChild>
              <div className="truncate">{name}</div>
            </TooltipTrigger>
            <TooltipContent>
              <p>{name}</p>
            </TooltipContent>
          </Tooltip>
        );
      },
    },
    {
      accessorKey: 'type',
      header: t('flow.type'),
      cell: ({ row }) => (
        <div>
          {t(`flow.${(row.getValue('type')?.toString() || '').toLowerCase()}`)}
        </div>
      ),
    },
    {
      accessorKey: 'optional',
      header: t('flow.optional'),
      cell: ({ row }) => <div>{row.getValue('optional') ? 'Yes' : 'No'}</div>,
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
    <div className="rounded-md border w-full bg-bg-card">
      <Table rootClassName="rounded-md">
        <TableHeader className="bg-bg-card">
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
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
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
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/begin-form/query-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 195 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `QueryTable`: Exported entity

### Functions (1)

- `QueryTable()`: Function definition

### Imports (10)

- `import {`
- `import { Pencil, Trash2 } from 'lucide-react';`
- `import * as React from 'react';`
- `import { TableEmpty } from '@/components/table-skeleton';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { useTranslation } from 'react-i18next';`
- `import { BeginQuery } from '../../interface';`

## Code Structure Analysis

- Total lines: 195
- Blank lines: 10 (5.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~185


## Dependencies and Imports

- `lucide-react`
- `react`
- `@/components/table-skeleton`
- `@/components/ui/button`
- `@/lib/utils`
- `react-i18next`
- `../../interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/begin-form`.

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

- Other files in `web/src/pages/agent/form/begin-form/` directory
- Potential test file: `test_query-table.tsx`

## Keywords

../../interface, @/components/table-skeleton, @/components/ui/button, @/lib/utils, BeginQuery, Button, ColumnDef, ColumnFiltersState, IProps, Pencil, QueryTable, React, SortingState, Table, TableBody, TableCell, TableEmpty, TableHead, TableHeader, TableRow, Tooltip, TooltipContent, TooltipTrigger, Trash2, TypeScript, VisibilityState, Yes, columns, idx, key, lucide-react, name, react, react-i18next, record, table, tanstack

---
*Generated by RAGFlow Repository Documentation Generator*
