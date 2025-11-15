# File Documentation: web/src/pages/dataset/dataset-overview/overview-table.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-overview/overview-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 476
- **Characters**: 14,856
- **Size**: 14,856 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import FileStatusBadge from '@/components/file-status-badge';
import { FileIcon, IconFontFill } from '@/components/icon-font';
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
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { RunningStatusMap } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { cn } from '@/lib/utils';
import { PipelineResultSearchParams } from '@/pages/dataflow-result/constant';
import { NavigateToDataflowResultProps } from '@/pages/dataflow-result/interface';
import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';
import { formatDate, formatSecondsToHumanReadable } from '@/utils/date';
import {
  ColumnDef,
  ColumnFiltersState,
  Row,
  SortingState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';
import { TFunction } from 'i18next';
import { ArrowUpDown, ClipboardList, Eye, MonitorUp } from 'lucide-react';
import { FC, useMemo, useState } from 'react';
import { useParams } from 'umi';
import { RunningStatus } from '../dataset/constant';
import ProcessLogModal from '../process-log-modal';
import { LogTabs, ProcessingType, ProcessingTypeMap } from './dataset-common';
import { DocumentLog, FileLogsTableProps, IFileLogItem } from './interface';

export const getFileLogsTableColumns = (
  t: TFunction<'translation', string>,
  showLog: (row: Row<IFileLogItem & DocumentLog>, active: LogTabs) => void,
  kowledgeId: string,
  navigateToDataflowResult: (
    props: NavigateToDataflowResultProps,
  ) => () => void,
) => {
  // const { t } = useTranslate('knowledgeDetails');
  const columns: ColumnDef<IFileLogItem & DocumentLog>[] = [
    // {
    //   id: 'select',
    //   header: ({ table }) => (
    //     <input
    //       type="checkbox"
    //       checked={table.getIsAllRowsSelected()}
    //       onChange={table.getToggleAllRowsSelectedHandler()}
    //       className="rounded bg-gray-900 text-blue-500 focus:ring-blue-500"
    //     />
    //   ),
    //   cell: ({ row }) => (
    //     <input
    //       type="checkbox"
    //       checked={row.getIsSelected()}
    //       onChange={row.getToggleSelectedHandler()}
    //       className="rounded border-gray-600 bg-gray-900 text-blue-500 focus:ring-blue-500"
    //     />
    //   ),
    // },
    {
      accessorKey: 'id',
      header: 'ID',
      cell: ({ row }) => (
        <div className="text-text-primary">{row.original.id}</div>
      ),
    },
    {
      accessorKey: 'fileName',
      header: t('fileName'),
      meta: { cellClassName: 'max-w-[20vw]' },
      cell: ({ row }) => (
        <Tooltip>
          <TooltipTrigger asChild>
            <div className="flex gap-2 cursor-pointer">
              <FileIcon name={row.original.document_name}></FileIcon>
              <span className={cn('truncate')}>
                {row.original.document_name}
              </span>
            </div>
          </TooltipTrigger>
          <TooltipContent>
            <p>{row.original.document_name}</p>
          </TooltipContent>
        </Tooltip>
      ),
    },
    {
      accessorKey: 'source_from',
      header: t('source'),
      meta: { cellClassName: 'max-w-[10vw]' },
      cell: ({ row }) => (
        <div className="text-text-primary">
          {row.original.source_from === 'local' ||
          row.original.source_from === '' ? (
            <div className="bg-accent-primary-5 w-6 h-6 rounded-full flex items-center justify-center">
              <MonitorUp className="text-accent-primary" size={16} />
            </div>
          ) : (
            <div className="w-6 h-6 flex items-center justify-center">
              {
                DataSourceInfo[
                  row.original.source_from as keyof typeof DataSourceInfo
                ].icon
              }
            </div>
          )}
        </div>
      ),
    },
    {
      accessorKey: 'pipeline_title',
      header: t('dataPipeline'),
      cell: ({ row }) => {
        const title = row.original.pipeline_title;
        const pipelineTitle = title === 'naive' ? 'general' : title;
        return (
          <div className="flex items-center gap-2 text-text-primary">
            <RAGFlowAvatar
              avatar={row.original.avatar}
              name={pipelineTitle}
              className="size-4"
            />
            {pipelineTitle}
          </div>
        );
      },
    },
    {
      accessorKey: 'process_begin_at',
      header: ({ column }) => {
        return (
          <Button
            variant="transparent"
            className="border-none"
            onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
          >
            {t('startDate')}
            <ArrowUpDown />
          </Button>
        );
      },
      cell: ({ row }) => (
        <div className="text-text-primary">
          {formatDate(row.original.process_begin_at)}
        </div>
      ),
    },
    {
      accessorKey: 'task_type',
      header: t('task'),
      cell: ({ row }) => (
        <div className="text-text-primary">{row.original.task_type}</div>
      ),
    },
    {
      accessorKey: 'operation_status',
      header: t('status'),
      cell: ({ row }) => (
        <FileStatusBadge
          status={row.original.operation_status as RunningStatus}
          name={
            RunningStatusMap[row.original.operation_status as RunningStatus]
          }
        />
      ),
    },
    {
      id: 'operations',
      header: t('operations'),
      cell: ({ row }) => (
        <div className="flex justify-start space-x-2 opacity-0 group-hover:opacity-100 transition-opacity">
          <Button
            variant="ghost"
            size="sm"
            className="p-1"
            onClick={() => {
              showLog(row, LogTabs.FILE_LOGS);
            }}
          >
            <Eye />
          </Button>
          {row.original.pipeline_id && (
            <Button
              variant="ghost"
              size="sm"
              className="p-1"
              onClick={navigateToDataflowResult({
                id: row.original.id,
                [PipelineResultSearchParams.KnowledgeId]: kowledgeId,
                [PipelineResultSearchParams.DocumentId]:
                  row.original.document_id,
                [PipelineResultSearchParams.IsReadOnly]: 'false',
                [PipelineResultSearchParams.Type]: 'dataflow',
              })}
            >
              <ClipboardList />
            </Button>
          )}
        </div>
      ),
    },
  ];

  return columns;
};

export const getDatasetLogsTableColumns = (
  t: TFunction<'translation', string>,
  showLog: (row: Row<IFileLogItem & DocumentLog>, active: LogTabs) => void,
) => {
  // const { t } = useTranslate('knowledgeDetails');
  const columns: ColumnDef<IFileLogItem & DocumentLog>[] = [
    // {
    // id: 'select',
    // header: ({ table }) => (
    //   <input
    //     type="checkbox"
    //     checked={table.getIsAllRowsSelected()}
    //     onChange={table.getToggleAllRowsSelectedHandler()}
    //     className="rounded bg-gray-900 text-blue-500 focus:ring-blue-500"
    //   />
    // ),
    // cell: ({ row }) => (
    //   <input
    //     type="checkbox"
    //     checked={row.getIsSelected()}
    //     onChange={row.getToggleSelectedHandler()}
    //     className="rounded border-gray-600 bg-gray-900 text-blue-500 focus:ring-blue-500"
    //   />
    // ),
    // },
    {
      accessorKey: 'id',
      header: 'ID',
      cell: ({ row }) => (
        <div className="text-text-primary">{row.original.id}</div>
      ),
    },
    {
      accessorKey: 'process_begin_at',
      header: ({ column }) => {
        return (
          <Button
            variant="transparent"
            className="border-none"
            onClick={() => column.toggleSorting(column.getIsSorted() === 'asc')}
          >
            {t('startDate')}
            <ArrowUpDown />
          </Button>
        );
      },
      cell: ({ row }) => (
        <div className="text-text-primary">
          {formatDate(row.original.process_begin_at)}
        </div>
      ),
    },
    {
      accessorKey: 'task_type',
      header: t('processingType'),
      cell: ({ row }) => (
        <div className="flex items-center gap-2 text-text-primary">
          {ProcessingType.knowledgeGraph === row.original.task_type && (
            <IconFontFill
              name={`knowledgegraph`}
              className="text-text-secondary"
            ></IconFontFill>
          )}
          {ProcessingType.raptor === row.original.task_type && (
            <IconFontFill
              name={`dataflow-01`}
              className="text-text-secondary"
            ></IconFontFill>
          )}
          {ProcessingTypeMap[row.original.task_type as ProcessingType] ||
            row.original.task_type}
        </div>
      ),
    },
    {
      accessorKey: 'operation_status',
      header: t('status'),
      cell: ({ row }) => (
        // <FileStatusBadge
        //   status={row.original.status}
        //   name={row.original.statusName}
        // />
        <FileStatusBadge
          status={row.original.operation_status as RunningStatus}
          name={
            RunningStatusMap[row.original.operation_status as RunningStatus]
          }
        />
      ),
    },
    {
      id: 'operations',
      header: t('operations'),
      cell: ({ row }) => (
        <div className="flex justify-start space-x-2 opacity-0 group-hover:opacity-100 transition-opacity">
          <Button
            variant="ghost"
            size="sm"
            className="p-1"
            onClick={() => {
              showLog(row, LogTabs.DATASET_LOGS);
            }}
          >
            <Eye />
          </Button>
        </div>
      ),
    },
  ];

  return columns;
};

const FileLogsTable: FC<FileLogsTableProps> = ({
  data,
  pagination,
  setPagination,
  active = LogTabs.FILE_LOGS,
}) => {
  const [sorting, setSorting] = useState<SortingState>([]);
  const [columnFilters, setColumnFilters] = useState<ColumnFiltersState>([]);
  const [rowSelection, setRowSelection] = useState({});
  const { t } = useTranslate('knowledgeDetails');
  const [isModalVisible, setIsModalVisible] = useState(false);
  const { navigateToDataflowResult } = useNavigatePage();
  const [logInfo, setLogInfo] = useState<IFileLogItem>();
  const kowledgeId = useParams().id;
  const showLog = (row: Row<IFileLogItem & DocumentLog>) => {
    const logDetail = {
      taskId: row.original?.dsl?.task_id,
      fileName: row.original.document_name,
      source: row.original.source_from,
      task: row.original?.task_type,
      status: row.original.status as RunningStatus,
      startDate: formatDate(row.original.process_begin_at),
      duration: formatSecondsToHumanReadable(
        row.original.process_duration || 0,
      ),
      details: row.original.progress_msg,
    } as unknown as IFileLogItem;
    console.log('logDetail', logDetail);
    setLogInfo(logDetail);
    setIsModalVisible(true);
  };

  const columns = useMemo(() => {
    return active === LogTabs.FILE_LOGS
      ? getFileLogsTableColumns(
          t,
          showLog,
          kowledgeId || '',
          navigateToDataflowResult,
        )
      : getDatasetLogsTableColumns(t, showLog);
  }, [active, t]);

  const currentPagination = useMemo(
    () => ({
      pageIndex: (pagination.current || 1) - 1,
      pageSize: pagination.pageSize || 10,
    }),
    [pagination],
  );

  const table = useReactTable<IFileLogItem & DocumentLog>({
    data: data || [],
    columns,
    manualPagination: true,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    onRowSelectionChange: setRowSelection,
    state: {
      sorting,
      columnFilters,
      rowSelection,
      pagination: currentPagination,
    },
    pageCount: pagination.total
      ? Math.ceil(pagination.total / pagination.pageSize)
      : 0,
  });

  return (
    <div className="w-full h-[calc(100vh-360px)]">
      <Table rootClassName="max-h-[calc(100vh-380px)]">
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
              <TableCell colSpan={columns.length} className="h-24 text-center">
                No results.
              </TableCell>
            </TableRow>
          )}
        </TableBody>
      </Table>
      <div className="flex items-center justify-end absolute bottom-3 right-12">
        <div className="space-x-2">
          <RAGFlowPagination
            {...{ current: pagination.current, pageSize: pagination.pageSize }}
            total={pagination.total}
            onChange={(page, pageSize) => setPagination({ page, pageSize })}
          />
        </div>
      </div>
      {isModalVisible && (
        <ProcessLogModal
          title={active === LogTabs.FILE_LOGS ? t('fileLogs') : t('datasetLog')}
          visible={isModalVisible}
          onCancel={() => setIsModalVisible(false)}
          logInfo={logInfo}
        />
      )}
    </div>
  );
};

export default FileLogsTable;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-overview/overview-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 476 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `getFileLogsTableColumns`: Exported entity
- `getDatasetLogsTableColumns`: Exported entity

### Functions (5)

- `getFileLogsTableColumns()`: Function definition
- `getDatasetLogsTableColumns()`: Function definition
- `showLog()`: Function definition
- `columns()`: Function definition
- `currentPagination()`: Function definition

### Imports (24)

- `import FileStatusBadge from '@/components/file-status-badge';`
- `import { FileIcon, IconFontFill } from '@/components/icon-font';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { Button } from '@/components/ui/button';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import {`
- `import {`
- `import { RunningStatusMap } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`

## Code Structure Analysis

- Total lines: 476
- Blank lines: 11 (2.3%)
- Comment lines: ~44 (9.2%)
- Code lines: ~421


## Dependencies and Imports

- `@/components/file-status-badge`
- `@/components/icon-font`
- `@/components/ragflow-avatar`
- `@/components/ui/button`
- `@/components/ui/ragflow-pagination`
- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks/navigate-hooks`
- `@/lib/utils`
- `@/pages/dataflow-result/constant`
- `@/pages/dataflow-result/interface`
- `@/pages/user-setting/data-source/contant`
- `@/utils/date`
- `i18next`
- `lucide-react`
- `react`
- `umi`
- `../dataset/constant`
- `../process-log-modal`
- `./dataset-common`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-overview`.

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

- Other files in `web/src/pages/dataset/dataset-overview/` directory
- Potential test file: `test_overview-table.tsx`

## Keywords

../dataset/constant, ../process-log-modal, ./dataset-common, ./interface, @/components/file-status-badge, @/components/icon-font, @/components/ragflow-avatar, @/components/ui/button, @/components/ui/ragflow-pagination, @/constants/knowledge, @/hooks/common-hooks, @/hooks/logic-hooks/navigate-hooks, @/lib/utils, @/pages/dataflow-result/constant, @/pages/dataflow-result/interface, @/pages/user-setting/data-source/contant, @/utils/date, ArrowUpDown, Button, ClipboardList, ColumnDef, ColumnFiltersState, DATASET_LOGS, DataSourceInfo, DocumentId, DocumentLog, Eye, FILE_LOGS, FileIcon, FileLogsTable, FileLogsTableProps, FileStatusBadge, IFileLogItem, IconFontFill, IsReadOnly, KnowledgeId, LogTabs, Math, MonitorUp, NavigateToDataflowResultProps, PipelineResultSearchParams, ProcessLogModal, ProcessingType, ProcessingTypeMap, RAGFlowAvatar, RAGFlowPagination, Row, RunningStatus, RunningStatusMap, SortingState...

---
*Generated by RAGFlow Repository Documentation Generator*
