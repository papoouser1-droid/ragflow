# File Documentation: web/src/hooks/use-dataflow-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-dataflow-request.ts`
- **Extension**: `.ts`
- **Lines**: 92
- **Characters**: 2,380
- **Size**: 2,380 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { IFlow } from '@/interfaces/database/agent';
import dataflowService from '@/services/dataflow-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useTranslation } from 'react-i18next';
import { useParams } from 'umi';

export const enum DataflowApiAction {
  ListDataflow = 'listDataflow',
  RemoveDataflow = 'removeDataflow',
  FetchDataflow = 'fetchDataflow',
  RunDataflow = 'runDataflow',
  SetDataflow = 'setDataflow',
}

export const useRemoveDataflow = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [DataflowApiAction.RemoveDataflow],
    mutationFn: async (ids: string[]) => {
      const { data } = await dataflowService.removeDataflow({
        canvas_ids: ids,
      });
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [DataflowApiAction.ListDataflow],
        });

        message.success(t('message.deleted'));
      }
      return data.code;
    },
  });

  return { data, loading, removeDataflow: mutateAsync };
};

export const useSetDataflow = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [DataflowApiAction.SetDataflow],
    mutationFn: async (params: Partial<IFlow>) => {
      const { data } = await dataflowService.setDataflow(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [DataflowApiAction.FetchDataflow],
        });

        message.success(t(`message.${params.id ? 'modified' : 'created'}`));
      }
      return data?.code;
    },
  });

  return { data, loading, setDataflow: mutateAsync };
};

export const useFetchDataflow = () => {
  const { id } = useParams();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IFlow>({
    queryKey: [DataflowApiAction.FetchDataflow, id],
    gcTime: 0,
    initialData: {} as IFlow,
    enabled: !!id,
    refetchOnWindowFocus: false,
    queryFn: async () => {
      const { data } = await dataflowService.fetchDataflow(id);

      return data?.data ?? ({} as IFlow);
    },
  });

  return { data, loading, refetch };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-dataflow-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 92 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `enum`: Exported entity
- `useRemoveDataflow`: Exported entity
- `useSetDataflow`: Exported entity
- `useFetchDataflow`: Exported entity

### Functions (3)

- `useRemoveDataflow()`: Function definition
- `useSetDataflow()`: Function definition
- `useFetchDataflow()`: Function definition

### Imports (6)

- `import message from '@/components/ui/message';`
- `import { IFlow } from '@/interfaces/database/agent';`
- `import dataflowService from '@/services/dataflow-service';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { useTranslation } from 'react-i18next';`
- `import { useParams } from 'umi';`

## Code Structure Analysis

- Total lines: 92
- Blank lines: 14 (15.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~78


## Dependencies and Imports

- `@/components/ui/message`
- `@/interfaces/database/agent`
- `@/services/dataflow-service`
- `@tanstack/react-query`
- `react-i18next`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

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

- Other files in `web/src/hooks/` directory
- Potential test file: `test_use-dataflow-request.ts`

## Keywords

@/components/ui/message, @/interfaces/database/agent, @/services/dataflow-service, @tanstack/react-query, DataflowApiAction, FetchDataflow, IFlow, ListDataflow, Partial, RemoveDataflow, RunDataflow, SetDataflow, TypeScript, enum, queryClient, react-i18next, tanstack, umi, useFetchDataflow, useRemoveDataflow, useSetDataflow

---
*Generated by RAGFlow Repository Documentation Generator*
