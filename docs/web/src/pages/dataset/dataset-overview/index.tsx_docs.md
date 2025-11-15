# Documentation: web/src/pages/dataset/dataset-overview/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-overview/index.tsx`
- **Size**: 9866 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-overview/index.tsx`.

## Original Source Code

```tsx
import FileStatusBadge from '@/components/file-status-badge';
import { FilterCollection } from '@/components/list-filter-bar/interface';
import SvgIcon from '@/components/svg-icon';
import { useIsDarkTheme } from '@/components/theme-provider';
import { AntToolTip } from '@/components/ui/tooltip';
import { RunningStatusMap } from '@/constants/knowledge';
import { useFetchDocumentList } from '@/hooks/use-document-request';
import { CircleQuestionMark } from 'lucide-react';
import { FC, useEffect, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { RunningStatus } from '../dataset/constant';
import { LogTabs } from './dataset-common';
import { DatasetFilter } from './dataset-filter';
import { useFetchFileLogList, useFetchOverviewTital } from './hook';
import { DocumentLog, IFileLogItem } from './interface';
import FileLogsTable from './overview-table';

interface StatCardProps {
  title: string;
  value: number;
  icon: JSX.Element;
  children?: JSX.Element;
  tooltip?: string;
}
interface CardFooterProcessProps {
  success: number;
  failed: number;
  successTip?: string;
  failedTip?: string;
}

const StatCard: FC<StatCardProps> = ({
  title,
  value,
  children,
  icon,
  tooltip,
}) => {
  return (
    <div className="bg-bg-card  p-4 rounded-lg border border-border flex flex-col gap-2">
      <div className="flex items-center justify-between">
        <h3 className="flex items-center gap-1 text-sm font-medium text-text-secondary">
          {title}
          {tooltip && (
            <AntToolTip title={tooltip} trigger="hover">
              <CircleQuestionMark size={12} />
            </AntToolTip>
          )}
        </h3>
        {icon}
      </div>
      <div className="text-2xl font-bold text-text-primary">{value}</div>
      <div className="h-12 w-full flex items-center">
        <div className="flex-1">{children}</div>
      </div>
    </div>
  );
};

const CardFooterProcess: FC<CardFooterProcessProps> = ({
  success = 0,
  successTip,
  failed = 0,
  failedTip,
}) => {
  const { t } = useTranslation();
  return (
    <div className="flex items-center flex-col gap-2">
      <div className="w-full flex justify-between gap-4 rounded-lg text-sm font-bold text-text-primary">
        <div className="flex items-center justify-between  rounded-md w-1/2 p-2 bg-state-success-5">
          <div className="flex items-center rounded-lg gap-1">
            <div className="w-2 h-2 rounded-full bg-state-success "></div>
            <div className="font-normal text-text-secondary text-xs flex items-center gap-1">
              {t('knowledgeDetails.success')}
              {successTip && (
                <AntToolTip title={successTip} trigger="hover">
                  <CircleQuestionMark size={12} />
                </AntToolTip>
              )}
            </div>
          </div>
          <div>{success || 0}</div>
        </div>
        <div className="flex items-center justify-between rounded-md w-1/2 bg-state-error-5 p-2">
          <div className="flex items-center rounded-lg gap-1">
            <div className="w-2 h-2 rounded-full bg-state-error"></div>
            <div className="font-normal text-text-secondary text-xs flex items-center gap-1">
              {t('knowledgeDetails.failed')}
              {failedTip && (
                <AntToolTip title={failedTip} trigger="hover">
                  <CircleQuestionMark size={12} />
                </AntToolTip>
              )}
            </div>
          </div>
          <div>{failed || 0}</div>
        </div>
      </div>
    </div>
  );
};

const FileLogsPage: FC = () => {
  const { t } = useTranslation();

  const [topAllData, setTopAllData] = useState({
    totalFiles: {
      value: 0,
      precent: 0,
    },
    downloads: {
      value: 0,
      success: 0,
      failed: 0,
    },
    processing: {
      value: 0,
      success: 0,
      failed: 0,
    },
  });
  const { data: topData } = useFetchOverviewTital();
  const {
    pagination: { total: fileTotal },
  } = useFetchDocumentList();

  useEffect(() => {
    setTopAllData((prev) => {
      return {
        ...prev,
        downloads: {
          ...prev.downloads,
          success: topData?.downloaded || 0,
        },
        processing: {
          value: topData?.processing || 0,
          success: topData?.finished || 0,
          failed: topData?.failed || 0,
        },
      };
    });
  }, [topData]);

  useEffect(() => {
    setTopAllData((prev) => {
      return {
        ...prev,
        totalFiles: {
          value: fileTotal || 0,
          precent: 0,
        },
      };
    });
  }, [fileTotal]);

  const {
    data: tableOriginData,
    searchString,
    handleInputChange,
    pagination,
    setPagination,
    active,
    filterValue,
    setFilterValue,
    handleFilterSubmit,
    setActive,
  } = useFetchFileLogList();

  const filters = useMemo(() => {
    const filterCollection: FilterCollection[] = [
      {
        field: 'operation_status',
        label: t('knowledgeDetails.status'),
        list: Object.values(RunningStatus).map((value) => {
          // const value = key as RunningStatus;
          console.log(value);
          return {
            id: value,
            // label: RunningStatusMap[value].label,
            label: (
              <FileStatusBadge
                status={value as RunningStatus}
                name={RunningStatusMap[value as RunningStatus]}
              />
            ),
          };
        }),
      },
      // {
      //   field: 'types',
      //   label: t('knowledgeDetails.task'),
      //   list: [
      //     {
      //       id: 'Parse',
      //       label: 'Parse',
      //     },
      //     {
      //       id: 'Download',
      //       label: 'Download',
      //     },
      //   ],
      // },
    ];
    if (active === LogTabs.FILE_LOGS) {
      return filterCollection;
    }
    if (active === LogTabs.DATASET_LOGS) {
      const list = filterCollection.filter((item, index) => index === 0);
      return list;
    }
    return [];
  }, [active, t]);

  const tableList = useMemo(() => {
    console.log('tableList', tableOriginData);
    if (tableOriginData && tableOriginData.logs?.length) {
      return tableOriginData.logs.map((item) => {
        return {
          ...item,
          status: item.operation_status as RunningStatus,
          statusName: RunningStatusMap[item.operation_status as RunningStatus],
        } as unknown as IFileLogItem & DocumentLog;
      });
    }
    return [];
  }, [tableOriginData]);

  const changeActiveLogs = (active: (typeof LogTabs)[keyof typeof LogTabs]) => {
    setFilterValue({});
    setActive(active);
  };
  const handlePaginationChange = ({
    page,
    pageSize,
  }: {
    page: number;
    pageSize: number;
  }) => {
    console.log('Pagination changed:', { page, pageSize });
    setPagination({
      ...pagination,
      page,
      pageSize: pageSize,
    });
  };

  const isDark = useIsDarkTheme();

  return (
    <div className="p-5 min-w-[880px] border-border border rounded-lg mr-5">
      {/* Stats Cards */}
      <div className="grid grid-cols-3 md:grid-cols-3 gap-4 mb-6">
        <StatCard
          title={t('datasetOverview.totalFiles')}
          value={topAllData.totalFiles.value}
          icon={
            isDark ? (
              <SvgIcon name="data-flow/total-files-icon" width={40} />
            ) : (
              <SvgIcon name="data-flow/total-files-icon-bri" width={40} />
            )
          }
        >
          <div>
            <span className="text-accent-primary">
              {topAllData.totalFiles.precent > 0 ? '+' : ''}
              {topAllData.totalFiles.precent}%{' '}
            </span>
            <span className="font-normal text-text-secondary text-xs">
              from last week
            </span>
          </div>
        </StatCard>
        <StatCard
          title={t('datasetOverview.downloading')}
          value={topAllData.downloads.value}
          icon={
            isDark ? (
              <SvgIcon name="data-flow/data-icon" width={40} />
            ) : (
              <SvgIcon name="data-flow/data-icon-bri" width={40} />
            )
          }
          tooltip={t('datasetOverview.downloadTip')}
        >
          <CardFooterProcess
            success={topAllData.downloads.success}
            successTip={t('datasetOverview.downloadSuccessTip')}
            failed={topAllData.downloads.failed}
            failedTip={t('datasetOverview.downloadFailedTip')}
          />
        </StatCard>
        <StatCard
          title={t('datasetOverview.processing')}
          value={topAllData.processing.value}
          icon={
            isDark ? (
              <SvgIcon name="data-flow/processing-icon" width={40} />
            ) : (
              <SvgIcon name="data-flow/processing-icon-bri" width={40} />
            )
          }
          tooltip={t('datasetOverview.processingTip')}
        >
          <CardFooterProcess
            success={topAllData.processing.success}
            successTip={t('datasetOverview.processingSuccessTip')}
            failed={topAllData.processing.failed}
            failedTip={t('datasetOverview.processingFailedTip')}
          />
        </StatCard>
      </div>

      {/* Tabs & Search */}
      <DatasetFilter
        filters={filters as FilterCollection[]}
        value={filterValue}
        active={active}
        setActive={changeActiveLogs}
        searchString={searchString}
        onSearchChange={handleInputChange}
        onChange={handleFilterSubmit}
      />

      {/* Table */}
      <FileLogsTable
        data={tableList}
        pagination={pagination}
        setPagination={handlePaginationChange}
        pageCount={10}
        active={active}
      />
    </div>
  );
};

export default FileLogsPage;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-overview/index.tsx` is located in the `web/src/pages/dataset/dataset-overview` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset-overview.

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

- [dataset-common.ts](dataset-common.ts_docs.md)
- [dataset-filter.tsx](dataset-filter.tsx_docs.md)
- [hook.ts](hook.ts_docs.md)
- [interface.ts](interface.ts_docs.md)
- [overview-table.tsx](overview-table.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
