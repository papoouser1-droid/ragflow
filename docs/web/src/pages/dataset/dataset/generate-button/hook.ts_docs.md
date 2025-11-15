# Documentation: web/src/pages/dataset/dataset/generate-button/hook.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/generate-button/hook.ts`
- **Size**: 4961 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset/generate-button/hook.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset/generate-button/hook.ts` is located in the `web/src/pages/dataset/dataset/generate-button` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to generate-button.

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

- [generate.tsx](generate.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
