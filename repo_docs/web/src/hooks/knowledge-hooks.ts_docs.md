# File Documentation: web/src/hooks/knowledge-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/knowledge-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 499
- **Characters**: 12,684
- **Size**: 12,684 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { ResponsePostType } from '@/interfaces/database/base';
import {
  IKnowledge,
  IKnowledgeGraph,
  IRenameTag,
  ITestingResult,
} from '@/interfaces/database/knowledge';
import i18n from '@/locales/config';
import kbService, {
  deleteKnowledgeGraph,
  getKnowledgeGraph,
  listDataset,
  listTag,
  removeTag,
  renameTag,
} from '@/services/knowledge-service';
import {
  useInfiniteQuery,
  useIsMutating,
  useMutation,
  useMutationState,
  useQuery,
  useQueryClient,
} from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { message } from 'antd';
import { useState } from 'react';
import { useParams, useSearchParams } from 'umi';
import { useHandleSearchChange } from './logic-hooks';
import { useSetPaginationParams } from './route-hook';

export const useKnowledgeBaseId = (): string => {
  const [searchParams] = useSearchParams();
  const { id } = useParams();
  const knowledgeBaseId = searchParams.get('id') || id;

  return knowledgeBaseId || '';
};

export const useFetchKnowledgeBaseConfiguration = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const { data, isFetching: loading } = useQuery<IKnowledge>({
    queryKey: ['fetchKnowledgeDetail'],
    initialData: {} as IKnowledge,
    gcTime: 0,
    queryFn: async () => {
      const { data } = await kbService.get_kb_detail({
        kb_id: knowledgeBaseId,
      });
      return data?.data ?? {};
    },
  });

  return { data, loading };
};

export const useFetchKnowledgeList = (
  shouldFilterListWithoutDocument: boolean = false,
): {
  list: IKnowledge[];
  loading: boolean;
} => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['fetchKnowledgeList'],
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

export const useSelectKnowledgeOptions = () => {
  const { list } = useFetchKnowledgeList();

  const options = list?.map((item) => ({
    label: item.name,
    value: item.id,
  }));

  return options;
};

export const useInfiniteFetchKnowledgeList = () => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });

  const PageSize = 30;

  const {
    data,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
  } = useInfiniteQuery({
    queryKey: ['infiniteFetchKnowledgeList', debouncedSearchString],
    queryFn: async ({ pageParam }) => {
      const { data } = await listDataset({
        page: pageParam,
        page_size: PageSize,
        keywords: debouncedSearchString,
      });
      const list = data?.data ?? [];
      return list;
    },
    initialPageParam: 1,
    getNextPageParam: (lastPage, pages, lastPageParam) => {
      if (lastPageParam * PageSize <= lastPage.total) {
        return lastPageParam + 1;
      }
      return undefined;
    },
  });
  return {
    data,
    loading: isFetching,
    error,
    fetchNextPage,
    hasNextPage,
    isFetching,
    isFetchingNextPage,
    status,
    handleInputChange,
    searchString,
  };
};

export const useCreateKnowledge = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['infiniteFetchKnowledgeList'],
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
    mutationKey: ['deleteKnowledge'],
    mutationFn: async (id: string) => {
      const { data } = await kbService.rmKb({ kb_id: id });
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));
        queryClient.invalidateQueries({
          queryKey: ['infiniteFetchKnowledgeList'],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, deleteKnowledge: mutateAsync };
};

//#region knowledge configuration

export const useUpdateKnowledge = (shouldFetchList = false) => {
  const knowledgeBaseId = useKnowledgeBaseId();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['saveKnowledge'],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await kbService.updateKb({
        kb_id: params?.kb_id ? params?.kb_id : knowledgeBaseId,
        ...params,
      });
      if (data.code === 0) {
        message.success(i18n.t(`message.updated`));
        if (shouldFetchList) {
          queryClient.invalidateQueries({
            queryKey: ['fetchKnowledgeListByPage'],
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

//#endregion

//#region Retrieval testing

export const useTestChunkRetrieval = (): ResponsePostType<ITestingResult> & {
  testChunk: (...params: any[]) => void;
} => {
  const knowledgeBaseId = useKnowledgeBaseId();
  const { page, size: pageSize } = useSetPaginationParams();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['testChunk'], // This method is invalid
    gcTime: 0,
    mutationFn: async (values: any) => {
      const { data } = await kbService.retrieval_test({
        ...values,
        kb_id: values.kb_id ?? knowledgeBaseId,
        page,
        size: pageSize,
      });
      if (data.code === 0) {
        const res = data.data;
        return {
          ...res,
          documents: res.doc_aggs,
        };
      }
      return (
        data?.data ?? {
          chunks: [],
          documents: [],
          total: 0,
        }
      );
    },
  });

  return {
    data: data ?? { chunks: [], documents: [], total: 0 },
    loading,
    testChunk: mutateAsync,
  };
};

export const useTestChunkAllRetrieval = (): ResponsePostType<ITestingResult> & {
  testChunkAll: (...params: any[]) => void;
} => {
  const knowledgeBaseId = useKnowledgeBaseId();
  const { page, size: pageSize } = useSetPaginationParams();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['testChunkAll'], // This method is invalid
    gcTime: 0,
    mutationFn: async (values: any) => {
      const { data } = await kbService.retrieval_test({
        ...values,
        kb_id: values.kb_id ?? knowledgeBaseId,
        doc_ids: [],
        page,
        size: pageSize,
      });
      if (data.code === 0) {
        const res = data.data;
        return {
          ...res,
          documents: res.doc_aggs,
        };
      }
      return (
        data?.data ?? {
          chunks: [],
          documents: [],
          total: 0,
        }
      );
    },
  });

  return {
    data: data ?? { chunks: [], documents: [], total: 0 },
    loading,
    testChunkAll: mutateAsync,
  };
};

export const useChunkIsTesting = () => {
  return useIsMutating({ mutationKey: ['testChunk'] }) > 0;
};

export const useSelectTestingResult = (): ITestingResult => {
  const data = useMutationState({
    filters: { mutationKey: ['testChunk'] },
    select: (mutation) => {
      return mutation.state.data;
    },
  });
  return (data.at(-1) ?? {
    chunks: [],
    documents: [],
    total: 0,
  }) as ITestingResult;
};

export const useSelectIsTestingSuccess = () => {
  const status = useMutationState({
    filters: { mutationKey: ['testChunk'] },
    select: (mutation) => {
      return mutation.state.status;
    },
  });
  return status.at(-1) === 'success';
};

export const useAllTestingSuccess = () => {
  const status = useMutationState({
    filters: { mutationKey: ['testChunkAll'] },
    select: (mutation) => {
      return mutation.state.status;
    },
  });
  return status.at(-1) === 'success';
};

export const useAllTestingResult = (): ITestingResult => {
  const data = useMutationState({
    filters: { mutationKey: ['testChunkAll'] },
    select: (mutation) => {
      return mutation.state.data;
    },
  });
  return (data.at(-1) ?? {
    chunks: [],
    documents: [],
    total: 0,
  }) as ITestingResult;
};
//#endregion

//#region tags

export const useFetchTagList = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const { data, isFetching: loading } = useQuery<Array<[string, number]>>({
    queryKey: ['fetchTagList'],
    initialData: [],
    gcTime: 0, // https://tanstack.com/query/latest/docs/framework/react/guides/caching?from=reactQueryV3
    queryFn: async () => {
      const { data } = await listTag(knowledgeBaseId);
      const list = data?.data || [];
      return list;
    },
  });

  return { list: data, loading };
};

export const useDeleteTag = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteTag'],
    mutationFn: async (tags: string[]) => {
      const { data } = await removeTag(knowledgeBaseId, tags);
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));
        queryClient.invalidateQueries({
          queryKey: ['fetchTagList'],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, deleteTag: mutateAsync };
};

export const useRenameTag = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['renameTag'],
    mutationFn: async (params: IRenameTag) => {
      const { data } = await renameTag(knowledgeBaseId, params);
      if (data.code === 0) {
        message.success(i18n.t(`message.modified`));
        queryClient.invalidateQueries({
          queryKey: ['fetchTagList'],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, renameTag: mutateAsync };
};

export const useTagIsRenaming = () => {
  return useIsMutating({ mutationKey: ['renameTag'] }) > 0;
};

export const useFetchTagListByKnowledgeIds = () => {
  const [knowledgeIds, setKnowledgeIds] = useState<string[]>([]);

  const { data, isFetching: loading } = useQuery<Array<[string, number]>>({
    queryKey: ['fetchTagListByKnowledgeIds'],
    enabled: knowledgeIds.length > 0,
    initialData: [],
    gcTime: 0, // https://tanstack.com/query/latest/docs/framework/react/guides/caching?from=reactQueryV3
    queryFn: async () => {
      const { data } = await kbService.listTagByKnowledgeIds({
        kb_ids: knowledgeIds.join(','),
      });
      const list = data?.data || [];
      return list;
    },
  });

  return { list: data, loading, setKnowledgeIds };
};

//#endregion

export function useFetchKnowledgeGraph() {
  const knowledgeBaseId = useKnowledgeBaseId();

  const { data, isFetching: loading } = useQuery<IKnowledgeGraph>({
    queryKey: ['fetchKnowledgeGraph', knowledgeBaseId],
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

export const useRemoveKnowledgeGraph = () => {
  const knowledgeBaseId = useKnowledgeBaseId();

  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['removeKnowledgeGraph'],
    mutationFn: async () => {
      const { data } = await deleteKnowledgeGraph(knowledgeBaseId);
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));
        queryClient.invalidateQueries({
          queryKey: ['fetchKnowledgeGraph'],
        });
      }
      return data?.code;
    },
  });

  return { data, loading, removeKnowledgeGraph: mutateAsync };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/knowledge-hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 499 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (22)

- `useKnowledgeBaseId`: Exported entity
- `useFetchKnowledgeBaseConfiguration`: Exported entity
- `useFetchKnowledgeList`: Exported entity
- `useSelectKnowledgeOptions`: Exported entity
- `useInfiniteFetchKnowledgeList`: Exported entity
- `useCreateKnowledge`: Exported entity
- `useDeleteKnowledge`: Exported entity
- `useUpdateKnowledge`: Exported entity
- `useTestChunkRetrieval`: Exported entity
- `useTestChunkAllRetrieval`: Exported entity
- `useChunkIsTesting`: Exported entity
- `useSelectTestingResult`: Exported entity
- `useSelectIsTestingSuccess`: Exported entity
- `useAllTestingSuccess`: Exported entity
- `useAllTestingResult`: Exported entity
- `useFetchTagList`: Exported entity
- `useDeleteTag`: Exported entity
- `useRenameTag`: Exported entity
- `useTagIsRenaming`: Exported entity
- `useFetchTagListByKnowledgeIds`: Exported entity

### Functions (28)

- `useKnowledgeBaseId()`: Function definition
- `useFetchKnowledgeBaseConfiguration()`: Function definition
- `list()`: Function definition
- `useSelectKnowledgeOptions()`: Function definition
- `options()`: Function definition
- `useInfiniteFetchKnowledgeList()`: Function definition
- `list()`: Function definition
- `useCreateKnowledge()`: Function definition
- `useDeleteKnowledge()`: Function definition
- `useUpdateKnowledge()`: Function definition
- `useTestChunkRetrieval()`: Function definition
- `useTestChunkAllRetrieval()`: Function definition
- `useChunkIsTesting()`: Function definition
- `useSelectTestingResult()`: Function definition
- `data()`: Function definition
- `useSelectIsTestingSuccess()`: Function definition
- `status()`: Function definition
- `useAllTestingSuccess()`: Function definition
- `status()`: Function definition
- `useAllTestingResult()`: Function definition

### Imports (11)

- `import { ResponsePostType } from '@/interfaces/database/base';`
- `import {`
- `import i18n from '@/locales/config';`
- `import kbService, {`
- `import {`
- `import { useDebounce } from 'ahooks';`
- `import { message } from 'antd';`
- `import { useState } from 'react';`
- `import { useParams, useSearchParams } from 'umi';`
- `import { useHandleSearchChange } from './logic-hooks';`

## Code Structure Analysis

- Total lines: 499
- Blank lines: 55 (11.0%)
- Comment lines: ~6 (1.2%)
- Code lines: ~438


## Dependencies and Imports

- `@/interfaces/database/base`
- `@/locales/config`
- `ahooks`
- `antd`
- `react`
- `umi`
- `./logic-hooks`
- `./route-hook`

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
- Potential test file: `test_knowledge-hooks.ts`

## Keywords

./logic-hooks, ./route-hook, @/interfaces/database/base, @/locales/config, Array, IKnowledge, IKnowledgeGraph, IRenameTag, ITestingResult, PageSize, Record, ResponsePostType, Retrieval, This, TypeScript, ahooks, antd, data, debouncedSearchString, knowledgeBaseId, list, options, queryClient, react, res, status, tanstack, umi, useAllTestingResult, useAllTestingSuccess, useChunkIsTesting, useCreateKnowledge, useDeleteKnowledge, useDeleteTag, useFetchKnowledgeBaseConfiguration, useFetchKnowledgeGraph, useFetchKnowledgeList, useFetchTagList, useFetchTagListByKnowledgeIds, useInfiniteFetchKnowledgeList, useKnowledgeBaseId, useRemoveKnowledgeGraph, useRenameTag, useSelectIsTestingSuccess, useSelectKnowledgeOptions, useSelectTestingResult, useTagIsRenaming, useTestChunkAllRetrieval, useTestChunkRetrieval, useUpdateKnowledge

---
*Generated by RAGFlow Repository Documentation Generator*
