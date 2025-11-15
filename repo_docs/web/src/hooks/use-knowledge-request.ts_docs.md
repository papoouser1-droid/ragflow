# File Documentation: web/src/hooks/use-knowledge-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-knowledge-request.ts`
- **Extension**: `.ts`
- **Lines**: 357
- **Characters**: 9,903
- **Size**: 9,903 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useHandleFilterSubmit } from '@/components/list-filter-bar/use-handle-filter-submit';
import message from '@/components/ui/message';
import {
  IKnowledge,
  IKnowledgeGraph,
  IKnowledgeResult,
  INextTestingResult,
} from '@/interfaces/database/knowledge';
import { ITestRetrievalRequestBody } from '@/interfaces/request/knowledge';
import i18n from '@/locales/config';
import kbService, {
  deleteKnowledgeGraph,
  getKnowledgeGraph,
  listDataset,
} from '@/services/knowledge-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { useCallback, useEffect, useMemo, useRef, useState } from 'react';
import { useParams, useSearchParams } from 'umi';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';

export const enum KnowledgeApiAction {
  TestRetrieval = 'testRetrieval',
  FetchKnowledgeListByPage = 'fetchKnowledgeListByPage',
  CreateKnowledge = 'createKnowledge',
  DeleteKnowledge = 'deleteKnowledge',
  SaveKnowledge = 'saveKnowledge',
  FetchKnowledgeDetail = 'fetchKnowledgeDetail',
  FetchKnowledgeGraph = 'fetchKnowledgeGraph',
  FetchMetadata = 'fetchMetadata',
  FetchKnowledgeList = 'fetchKnowledgeList',
  RemoveKnowledgeGraph = 'removeKnowledgeGraph',
}

export const useKnowledgeBaseId = (): string => {
  const { id } = useParams();

  return (id as string) || '';
};

export const useTestRetrieval = () => {
  const knowledgeBaseId = useKnowledgeBaseId();
  const [values, setValues] = useState<ITestRetrievalRequestBody>();
  const mountedRef = useRef(false);
  const { filterValue, handleFilterSubmit } = useHandleFilterSubmit();

  const [page, setPage] = useState(1);
  const [pageSize, setPageSize] = useState(10);

  const onPaginationChange = useCallback((page: number, pageSize: number) => {
    setPage(page);
    setPageSize(pageSize);
  }, []);

  const queryParams = useMemo(() => {
    return {
      ...values,
      kb_id: values?.kb_id || knowledgeBaseId,
      page,
      size: pageSize,
      doc_ids: filterValue.doc_ids,
    };
  }, [filterValue, knowledgeBaseId, page, pageSize, values]);

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<INextTestingResult>({
    queryKey: [KnowledgeApiAction.TestRetrieval, queryParams, page, pageSize],
    initialData: {
      chunks: [],
      doc_aggs: [],
      total: 0,
      isRuned: false,
    },
    enabled: false,
    gcTime: 0,
    queryFn: async () => {
      const { data } = await kbService.retrieval_test(queryParams);
      const result = data?.data ?? {};
      return { ...result, isRuned: true };
    },
  });

  useEffect(() => {
    if (mountedRef.current) {
      refetch();
    }
    mountedRef.current = true;
  }, [page, pageSize, refetch, filterValue]);

  return {
    data,
    loading,
    setValues,
    refetch,
    onPaginationChange,
    page,
    pageSize,
    handleFilterSubmit,
    filterValue,
  };
};

export const useFetchNextKnowledgeListByPage = () => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });
  const { filterValue, handleFilterSubmit } = useHandleFilterSubmit();

  const { data, isFetching: loading } = useQuery<IKnowledgeResult>({
    queryKey: [
      KnowledgeApiAction.FetchKnowledgeListByPage,
      {
        debouncedSearchString,
        ...pagination,
        filterValue,
      },
    ],
    initialData: {
      kbs: [],
      total: 0,
    },
    gcTime: 0,
    queryFn: async () => {
      const { data } = await listDataset(
        {
          keywords: debouncedSearchString,
          page_size: pagination.pageSize,
          page: pagination.current,
        },
        {
          owner_ids: filterValue.owner,
        },
      );

      return data?.data;
    },
  });

  const onInputChange: React.ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      // setPagination({ page: 1 }); // TODO: This results in repeated requests
      handleInputChange(e);
    },
    [handleInputChange],
  );

  return {
    ...data,
    searchString,
    handleInputChange: onInputChange,
    pagination: { ...pagination, total: data?.total },
    setPagination,
    loading,
    filterValue,
    handleFilterSubmit,
  };
};

export const useCreateKnowledge = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [KnowledgeApiAction.CreateKnowledge],
    mutationFn: async (params: { id?: string; name: string }) => {
      const { data = {} } = await kbService.createKb(params);
      if (data.code === 0) {
        message.success(
          i18n.t(`message.${params?.id ? 'modified' : 'created'}`),
        );
        queryClient.invalidateQueries({ queryKey: ['fetchKnowledgeList'] });
      }
      return data;
    },
  });

  return { data, loading, createKnowledge: mutateAsync };
};

export const useDeleteKnowledge = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [KnowledgeApiAction.DeleteKnowledge],
    mutationFn: async (id: string) => {
      const { data } = await kbService.rmKb({ kb_id: id });
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));
        queryClient.invalidateQueries({
          queryKey: [KnowledgeApiAction.FetchKnowledgeListByPage],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, deleteKnowledge: mutateAsync };
};

export const useUpdateKnowledge = (shouldFetchList = false) => {
  const knowledgeBaseId = useKnowledgeBaseId();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [KnowledgeApiAction.SaveKnowledge],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await kbService.updateKb({
        kb_id: params?.kb_id ? params?.kb_id : knowledgeBaseId,
        ...params,
      });
      if (data.code === 0) {
        message.success(i18n.t(`message.updated`));
        if (shouldFetchList) {
          queryClient.invalidateQueries({
            queryKey: [KnowledgeApiAction.FetchKnowledgeListByPage],
          });
        } else {
          queryClient.invalidateQueries({ queryKey: ['fetchKnowledgeDetail'] });
        }
      }
      return data;
    },
  });

  return { data, loading, saveKnowledgeConfiguration: mutateAsync };
};

export const useFetchKnowledgeBaseConfiguration = (props?: {
  isEdit?: boolean;
  refreshCount?: number;
}) => {
  const { isEdit = true, refreshCount } = props || { isEdit: true };
  const { id } = useParams();
  const [searchParams] = useSearchParams();
  const knowledgeBaseId = searchParams.get('id') || id;

  let queryKey: (KnowledgeApiAction | number)[] = [
    KnowledgeApiAction.FetchKnowledgeDetail,
  ];
  if (typeof refreshCount === 'number') {
    queryKey = [KnowledgeApiAction.FetchKnowledgeDetail, refreshCount];
  }

  const { data, isFetching: loading } = useQuery<IKnowledge>({
    queryKey,
    initialData: {} as IKnowledge,
    gcTime: 0,
    queryFn: async () => {
      if (isEdit) {
        const { data } = await kbService.get_kb_detail({
          kb_id: knowledgeBaseId,
        });
        return data?.data ?? {};
      } else {
        return {};
      }
    },
  });

  return { data, loading };
};

export function useFetchKnowledgeGraph() {
  const knowledgeBaseId = useKnowledgeBaseId();

  const { data, isFetching: loading } = useQuery<IKnowledgeGraph>({
    queryKey: [KnowledgeApiAction.FetchKnowledgeGraph, knowledgeBaseId],
    initialData: { graph: {}, mind_map: {} } as IKnowledgeGraph,
    enabled: !!knowledgeBaseId,
    gcTime: 0,
    queryFn: async () => {
      const { data } = await getKnowledgeGraph(knowledgeBaseId);
      return data?.data;
    },
  });

  return { data, loading };
}

export function useFetchKnowledgeMetadata(kbIds: string[] = []) {
  const { data, isFetching: loading } = useQuery<
    Record<string, Record<string, string[]>>
  >({
    queryKey: [KnowledgeApiAction.FetchMetadata, kbIds],
    initialData: {},
    enabled: kbIds.length > 0,
    gcTime: 0,
    queryFn: async () => {
      const { data } = await kbService.getMeta({ kb_ids: kbIds.join(',') });
      return data?.data ?? {};
    },
  });

  return { data, loading };
}

export const useRemoveKnowledgeGraph = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [KnowledgeApiAction.RemoveKnowledgeGraph],
    mutationFn: async () => {
      const { data } = await deleteKnowledgeGraph(knowledgeBaseId);
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));
        queryClient.invalidateQueries({
          queryKey: [KnowledgeApiAction.FetchKnowledgeGraph],
        });
      }
      return data?.code;
    },
  });

  return { data, loading, removeKnowledgeGraph: mutateAsync };
};

export const useFetchKnowledgeList = (
  shouldFilterListWithoutDocument: boolean = false,
): {
  list: IKnowledge[];
  loading: boolean;
} => {
  const { data, isFetching: loading } = useQuery({
    queryKey: [KnowledgeApiAction.FetchKnowledgeList],
    initialData: [],
    gcTime: 0, // https://tanstack.com/query/latest/docs/framework/react/guides/caching?from=reactQueryV3
    queryFn: async () => {
      const { data } = await listDataset();
      const list = data?.data?.kbs ?? [];
      return shouldFilterListWithoutDocument
        ? list.filter((x: IKnowledge) => x.chunk_num > 0)
        : list;
    },
  });

  return { list: data, loading };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-knowledge-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 357 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (12)

- `enum`: Exported entity
- `useKnowledgeBaseId`: Exported entity
- `useTestRetrieval`: Exported entity
- `useFetchNextKnowledgeListByPage`: Exported entity
- `useCreateKnowledge`: Exported entity
- `useDeleteKnowledge`: Exported entity
- `useUpdateKnowledge`: Exported entity
- `useFetchKnowledgeBaseConfiguration`: Exported entity
- `useFetchKnowledgeGraph`: Exported entity
- `useFetchKnowledgeMetadata`: Exported entity
- `useRemoveKnowledgeGraph`: Exported entity
- `useFetchKnowledgeList`: Exported entity

### Functions (14)

- `useKnowledgeBaseId()`: Function definition
- `useTestRetrieval()`: Function definition
- `onPaginationChange()`: Function definition
- `queryParams()`: Function definition
- `result()`: Function definition
- `useFetchNextKnowledgeListByPage()`: Function definition
- `useCreateKnowledge()`: Function definition
- `useDeleteKnowledge()`: Function definition
- `useUpdateKnowledge()`: Function definition
- `useFetchKnowledgeBaseConfiguration()`: Function definition
- `useFetchKnowledgeGraph()`: Function definition
- `useFetchKnowledgeMetadata()`: Function definition
- `useRemoveKnowledgeGraph()`: Function definition
- `list()`: Function definition

### Imports (11)

- `import { useHandleFilterSubmit } from '@/components/list-filter-bar/use-handle-filter-submit';`
- `import message from '@/components/ui/message';`
- `import {`
- `import { ITestRetrievalRequestBody } from '@/interfaces/request/knowledge';`
- `import i18n from '@/locales/config';`
- `import kbService, {`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { useDebounce } from 'ahooks';`
- `import { useCallback, useEffect, useMemo, useRef, useState } from 'react';`
- `import { useParams, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 357
- Blank lines: 36 (10.1%)
- Comment lines: ~1 (0.3%)
- Code lines: ~320


## Dependencies and Imports

- `@/components/list-filter-bar/use-handle-filter-submit`
- `@/components/ui/message`
- `@/interfaces/request/knowledge`
- `@/locales/config`
- `@tanstack/react-query`
- `ahooks`
- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/hooks/` directory
- Potential test file: `test_use-knowledge-request.ts`

## Keywords

@/components/list-filter-bar/use-handle-filter-submit, @/components/ui/message, @/interfaces/request/knowledge, @/locales/config, @tanstack/react-query, ChangeEventHandler, CreateKnowledge, DeleteKnowledge, FetchKnowledgeDetail, FetchKnowledgeGraph, FetchKnowledgeList, FetchKnowledgeListByPage, FetchMetadata, HTMLInputElement, IKnowledge, IKnowledgeGraph, IKnowledgeResult, INextTestingResult, ITestRetrievalRequestBody, KnowledgeApiAction, React, Record, RemoveKnowledgeGraph, SaveKnowledge, TODO, TestRetrieval, This, TypeScript, ahooks, debouncedSearchString, enum, knowledgeBaseId, list, mountedRef, onInputChange, onPaginationChange, queryClient, queryKey, queryParams, react, result, tanstack, umi, useCreateKnowledge, useDeleteKnowledge, useFetchKnowledgeBaseConfiguration, useFetchKnowledgeGraph, useFetchKnowledgeList, useFetchKnowledgeMetadata, useFetchNextKnowledgeListByPage...

---
*Generated by RAGFlow Repository Documentation Generator*
