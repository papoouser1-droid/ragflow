# File Documentation: web/src/hooks/use-chunk-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-chunk-request.ts`
- **Extension**: `.ts`
- **Lines**: 121
- **Characters**: 3,240
- **Size**: 3,240 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-chunk-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 121 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `useFetchNextChunkList`: Exported entity
- `useSwitchChunk`: Exported entity

### Functions (2)

- `handleSetAvailable()`: Function definition
- `useSwitchChunk()`: Function definition

### Imports (11)

- `import message from '@/components/ui/message';`
- `import { ResponseGetType } from '@/interfaces/database/base';`
- `import { IChunk, IKnowledgeFile } from '@/interfaces/database/knowledge';`
- `import kbService from '@/services/knowledge-service';`
- `import { useMutation, useQuery } from '@tanstack/react-query';`
- `import { useDebounce } from 'ahooks';`
- `import { useCallback, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { IChunkListResult } from './chunk-hooks';`
- `import {`

## Code Structure Analysis

- Total lines: 121
- Blank lines: 9 (7.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~112


## Dependencies and Imports

- `@/components/ui/message`
- `@/interfaces/database/base`
- `@/interfaces/database/knowledge`
- `@/services/knowledge-service`
- `@tanstack/react-query`
- `ahooks`
- `react`
- `react-i18next`
- `./chunk-hooks`
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
- Potential test file: `test_use-chunk-request.ts`

## Keywords

./chunk-hooks, ./route-hook, @/components/ui/message, @/interfaces/database/base, @/interfaces/database/knowledge, @/services/knowledge-service, @tanstack/react-query, ChangeEventHandler, HTMLInputElement, IChunk, IChunkListResult, IKnowledgeFile, React, ResponseGetType, TanStack, TypeScript, ahooks, debouncedSearchString, handleSetAvailable, onInputChange, react, react-i18next, res, tanstack, useFetchNextChunkList, useSwitchChunk

---
*Generated by RAGFlow Repository Documentation Generator*
