# Documentation: web/src/hooks/chunk-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/chunk-hooks.ts`
- **Size**: 5691 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/chunk-hooks.ts`.

## Original Source Code

```ts
import { ResponseGetType, ResponseType } from '@/interfaces/database/base';
import { IChunk, IKnowledgeFile } from '@/interfaces/database/knowledge';
import kbService from '@/services/knowledge-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { PaginationProps, message } from 'antd';
import { useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import {
  useGetKnowledgeSearchParams,
  useSetPaginationParams,
} from './route-hook';

export interface IChunkListResult {
  searchString?: string;
  handleInputChange?: React.ChangeEventHandler<HTMLInputElement>;
  pagination: PaginationProps;
  setPagination?: (pagination: { page: number; pageSize: number }) => void;
  available: number | undefined;
  handleSetAvailable: (available: number | undefined) => void;
}

export const useFetchNextChunkList = (): ResponseGetType<{
  data: IChunk[];
  total: number;
  documentInfo: IKnowledgeFile;
}> &
  IChunkListResult => {
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const { documentId } = useGetKnowledgeSearchParams();
  const { searchString, handleInputChange } = useHandleSearchChange();
  const [available, setAvailable] = useState<number | undefined>();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });

  const { data, isFetching: loading } = useQuery({
    queryKey: [
      'fetchChunkList',
      documentId,
      pagination.current,
      pagination.pageSize,
      debouncedSearchString,
      available,
    ],
    placeholderData: (previousData) =>
      previousData ?? { data: [], total: 0, documentInfo: {} }, // https://github.com/TanStack/query/issues/8183
    gcTime: 0,
    queryFn: async () => {
      const { data } = await kbService.chunk_list({
        doc_id: documentId,
        page: pagination.current,
        size: pagination.pageSize,
        available_int: available,
        keywords: searchString,
      });
      if (data.code === 0) {
        const res = data.data;
        return {
          data: res.chunks,
          total: res.total,
          documentInfo: res.doc,
        };
      }

      return (
        data?.data ?? {
          data: [],
          total: 0,
          documentInfo: {},
        }
      );
    },
  });

  const onInputChange: React.ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      setPagination({ page: 1 });
      handleInputChange(e);
    },
    [handleInputChange, setPagination],
  );

  const handleSetAvailable = useCallback(
    (a: number | undefined) => {
      setPagination({ page: 1 });
      setAvailable(a);
    },
    [setAvailable, setPagination],
  );

  return {
    data,
    loading,
    pagination,
    setPagination,
    searchString,
    handleInputChange: onInputChange,
    available,
    handleSetAvailable,
  };
};

export const useSelectChunkList = () => {
  const queryClient = useQueryClient();
  const data = queryClient.getQueriesData<{
    data: IChunk[];
    total: number;
    documentInfo: IKnowledgeFile;
  }>({ queryKey: ['fetchChunkList'] });

  return data?.at(-1)?.[1];
};

export const useDeleteChunk = () => {
  const queryClient = useQueryClient();
  const { setPaginationParams } = useSetPaginationParams();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteChunk'],
    mutationFn: async (params: { chunkIds: string[]; doc_id: string }) => {
      const { data } = await kbService.rm_chunk(params);
      if (data.code === 0) {
        setPaginationParams(1);
        queryClient.invalidateQueries({ queryKey: ['fetchChunkList'] });
      }
      return data?.code;
    },
  });

  return { data, loading, deleteChunk: mutateAsync };
};

export const useSwitchChunk = () => {
  const { t } = useTranslation();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['switchChunk'],
    mutationFn: async (params: {
      chunk_ids?: string[];
      available_int?: number;
      doc_id: string;
    }) => {
      const { data } = await kbService.switch_chunk(params);
      if (data.code === 0) {
        message.success(t('message.modified'));
        queryClient.invalidateQueries({ queryKey: ['fetchChunkList'] });
      }
      return data?.code;
    },
  });

  return { data, loading, switchChunk: mutateAsync };
};

export const useCreateChunk = () => {
  const { t } = useTranslation();
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['createChunk'],
    mutationFn: async (payload: any) => {
      let service = kbService.create_chunk;
      if (payload.chunk_id) {
        service = kbService.set_chunk;
      }
      const { data } = await service(payload);
      if (data.code === 0) {
        message.success(t('message.created'));
        setTimeout(() => {
          queryClient.invalidateQueries({ queryKey: ['fetchChunkList'] });
        }, 1000); // Delay to ensure the list is updated
      }
      return data?.code;
    },
  });

  return { data, loading, createChunk: mutateAsync };
};

export const useFetchChunk = (chunkId?: string): ResponseType<any> => {
  const { data } = useQuery({
    queryKey: ['fetchChunk'],
    enabled: !!chunkId,
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const data = await kbService.get_chunk({
        chunk_id: chunkId,
      });

      return data;
    },
  });

  return data;
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/chunk-hooks.ts` is located in the `web/src/hooks` directory.

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
- [common-hooks.tsx](common-hooks.tsx_docs.md)
- [document-hooks.ts](document-hooks.ts_docs.md)
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)
- [login-hooks.ts](login-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
