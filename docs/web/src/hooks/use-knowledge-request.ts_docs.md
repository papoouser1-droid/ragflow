# Documentation: web/src/hooks/use-knowledge-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-knowledge-request.ts`
- **Size**: 9903 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/use-knowledge-request.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/use-knowledge-request.ts` is located in the `web/src/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [auth-hooks.ts](auth-hooks.ts_docs.md)
- [chat-hooks.ts](chat-hooks.ts_docs.md)
- [chunk-hooks.ts](chunk-hooks.ts_docs.md)
- [common-hooks.tsx](common-hooks.tsx_docs.md)
- [document-hooks.ts](document-hooks.ts_docs.md)
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
