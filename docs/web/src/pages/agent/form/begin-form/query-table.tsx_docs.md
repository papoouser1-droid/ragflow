# Documentation: web/src/pages/agent/form/begin-form/query-table.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/begin-form/query-table.tsx`
- **Size**: 5367 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/begin-form/query-table.tsx`.

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/begin-form/query-table.tsx` is located in the `web/src/pages/agent/form/begin-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to begin-form.

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

- [begin-dynamic-options.tsx](begin-dynamic-options.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [parameter-dialog.tsx](parameter-dialog.tsx_docs.md)
- [use-edit-query.ts](use-edit-query.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
