# File Documentation: web/src/pages/user-setting/data-source/hooks.ts

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 210
- **Characters**: 6,341
- **Size**: 6,347 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { useSetModalState } from '@/hooks/common-hooks';
import { useGetPaginationWithRouter } from '@/hooks/logic-hooks';
import dataSourceService, {
  dataSourceRebuild,
  dataSourceResume,
  deleteDataSource,
  featchDataSourceDetail,
  getDataSourceLogs,
} from '@/services/data-source-service';
import { useQuery, useQueryClient } from '@tanstack/react-query';
import { t } from 'i18next';
import { useCallback, useMemo, useState } from 'react';
import { useParams, useSearchParams } from 'umi';
import { DataSourceInfo, DataSourceKey } from './contant';
import { IDataSorceInfo, IDataSource, IDataSourceBase } from './interface';

export const useListDataSource = () => {
  const { data: list, isFetching } = useQuery<IDataSource[]>({
    queryKey: ['data-source'],
    queryFn: async () => {
      const { data } = await dataSourceService.dataSourceList();
      return data.data;
    },
  });

  const categorizeDataBySource = (data: IDataSourceBase[]) => {
    const categorizedData: Record<DataSourceKey, any[]> = {} as Record<
      DataSourceKey,
      any[]
    >;

    data.forEach((item) => {
      const source = item.source;
      if (!categorizedData[source]) {
        categorizedData[source] = [];
      }
      categorizedData[source].push({
        ...item,
      });
    });

    return categorizedData;
  };

  const updatedDataSourceTemplates = useMemo(() => {
    const categorizedData = categorizeDataBySource(list || []);
    let sourcelist: Array<IDataSorceInfo & { list: Array<IDataSourceBase> }> =
      [];
    Object.keys(categorizedData).forEach((key: string) => {
      const k = key as DataSourceKey;
      sourcelist.push({
        id: k,
        name: DataSourceInfo[k].name,
        description: DataSourceInfo[k].description,
        icon: DataSourceInfo[k].icon,
        list: categorizedData[k] || [],
      });
    });

    console.log('🚀 ~ useListDataSource ~ sourcelist:', sourcelist);
    return sourcelist;
  }, [list]);

  return { list, categorizedList: updatedDataSourceTemplates, isFetching };
};

export const useAddDataSource = () => {
  const [addSource, setAddSource] = useState<IDataSorceInfo | undefined>(
    undefined,
  );
  const [addLoading, setAddLoading] = useState<boolean>(false);
  const {
    visible: addingModalVisible,
    hideModal: hideAddingModal,
    showModal,
  } = useSetModalState();
  const showAddingModal = useCallback(
    (data: IDataSorceInfo) => {
      setAddSource(data);
      showModal();
    },
    [showModal],
  );
  const queryClient = useQueryClient();

  const handleAddOk = useCallback(
    async (data: any) => {
      setAddLoading(true);
      const { data: res } = await dataSourceService.dataSourceSet(data);
      console.log('🚀 ~ handleAddOk ~ code:', res.code);
      if (res.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['data-source'] });
        message.success(t(`message.operated`));
        hideAddingModal();
      }
      setAddLoading(false);
    },
    [hideAddingModal, queryClient],
  );

  return {
    addSource,
    addLoading,
    setAddSource,
    addingModalVisible,
    hideAddingModal,
    showAddingModal,
    handleAddOk,
  };
};

export const useLogListDataSource = (refresh_freq: number | false) => {
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const [currentQueryParameters] = useSearchParams();
  const id = currentQueryParameters.get('id');

  const { data, isFetching } = useQuery<{ logs: IDataSource[]; total: number }>(
    {
      queryKey: ['data-source-logs', id, pagination, refresh_freq],
      refetchInterval: refresh_freq ? refresh_freq * 60 * 1000 : false,
      queryFn: async () => {
        const { data } = await getDataSourceLogs(id as string, {
          page_size: pagination.pageSize,
          page: pagination.current,
        });
        return data.data;
      },
    },
  );
  return {
    data: data?.logs,
    isFetching,
    pagination: { ...pagination, total: data?.total },
    setPagination,
  };
};

export const useDeleteDataSource = () => {
  const [deleteLoading, setDeleteLoading] = useState<boolean>(false);
  const { hideModal, showModal } = useSetModalState();
  const queryClient = useQueryClient();
  const handleDelete = useCallback(
    async ({ id }: { id: string }) => {
      setDeleteLoading(true);
      const { data } = await deleteDataSource(id);
      if (data.code === 0) {
        message.success(t(`message.deleted`));
        queryClient.invalidateQueries({ queryKey: ['data-source'] });
      }
      setDeleteLoading(false);
    },
    [setDeleteLoading, queryClient],
  );
  return { deleteLoading, hideModal, showModal, handleDelete };
};

export const useFetchDataSourceDetail = () => {
  const [currentQueryParameters] = useSearchParams();
  const id = currentQueryParameters.get('id');
  const { data } = useQuery<IDataSource>({
    queryKey: ['data-source-detail', id],
    enabled: !!id,
    queryFn: async () => {
      const { data } = await featchDataSourceDetail(id as string);
      // if (data.code === 0) {

      // }
      return data.data;
    },
  });
  return { data };
};

export const useDataSourceResume = () => {
  const [currentQueryParameters] = useSearchParams();
  const id = currentQueryParameters.get('id');
  const queryClient = useQueryClient();
  const handleResume = useCallback(
    async (param: { resume: boolean }) => {
      const { data } = await dataSourceResume(id as string, param);
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['data-source-detail', id] });
        message.success(t(`message.operated`));
      }
    },
    [id, queryClient],
  );
  return { handleResume };
};

export const useDataSourceRebuild = () => {
  const { id } = useParams();
  // const [currentQueryParameters] = useSearchParams();
  // const id = currentQueryParameters.get('id');
  const handleRebuild = useCallback(
    async (param: { source_id: string }) => {
      const { data } = await dataSourceRebuild(param.source_id as string, {
        kb_id: id as string,
      });
      if (data.code === 0) {
        // queryClient.invalidateQueries({ queryKey: ['data-source-detail', id] });
        message.success(t(`message.operated`));
      }
    },
    [id],
  );
  return { handleRebuild };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/data-source/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 210 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (7)

- `useListDataSource`: Exported entity
- `useAddDataSource`: Exported entity
- `useLogListDataSource`: Exported entity
- `useDeleteDataSource`: Exported entity
- `useFetchDataSourceDetail`: Exported entity
- `useDataSourceResume`: Exported entity
- `useDataSourceRebuild`: Exported entity

### Functions (14)

- `useListDataSource()`: Function definition
- `categorizeDataBySource()`: Function definition
- `updatedDataSourceTemplates()`: Function definition
- `useAddDataSource()`: Function definition
- `showAddingModal()`: Function definition
- `handleAddOk()`: Function definition
- `useLogListDataSource()`: Function definition
- `useDeleteDataSource()`: Function definition
- `handleDelete()`: Function definition
- `useFetchDataSourceDetail()`: Function definition
- `useDataSourceResume()`: Function definition
- `handleResume()`: Function definition
- `useDataSourceRebuild()`: Function definition
- `handleRebuild()`: Function definition

### Imports (10)

- `import message from '@/components/ui/message';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useGetPaginationWithRouter } from '@/hooks/logic-hooks';`
- `import dataSourceService, {`
- `import { useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { t } from 'i18next';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { useParams, useSearchParams } from 'umi';`
- `import { DataSourceInfo, DataSourceKey } from './contant';`
- `import { IDataSorceInfo, IDataSource, IDataSourceBase } from './interface';`

## Code Structure Analysis

- Total lines: 210
- Blank lines: 18 (8.6%)
- Comment lines: ~5 (2.4%)
- Code lines: ~187


## Dependencies and Imports

- `@/components/ui/message`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `@tanstack/react-query`
- `i18next`
- `react`
- `umi`
- `./contant`
- `./interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/data-source`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/data-source/` directory
- Potential test file: `test_hooks.ts`

## Keywords

./contant, ./interface, @/components/ui/message, @/hooks/common-hooks, @/hooks/logic-hooks, @tanstack/react-query, Array, DataSourceInfo, DataSourceKey, IDataSorceInfo, IDataSource, IDataSourceBase, Object, Record, TypeScript, categorizeDataBySource, categorizedData, handleAddOk, handleDelete, handleRebuild, handleResume, i18next, id, k, queryClient, react, showAddingModal, source, sourcelist, tanstack, umi, updatedDataSourceTemplates, useAddDataSource, useDataSourceRebuild, useDataSourceResume, useDeleteDataSource, useFetchDataSourceDetail, useListDataSource, useLogListDataSource

---
*Generated by RAGFlow Repository Documentation Generator*
