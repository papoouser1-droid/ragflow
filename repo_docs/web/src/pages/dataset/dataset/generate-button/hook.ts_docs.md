# File Documentation: web/src/pages/dataset/dataset/generate-button/hook.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/generate-button/hook.ts`
- **Extension**: `.ts`
- **Lines**: 180
- **Characters**: 4,961
- **Size**: 4,961 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import agentService from '@/services/agent-service';
import kbService, { deletePipelineTask } from '@/services/knowledge-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { t } from 'i18next';
import { useEffect, useState } from 'react';
import { useParams } from 'umi';
import { ProcessingType } from '../../dataset-overview/dataset-common';
import { GenerateType, GenerateTypeMap } from './generate';
export const generateStatus = {
  running: 'running',
  completed: 'completed',
  start: 'start',
  failed: 'failed',
};

enum DatasetKey {
  generate = 'generate',
  pauseGenerate = 'pauseGenerate',
}

export interface ITraceInfo {
  begin_at: string;
  chunk_ids: string;
  create_date: string;
  create_time: number;
  digest: string;
  doc_id: string;
  from_page: number;
  id: string;
  priority: number;
  process_duration: number;
  progress: number;
  progress_msg: string;
  retry_count: number;
  task_type: string;
  to_page: number;
  update_date: string;
  update_time: number;
}

export const useTraceGenerate = ({ open }: { open: boolean }) => {
  const { id } = useParams();
  const [isLoopGraphRun, setLoopGraphRun] = useState(false);
  const [isLoopRaptorRun, setLoopRaptorRun] = useState(false);
  const { data: graphRunData, isFetching: graphRunloading } =
    useQuery<ITraceInfo>({
      queryKey: [GenerateType.KnowledgeGraph, id, open],
      // initialData: {},
      gcTime: 0,
      refetchInterval: isLoopGraphRun ? 5000 : false,
      retry: 3,
      retryDelay: 1000,
      enabled: open,
      queryFn: async () => {
        const { data } = await kbService.traceGraphRag({
          kb_id: id,
        });
        return data?.data || {};
      },
    });

  const { data: raptorRunData, isFetching: raptorRunloading } =
    useQuery<ITraceInfo>({
      queryKey: [GenerateType.Raptor, id, open],
      // initialData: {},
      gcTime: 0,
      refetchInterval: isLoopRaptorRun ? 5000 : false,
      retry: 3,
      retryDelay: 1000,
      enabled: open,
      queryFn: async () => {
        const { data } = await kbService.traceRaptor({
          kb_id: id,
        });
        return data?.data || {};
      },
    });

  useEffect(() => {
    setLoopGraphRun(
      !!(
        (graphRunData?.progress || graphRunData?.progress === 0) &&
        graphRunData?.progress < 1 &&
        graphRunData?.progress >= 0
      ),
    );
  }, [graphRunData?.progress]);

  useEffect(() => {
    setLoopRaptorRun(
      !!(
        (raptorRunData?.progress || raptorRunData?.progress === 0) &&
        raptorRunData?.progress < 1 &&
        raptorRunData?.progress >= 0
      ),
    );
  }, [raptorRunData?.progress]);
  return {
    graphRunData,
    graphRunloading,
    raptorRunData,
    raptorRunloading,
  };
};

export const useUnBindTask = () => {
  const { id } = useParams();
  const { mutateAsync: handleUnbindTask } = useMutation({
    mutationKey: [DatasetKey.pauseGenerate],
    mutationFn: async ({ type }: { type: ProcessingType }) => {
      const { data } = await deletePipelineTask({ kb_id: id as string, type });
      if (data.code === 0) {
        message.success(t('message.operated'));
        // queryClient.invalidateQueries({
        //   queryKey: [type],
        // });
      }
      return data;
    },
  });
  return { handleUnbindTask };
};
export const useDatasetGenerate = () => {
  const queryClient = useQueryClient();
  const { id } = useParams();
  const { handleUnbindTask } = useUnBindTask();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [DatasetKey.generate],
    mutationFn: async ({ type }: { type: GenerateType }) => {
      const func =
        type === GenerateType.KnowledgeGraph
          ? kbService.runGraphRag
          : kbService.runRaptor;
      const { data } = await func({
        kb_id: id,
      });
      if (data.code === 0) {
        message.success(t('message.operated'));
        queryClient.invalidateQueries({
          queryKey: [type],
        });
      }
      return data;
    },
  });
  // const pauseGenerate = useCallback(() => {
  //   // TODO: pause generate
  //   console.log('pause generate');
  // }, []);
  const { mutateAsync: pauseGenerate } = useMutation({
    mutationKey: [DatasetKey.pauseGenerate],
    mutationFn: async ({
      task_id,
      type,
    }: {
      task_id: string;
      type: GenerateType;
    }) => {
      const { data } = await agentService.cancelDataflow(task_id);

      const unbindData = await handleUnbindTask({
        type: GenerateTypeMap[type as GenerateType],
      });
      if (data.code === 0 && unbindData.code === 0) {
        // message.success(t('message.operated'));
        queryClient.invalidateQueries({
          queryKey: [type],
        });
      }
      return data;
    },
  });
  return { runGenerate: mutateAsync, pauseGenerate, data, loading };
};

```

## High-Level Overview

      // initialData: {},

## Detailed Walkthrough

### Exports (4)

- `generateStatus`: Exported entity
- `useTraceGenerate`: Exported entity
- `useUnBindTask`: Exported entity
- `useDatasetGenerate`: Exported entity

### Functions (4)

- `useTraceGenerate()`: Function definition
- `useUnBindTask()`: Function definition
- `useDatasetGenerate()`: Function definition
- `pauseGenerate()`: Function definition

### Imports (9)

- `import message from '@/components/ui/message';`
- `import agentService from '@/services/agent-service';`
- `import kbService, { deletePipelineTask } from '@/services/knowledge-service';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { t } from 'i18next';`
- `import { useEffect, useState } from 'react';`
- `import { useParams } from 'umi';`
- `import { ProcessingType } from '../../dataset-overview/dataset-common';`
- `import { GenerateType, GenerateTypeMap } from './generate';`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 9 (5.0%)
- Comment lines: ~10 (5.6%)
- Code lines: ~161


## Dependencies and Imports

- `@/components/ui/message`
- `@/services/agent-service`
- `@/services/knowledge-service`
- `@tanstack/react-query`
- `i18next`
- `react`
- `umi`
- `../../dataset-overview/dataset-common`
- `./generate`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset/generate-button`.

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

- Other files in `web/src/pages/dataset/dataset/generate-button/` directory
- Potential test file: `test_hook.ts`

## Keywords

../../dataset-overview/dataset-common, ./generate, @/components/ui/message, @/services/agent-service, @/services/knowledge-service, @tanstack/react-query, DatasetKey, GenerateType, GenerateTypeMap, ITraceInfo, KnowledgeGraph, ProcessingType, Raptor, TODO, TypeScript, as, func, generateStatus, i18next, pauseGenerate, queryClient, react, tanstack, umi, unbindData, useDatasetGenerate, useTraceGenerate, useUnBindTask

---
*Generated by RAGFlow Repository Documentation Generator*
