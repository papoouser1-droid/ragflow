# Documentation: web/src/hooks/use-chunk-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-chunk-request.ts`
- **Size**: 3240 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/use-chunk-request.ts`.

## Original Source Code

```ts
import message from '@/components/ui/message';
import { ResponseGetType } from '@/interfaces/database/base';
import { IChunk, IKnowledgeFile } from '@/interfaces/database/knowledge';
import kbService from '@/services/knowledge-service';
import { useMutation, useQuery } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { IChunkListResult } from './chunk-hooks';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import { useGetKnowledgeSearchParams } from './route-hook';

export const useFetchNextChunkList = (
  enabled = true,
): ResponseGetType<{
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
    placeholderData: (previousData: any) =>
      previousData ?? { data: [], total: 0, documentInfo: {} }, // https://github.com/TanStack/query/issues/8183
    gcTime: 0,
    enabled,
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

export const useSwitchChunk = () => {
  const { t } = useTranslation();
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
      }
      return data?.code;
    },
  });

  return { data, loading, switchChunk: mutateAsync };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/use-chunk-request.ts` is located in the `web/src/hooks` directory.

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
