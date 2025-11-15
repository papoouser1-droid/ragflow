# Documentation: web/src/hooks/flow-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/flow-hooks.ts`
- **Size**: 7657 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/flow-hooks.ts`.

## Original Source Code

```ts
import { DSL, IFlow } from '@/interfaces/database/flow';
import { IDebugSingleRequestBody } from '@/interfaces/request/flow';
import i18n from '@/locales/config';
import { useGetSharedChatSearchParams } from '@/pages/chat/shared-hooks';
import flowService from '@/services/flow-service';
import { buildMessageListWithUuid } from '@/utils/chat';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { message } from 'antd';
import { set } from 'lodash';
import get from 'lodash/get';
import { useParams } from 'umi';

export const useFetchFlowList = (): { data: IFlow[]; loading: boolean } => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['fetchFlowList'],
    initialData: [],
    gcTime: 0,
    queryFn: async () => {
      const { data } = await flowService.listCanvas();

      return data?.data ?? [];
    },
  });

  return { data, loading };
};

export const useFetchListVersion = (
  canvas_id: string,
): {
  data: {
    created_at: string;
    title: string;
    id: string;
  }[];
  loading: boolean;
} => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['fetchListVersion'],
    initialData: [],
    gcTime: 0,
    queryFn: async () => {
      const { data } = await flowService.getListVersion({}, canvas_id);

      return data?.data ?? [];
    },
  });

  return { data, loading };
};

export const useFetchVersion = (
  version_id?: string,
): {
  data?: IFlow;
  loading: boolean;
} => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['fetchVersion', version_id],
    initialData: undefined,
    gcTime: 0,
    enabled: !!version_id, // Only call API when both values are provided
    queryFn: async () => {
      if (!version_id) return undefined;

      const { data } = await flowService.getVersion({}, version_id);

      return data?.data ?? undefined;
    },
  });

  return { data, loading };
};

export const useFetchFlow = (): {
  data: IFlow;
  loading: boolean;
  refetch: () => void;
} => {
  const { id } = useParams();
  const { sharedId } = useGetSharedChatSearchParams();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery({
    queryKey: ['flowDetail'],
    initialData: {} as IFlow,
    refetchOnReconnect: false,
    refetchOnMount: false,
    refetchOnWindowFocus: false,
    gcTime: 0,
    queryFn: async () => {
      const { data } = await flowService.getCanvas({}, sharedId || id);

      const messageList = buildMessageListWithUuid(
        get(data, 'data.dsl.messages', []),
      );
      set(data, 'data.dsl.messages', messageList);

      return data?.data ?? {};
    },
  });

  return { data, loading, refetch };
};

export const useSettingFlow = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['SettingFlow'],
    mutationFn: async (params: any) => {
      const ret = await flowService.settingCanvas(params);
      if (ret?.data?.code === 0) {
        message.success('success');
      } else {
        message.error(ret?.data?.data);
      }
      return ret;
    },
  });

  return { data, loading, settingFlow: mutateAsync };
};

export const useFetchFlowSSE = (): {
  data: IFlow;
  loading: boolean;
  refetch: () => void;
} => {
  const { sharedId } = useGetSharedChatSearchParams();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery({
    queryKey: ['flowDetailSSE'],
    initialData: {} as IFlow,
    refetchOnReconnect: false,
    refetchOnMount: false,
    refetchOnWindowFocus: false,
    gcTime: 0,
    queryFn: async () => {
      if (!sharedId) return {};
      const { data } = await flowService.getCanvasSSE({}, sharedId);

      const messageList = buildMessageListWithUuid(
        get(data, 'data.dsl.messages', []),
      );
      set(data, 'data.dsl.messages', messageList);

      return data?.data ?? {};
    },
  });

  return { data, loading, refetch };
};

export const useSetFlow = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['setFlow'],
    mutationFn: async (params: {
      id?: string;
      title?: string;
      dsl?: DSL;
      avatar?: string;
    }) => {
      const { data = {} } = await flowService.setCanvas(params);
      if (data.code === 0) {
        message.success(
          i18n.t(`message.${params?.id ? 'modified' : 'created'}`),
        );
        queryClient.invalidateQueries({ queryKey: ['fetchFlowList'] });
      }
      return data;
    },
  });

  return { data, loading, setFlow: mutateAsync };
};

export const useDeleteFlow = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteFlow'],
    mutationFn: async (canvasIds: string[]) => {
      const { data } = await flowService.removeCanvas({ canvasIds });
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: ['infiniteFetchFlowListTeam'],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, deleteFlow: mutateAsync };
};

export const useRunFlow = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['runFlow'],
    mutationFn: async (params: { id: string; dsl: DSL }) => {
      const { data } = await flowService.runCanvas(params);
      if (data.code === 0) {
        message.success(i18n.t(`message.modified`));
      }
      return data?.data ?? {};
    },
  });

  return { data, loading, runFlow: mutateAsync };
};

export const useResetFlow = () => {
  const { id } = useParams();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['resetFlow'],
    mutationFn: async () => {
      const { data } = await flowService.resetCanvas({ id });
      return data;
    },
  });

  return { data, loading, resetFlow: mutateAsync };
};

export const useTestDbConnect = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['testDbConnect'],
    mutationFn: async (params: any) => {
      const ret = await flowService.testDbConnect(params);
      if (ret?.data?.code === 0) {
        message.success(ret?.data?.data);
      } else {
        message.error(ret?.data?.data);
      }
      return ret;
    },
  });

  return { data, loading, testDbConnect: mutateAsync };
};

export const useFetchInputElements = (componentId?: string) => {
  const { id } = useParams();

  const { data, isPending: loading } = useQuery({
    queryKey: ['fetchInputElements', id, componentId],
    initialData: [],
    enabled: !!id && !!componentId,
    retryOnMount: false,
    refetchOnWindowFocus: false,
    refetchOnReconnect: false,
    gcTime: 0,
    queryFn: async () => {
      try {
        const { data } = await flowService.getInputElements({
          id,
          component_id: componentId,
        });
        return data?.data ?? [];
      } catch (error) {
        console.log('🚀 ~ queryFn: ~ error:', error);
      }
    },
  });

  return { data, loading };
};

export const useDebugSingle = () => {
  const { id } = useParams();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['debugSingle'],
    mutationFn: async (params: IDebugSingleRequestBody) => {
      const ret = await flowService.debugSingle({ id, ...params });
      if (ret?.data?.code !== 0) {
        message.error(ret?.data?.message);
      }
      return ret?.data?.data;
    },
  });

  return { data, loading, debugSingle: mutateAsync };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/flow-hooks.ts` is located in the `web/src/hooks` directory.

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
