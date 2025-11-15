# File Documentation: web/src/hooks/route-hook.ts

## File Metadata

- **Path**: `web/src/hooks/route-hook.ts`
- **Extension**: `.ts`
- **Lines**: 92
- **Characters**: 2,448
- **Size**: 2,448 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  KnowledgeRouteKey,
  KnowledgeSearchParams,
} from '@/constants/knowledge';
import { useCallback } from 'react';
import { useLocation, useNavigate, useSearchParams } from 'umi';

export enum SegmentIndex {
  Second = '2',
  Third = '3',
}

export const useSegmentedPathName = (index: SegmentIndex) => {
  const { pathname } = useLocation();

  const pathArray = pathname.split('/');
  return pathArray[index] || '';
};

export const useSecondPathName = () => {
  return useSegmentedPathName(SegmentIndex.Second);
};

export const useThirdPathName = () => {
  return useSegmentedPathName(SegmentIndex.Third);
};

export const useGetKnowledgeSearchParams = () => {
  const [currentQueryParameters] = useSearchParams();

  return {
    type: currentQueryParameters.get(KnowledgeSearchParams.Type) || '',
    documentId:
      currentQueryParameters.get(KnowledgeSearchParams.DocumentId) || '',
    knowledgeId:
      currentQueryParameters.get(KnowledgeSearchParams.KnowledgeId) || '',
  };
};

export const useNavigateWithFromState = () => {
  const navigate = useNavigate();
  return useCallback(
    (path: string) => {
      navigate(path, { state: { from: path } });
    },
    [navigate],
  );
};

export const useNavigateToDataset = () => {
  const navigate = useNavigate();
  const { knowledgeId } = useGetKnowledgeSearchParams();

  return useCallback(() => {
    navigate(`/knowledge/${KnowledgeRouteKey.Dataset}?id=${knowledgeId}`);
  }, [knowledgeId, navigate]);
};

export const useGetPaginationParams = () => {
  const [currentQueryParameters] = useSearchParams();

  return {
    page: currentQueryParameters.get('page') || 1,
    size: currentQueryParameters.get('size') || 10,
  };
};

export const useSetPaginationParams = () => {
  const [queryParameters, setSearchParams] = useSearchParams();
  // const newQueryParameters: URLSearchParams = useMemo(
  //   () => new URLSearchParams(queryParameters.toString()),
  //   [queryParameters],
  // );

  const setPaginationParams = useCallback(
    (page: number = 1, pageSize?: number) => {
      queryParameters.set('page', page.toString());
      if (pageSize) {
        queryParameters.set('size', pageSize.toString());
      }
      setSearchParams(queryParameters);
    },
    [setSearchParams, queryParameters],
  );

  return {
    setPaginationParams,
    page: Number(queryParameters.get('page')) || 1,
    size: Number(queryParameters.get('size')) || 50,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/route-hook.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 92 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (8)

- `useSegmentedPathName`: Exported entity
- `useSecondPathName`: Exported entity
- `useThirdPathName`: Exported entity
- `useGetKnowledgeSearchParams`: Exported entity
- `useNavigateWithFromState`: Exported entity
- `useNavigateToDataset`: Exported entity
- `useGetPaginationParams`: Exported entity
- `useSetPaginationParams`: Exported entity

### Functions (9)

- `useSegmentedPathName()`: Function definition
- `useSecondPathName()`: Function definition
- `useThirdPathName()`: Function definition
- `useGetKnowledgeSearchParams()`: Function definition
- `useNavigateWithFromState()`: Function definition
- `navigate()`: Function definition
- `useNavigateToDataset()`: Function definition
- `useGetPaginationParams()`: Function definition
- `useSetPaginationParams()`: Function definition

### Imports (3)

- `import {`
- `import { useCallback } from 'react';`
- `import { useLocation, useNavigate, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 92
- Blank lines: 16 (17.4%)
- Comment lines: ~4 (4.3%)
- Code lines: ~72


## Dependencies and Imports

- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/hooks/` directory
- Potential test file: `test_route-hook.ts`

## Keywords

Dataset, DocumentId, KnowledgeId, KnowledgeRouteKey, KnowledgeSearchParams, Number, Second, SegmentIndex, Third, Type, TypeScript, URLSearchParams, navigate, newQueryParameters, pathArray, react, setPaginationParams, umi, useGetKnowledgeSearchParams, useGetPaginationParams, useNavigateToDataset, useNavigateWithFromState, useSecondPathName, useSegmentedPathName, useSetPaginationParams, useThirdPathName

---
*Generated by RAGFlow Repository Documentation Generator*
