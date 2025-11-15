# Documentation: web/src/pages/agent/form/invoke-form/variable-table.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/variable-table.tsx`
- **Size**: 5550 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/invoke-form/variable-table.tsx`.

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/invoke-form/variable-table.tsx` is located in the `web/src/pages/agent/form/invoke-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to invoke-form.

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

- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [schema.ts](schema.ts_docs.md)
- [use-edit-variable.ts](use-edit-variable.ts_docs.md)
- [variable-dialog.tsx](variable-dialog.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
