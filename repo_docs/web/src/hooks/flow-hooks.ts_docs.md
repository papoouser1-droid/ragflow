# File Documentation: web/src/hooks/flow-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/flow-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 317
- **Characters**: 7,654
- **Size**: 7,657 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/flow-hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 317 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (13)

- `useFetchFlowList`: Exported entity
- `useFetchListVersion`: Exported entity
- `useFetchVersion`: Exported entity
- `useFetchFlow`: Exported entity
- `useSettingFlow`: Exported entity
- `useFetchFlowSSE`: Exported entity
- `useSetFlow`: Exported entity
- `useDeleteFlow`: Exported entity
- `useRunFlow`: Exported entity
- `useResetFlow`: Exported entity
- `useTestDbConnect`: Exported entity
- `useFetchInputElements`: Exported entity
- `useDebugSingle`: Exported entity

### Functions (13)

- `useFetchFlowList()`: Function definition
- `useFetchListVersion()`: Function definition
- `useFetchVersion()`: Function definition
- `useFetchFlow()`: Function definition
- `useSettingFlow()`: Function definition
- `useFetchFlowSSE()`: Function definition
- `useSetFlow()`: Function definition
- `useDeleteFlow()`: Function definition
- `useRunFlow()`: Function definition
- `useResetFlow()`: Function definition
- `useTestDbConnect()`: Function definition
- `useFetchInputElements()`: Function definition
- `useDebugSingle()`: Function definition

### Imports (11)

- `import { DSL, IFlow } from '@/interfaces/database/flow';`
- `import { IDebugSingleRequestBody } from '@/interfaces/request/flow';`
- `import i18n from '@/locales/config';`
- `import { useGetSharedChatSearchParams } from '@/pages/chat/shared-hooks';`
- `import flowService from '@/services/flow-service';`
- `import { buildMessageListWithUuid } from '@/utils/chat';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { message } from 'antd';`
- `import { set } from 'lodash';`
- `import get from 'lodash/get';`

## Code Structure Analysis

- Total lines: 317
- Blank lines: 38 (12.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~279


## Dependencies and Imports

- `@/interfaces/database/flow`
- `@/interfaces/request/flow`
- `@/locales/config`
- `@/pages/chat/shared-hooks`
- `@/services/flow-service`
- `@/utils/chat`
- `@tanstack/react-query`
- `antd`
- `lodash`
- `lodash/get`
- `umi`

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
- Potential test file: `test_flow-hooks.ts`

## Keywords

@/interfaces/database/flow, @/interfaces/request/flow, @/locales/config, @/pages/chat/shared-hooks, @/services/flow-service, @/utils/chat, @tanstack/react-query, API, DSL, IDebugSingleRequestBody, IFlow, Only, SettingFlow, TypeScript, antd, lodash, lodash/get, messageList, queryClient, ret, tanstack, umi, useDebugSingle, useDeleteFlow, useFetchFlow, useFetchFlowList, useFetchFlowSSE, useFetchInputElements, useFetchListVersion, useFetchVersion, useResetFlow, useRunFlow, useSetFlow, useSettingFlow, useTestDbConnect

---
*Generated by RAGFlow Repository Documentation Generator*
