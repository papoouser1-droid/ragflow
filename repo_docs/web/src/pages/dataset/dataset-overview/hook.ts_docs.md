# File Documentation: web/src/pages/dataset/dataset-overview/hook.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-overview/hook.ts`
- **Extension**: `.ts`
- **Lines**: 99
- **Characters**: 2,867
- **Size**: 2,867 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useHandleFilterSubmit } from '@/components/list-filter-bar/use-handle-filter-submit';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from '@/hooks/logic-hooks';
import kbService, {
  listDataPipelineLogDocument,
  listPipelineDatasetLogs,
} from '@/services/knowledge-service';
import { useQuery } from '@tanstack/react-query';
import { useCallback, useState } from 'react';
import { useParams, useSearchParams } from 'umi';
import { LogTabs } from './dataset-common';
import { IFileLogList, IOverviewTotal } from './interface';

const useFetchOverviewTital = () => {
  const [searchParams] = useSearchParams();
  const { id } = useParams();
  const knowledgeBaseId = searchParams.get('id') || id;
  const { data } = useQuery<IOverviewTotal>({
    queryKey: ['overviewTotal'],
    queryFn: async () => {
      const { data: res = {} } = await kbService.getKnowledgeBasicInfo({
        kb_id: knowledgeBaseId,
      });
      return res.data || [];
    },
  });
  return { data };
};

const useFetchFileLogList = () => {
  const [searchParams] = useSearchParams();
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const { filterValue, setFilterValue, handleFilterSubmit } =
    useHandleFilterSubmit();
  const { id } = useParams();
  const [active, setActive] = useState<(typeof LogTabs)[keyof typeof LogTabs]>(
    LogTabs.FILE_LOGS,
  );
  const knowledgeBaseId = searchParams.get('id') || id;
  const fetchFunc =
    active === LogTabs.DATASET_LOGS
      ? listPipelineDatasetLogs
      : listDataPipelineLogDocument;
  const { data } = useQuery<IFileLogList>({
    queryKey: [
      'fileLogList',
      knowledgeBaseId,
      pagination,
      searchString,
      active,
      filterValue,
    ],
    placeholderData: (previousData) => {
      if (previousData === undefined) {
        return { logs: [], total: 0 };
      }
      return previousData;
    },
    enabled: true,
    queryFn: async () => {
      const { data: res = {} } = await fetchFunc(
        {
          kb_id: knowledgeBaseId,
          page: pagination.current,
          page_size: pagination.pageSize,
          keywords: searchString,
          // order_by: '',
        },
        { ...filterValue },
      );
      return res.data || [];
    },
  });
  const onInputChange: React.ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      setPagination({ page: 1 });
      handleInputChange(e);
    },
    [handleInputChange, setPagination],
  );
  return {
    data,
    searchString,
    handleInputChange: onInputChange,
    pagination: { ...pagination, total: data?.total },
    setPagination,
    active,
    setActive,
    filterValue,
    setFilterValue,
    handleFilterSubmit,
  };
};

export { useFetchFileLogList, useFetchOverviewTital };

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-overview/hook.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 99 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `useFetchOverviewTital()`: Function definition
- `useFetchFileLogList()`: Function definition

### Imports (8)

- `import { useHandleFilterSubmit } from '@/components/list-filter-bar/use-handle-filter-submit';`
- `import {`
- `import kbService, {`
- `import { useQuery } from '@tanstack/react-query';`
- `import { useCallback, useState } from 'react';`
- `import { useParams, useSearchParams } from 'umi';`
- `import { LogTabs } from './dataset-common';`
- `import { IFileLogList, IOverviewTotal } from './interface';`

## Code Structure Analysis

- Total lines: 99
- Blank lines: 4 (4.0%)
- Comment lines: ~1 (1.0%)
- Code lines: ~94


## Dependencies and Imports

- `@/components/list-filter-bar/use-handle-filter-submit`
- `@tanstack/react-query`
- `react`
- `umi`
- `./dataset-common`
- `./interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-overview`.

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

- Other files in `web/src/pages/dataset/dataset-overview/` directory
- Potential test file: `test_hook.ts`

## Keywords

./dataset-common, ./interface, @/components/list-filter-bar/use-handle-filter-submit, @tanstack/react-query, ChangeEventHandler, DATASET_LOGS, FILE_LOGS, HTMLInputElement, IFileLogList, IOverviewTotal, LogTabs, React, TypeScript, fetchFunc, knowledgeBaseId, onInputChange, react, tanstack, umi, useFetchFileLogList, useFetchOverviewTital

---
*Generated by RAGFlow Repository Documentation Generator*
