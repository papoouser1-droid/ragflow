# Documentation: web/src/pages/user-setting/data-source/data-source-detail-page/log-table.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/data-source-detail-page/log-table.tsx`
- **Size**: 7466 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/data-source/data-source-detail-page/log-table.tsx`.

## Original Source Code

```tsx
import FileStatusBadge from '@/components/file-status-badge';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { Button } from '@/components/ui/button';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import { RunningStatusMap } from '@/constants/knowledge';
import { RunningStatus } from '@/pages/dataset/dataset/constant';
import { Routes } from '@/routes';
import { formatDate } from '@/utils/date';
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@radix-ui/react-hover-card';
import {
  ColumnDef,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';
import { t } from 'i18next';
import { pick } from 'lodash';
import { Eye } from 'lucide-react';
import { useCallback, useMemo } from 'react';
import { useNavigate } from 'umi';
import { useLogListDataSource } from '../hooks';

const columns = ({
  handleToDataSetDetail,
}: {
  handleToDataSetDetail: (id: string) => void;
}) => {
  return [
    {
      accessorKey: 'update_date',
      header: t('setting.timeStarted'),
      cell: ({ row }) => (
        <div className="flex items-center gap-2 text-text-primary">
          {row.original.update_date
            ? formatDate(row.original.update_date)
            : '-'}
        </div>
      ),
    },
    {
      accessorKey: 'status',
      header: t('knowledgeDetails.status'),
      cell: ({ row }) => (
        <FileStatusBadge
          status={row.original.status as RunningStatus}
          name={RunningStatusMap[row.original.status as RunningStatus]}
          className="!w-20"
        />
      ),
    },
    {
      accessorKey: 'kb_name',
      header: t('knowledgeDetails.dataset'),
      cell: ({ row }) => {
        return (
          <div
            className="flex items-center gap-2 text-text-primary cursor-pointer"
            onClick={() => {
              console.log('handleToDataSetDetail', row.original.kb_id);
              handleToDataSetDetail(row.original.kb_id);
            }}
          >
            <RAGFlowAvatar
              avatar={row.original.avatar}
              name={row.original.kb_name}
              className="size-4"
            />
            {row.original.kb_name}
          </div>
        );
      },
    },
    {
      accessorKey: 'new_docs_indexed',
      header: t('setting.newDocs'),
    },

    {
      id: 'operations',
      header: t('setting.errorMsg'),
      cell: ({ row }) => (
        <div className="flex gap-1 items-center">
          {row.original.error_msg}
          {row.original.error_msg && (
            <div className="flex justify-start space-x-2 opacity-0 group-hover:opacity-100 transition-opacity">
              <HoverCard>
                <HoverCardTrigger>
                  <Button
                    variant="ghost"
                    size="sm"
                    className="p-1"
                    // onClick={() => {
                    //   showLog(row, LogTabs.FILE_LOGS);
                    // }}
                  >
                    <Eye />
                  </Button>
                </HoverCardTrigger>
                <HoverCardContent className="w-[40vw] max-h-[40vh] overflow-auto bg-bg-base z-[999] px-3 py-2 rounded-md border border-border-default">
                  <div className="space-y-2">
                    {row.original.full_exception_trace}
                  </div>
                </HoverCardContent>
              </HoverCard>
            </div>
          )}
        </div>
      ),
    },
  ] as ColumnDef<any>[];
};

// const paginationInit = {
//   current: 1,
//   pageSize: 10,
//   total: 0,
// };
export const DataSourceLogsTable = ({
  refresh_freq,
}: {
  refresh_freq: number | false;
}) => {
  // const [pagination, setPagination] = useState(paginationInit);
  const { data, pagination, setPagination } =
    useLogListDataSource(refresh_freq);
  const navigate = useNavigate();
  const currentPagination = useMemo(
    () => ({
      pageIndex: (pagination.current || 1) - 1,
      pageSize: pagination.pageSize || 10,
    }),
    [pagination],
  );

  const handleToDataSetDetail = useCallback(
    (id: string) => {
      console.log('handleToDataSetDetail', id);
      navigate(`${Routes.DatasetBase}${Routes.DatasetBase}/${id}`);
    },
    [navigate],
  );

  const table = useReactTable<any>({
    data: data || [],
    columns: columns({ handleToDataSetDetail }),
    manualPagination: true,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    // onSortingChange: setSorting,
    // onColumnFiltersChange: setColumnFilters,
    // onRowSelectionChange: setRowSelection,
    state: {
      //   sorting,
      //   columnFilters,
      //   rowSelection,
      pagination: currentPagination,
    },
    // pageCount: pagination.total
    //   ? Math.ceil(pagination.total / pagination.pageSize)
    //   : 0,
    rowCount: pagination.total ?? 0,
  });

  return (
    // <div className="w-full h-[calc(100vh-360px)]">
    //   <Table rootClassName="max-h-[calc(100vh-380px)]">
    <div className="w-full">
      <Table>
        <TableHeader>
          {table.getHeaderGroups().map((headerGroup) => (
            <TableRow key={headerGroup.id}>
              {headerGroup.headers.map((header) => (
                <TableHead key={header.id}>
                  {flexRender(
                    header.column.columnDef.header,
                    header.getContext(),
                  )}
                </TableHead>
              ))}
            </TableRow>
          ))}
        </TableHeader>
        <TableBody className="relative min-w-[1280px] overflow-auto">
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
              <TableCell colSpan={5} className="h-24 text-center">
                {t('common.noData')}
              </TableCell>
            </TableRow>
          )}
        </TableBody>
      </Table>
      <div className="flex items-center justify-end mt-4">
        <div className="space-x-2">
          {/* <RAGFlowPagination
            {...{ current: pagination.current, pageSize: pagination.pageSize }}
            total={pagination.total}
            onChange={(page, pageSize) => setPagination({ page, pageSize })}
          /> */}
          <RAGFlowPagination
            {...pick(pagination, 'current', 'pageSize')}
            total={pagination.total}
            onChange={(page, pageSize) => {
              setPagination({ page, pageSize });
            }}
          ></RAGFlowPagination>
        </div>
      </div>
    </div>
  );
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/data-source/data-source-detail-page/log-table.tsx` is located in the `web/src/pages/user-setting/data-source/data-source-detail-page` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to data-source-detail-page.

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

- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
