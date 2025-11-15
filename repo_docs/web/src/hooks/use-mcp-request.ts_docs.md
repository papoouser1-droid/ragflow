# File Documentation: web/src/hooks/use-mcp-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-mcp-request.ts`
- **Extension**: `.ts`
- **Lines**: 271
- **Characters**: 7,213
- **Size**: 7,213 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { ResponseType } from '@/interfaces/database/base';
import {
  IExportedMcpServers,
  IMcpServer,
  IMcpServerListResponse,
  IMCPTool,
  IMCPToolRecord,
} from '@/interfaces/database/mcp';
import {
  IImportMcpServersRequestBody,
  ITestMcpRequestBody,
} from '@/interfaces/request/mcp';
import i18n from '@/locales/config';
import mcpServerService, {
  listMcpServers,
} from '@/services/mcp-server-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { useState } from 'react';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';

export const enum McpApiAction {
  ListMcpServer = 'listMcpServer',
  GetMcpServer = 'getMcpServer',
  CreateMcpServer = 'createMcpServer',
  UpdateMcpServer = 'updateMcpServer',
  DeleteMcpServer = 'deleteMcpServer',
  ImportMcpServer = 'importMcpServer',
  ExportMcpServer = 'exportMcpServer',
  ListMcpServerTools = 'listMcpServerTools',
  TestMcpServerTool = 'testMcpServerTool',
  CacheMcpServerTool = 'cacheMcpServerTool',
  TestMcpServer = 'testMcpServer',
}

export const useListMcpServer = () => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });

  const { data, isFetching: loading } = useQuery<IMcpServerListResponse>({
    queryKey: [
      McpApiAction.ListMcpServer,
      {
        debouncedSearchString,
        ...pagination,
      },
    ],
    initialData: { total: 0, mcp_servers: [] },
    gcTime: 0,
    queryFn: async () => {
      const { data } = await listMcpServers({
        keywords: debouncedSearchString,
        page_size: pagination.pageSize,
        page: pagination.current,
      });
      return data?.data;
    },
  });

  return {
    data,
    loading,
    handleInputChange,
    setPagination,
    searchString,
    pagination: { ...pagination, total: data?.total },
  };
};

export const useGetMcpServer = (id: string) => {
  const { data, isFetching: loading } = useQuery<IMcpServer>({
    queryKey: [McpApiAction.GetMcpServer, id],
    initialData: {} as IMcpServer,
    gcTime: 0,
    enabled: !!id,
    queryFn: async () => {
      const { data } = await mcpServerService.get({ mcp_id: id });
      return data?.data ?? {};
    },
  });

  return { data, loading, id };
};

export const useCreateMcpServer = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.CreateMcpServer],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await mcpServerService.create(params);
      if (data.code === 0) {
        message.success(i18n.t(`message.created`));

        queryClient.invalidateQueries({
          queryKey: [McpApiAction.ListMcpServer],
        });
      }
      return data.code;
    },
  });

  return { data, loading, createMcpServer: mutateAsync };
};

export const useUpdateMcpServer = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.UpdateMcpServer],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await mcpServerService.update(params);
      if (data.code === 0) {
        message.success(i18n.t(`message.updated`));

        queryClient.invalidateQueries({
          queryKey: [McpApiAction.ListMcpServer],
        });
      }
      return data.code;
    },
  });

  return { data, loading, updateMcpServer: mutateAsync };
};

export const useDeleteMcpServer = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.DeleteMcpServer],
    mutationFn: async (ids: string[]) => {
      const { data = {} } = await mcpServerService.delete({ mcp_ids: ids });
      if (data.code === 0) {
        message.success(i18n.t(`message.deleted`));

        queryClient.invalidateQueries({
          queryKey: [McpApiAction.ListMcpServer],
        });
      }
      return data;
    },
  });

  return { data, loading, deleteMcpServer: mutateAsync };
};

export const useImportMcpServer = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.ImportMcpServer],
    mutationFn: async (params: IImportMcpServersRequestBody) => {
      const { data = {} } = await mcpServerService.import(params);
      if (data.code === 0) {
        message.success(i18n.t(`message.operated`));

        queryClient.invalidateQueries({
          queryKey: [McpApiAction.ListMcpServer],
        });
      }
      return data;
    },
  });

  return { data, loading, importMcpServer: mutateAsync };
};

export const useExportMcpServer = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation<ResponseType<IExportedMcpServers>, Error, string[]>({
    mutationKey: [McpApiAction.ExportMcpServer],
    mutationFn: async (ids) => {
      const { data = {} } = await mcpServerService.export({ mcp_ids: ids });
      if (data.code === 0) {
        message.success(i18n.t(`message.operated`));
      }
      return data;
    },
  });

  return { data, loading, exportMcpServer: mutateAsync };
};

export const useListMcpServerTools = () => {
  const [ids, setIds] = useState<string[]>([]);
  const { data, isFetching: loading } = useQuery<IMCPToolRecord>({
    queryKey: [McpApiAction.ListMcpServerTools],
    initialData: {} as IMCPToolRecord,
    gcTime: 0,
    enabled: ids.length > 0,
    queryFn: async () => {
      const { data } = await mcpServerService.listTools({ mcp_ids: ids });
      return data?.data ?? {};
    },
  });

  return { data, loading, setIds };
};

export const useTestMcpServer = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation<ResponseType<IMCPTool[]>, Error, ITestMcpRequestBody>({
    mutationKey: [McpApiAction.TestMcpServer],
    mutationFn: async (params) => {
      const { data } = await mcpServerService.test(params);

      return data;
    },
  });

  return { data, loading, testMcpServer: mutateAsync };
};

export const useCacheMcpServerTool = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.CacheMcpServerTool],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await mcpServerService.cacheTool(params);

      return data;
    },
  });

  return { data, loading, cacheMcpServerTool: mutateAsync };
};

export const useTestMcpServerTool = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [McpApiAction.TestMcpServerTool],
    mutationFn: async (params: Record<string, any>) => {
      const { data = {} } = await mcpServerService.testTool(params);

      return data;
    },
  });

  return { data, loading, testMcpServerTool: mutateAsync };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-mcp-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 271 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (12)

- `enum`: Exported entity
- `useListMcpServer`: Exported entity
- `useGetMcpServer`: Exported entity
- `useCreateMcpServer`: Exported entity
- `useUpdateMcpServer`: Exported entity
- `useDeleteMcpServer`: Exported entity
- `useImportMcpServer`: Exported entity
- `useExportMcpServer`: Exported entity
- `useListMcpServerTools`: Exported entity
- `useTestMcpServer`: Exported entity
- `useCacheMcpServerTool`: Exported entity
- `useTestMcpServerTool`: Exported entity

### Functions (11)

- `useListMcpServer()`: Function definition
- `useGetMcpServer()`: Function definition
- `useCreateMcpServer()`: Function definition
- `useUpdateMcpServer()`: Function definition
- `useDeleteMcpServer()`: Function definition
- `useImportMcpServer()`: Function definition
- `useExportMcpServer()`: Function definition
- `useListMcpServerTools()`: Function definition
- `useTestMcpServer()`: Function definition
- `useCacheMcpServerTool()`: Function definition
- `useTestMcpServerTool()`: Function definition

### Imports (10)

- `import message from '@/components/ui/message';`
- `import { ResponseType } from '@/interfaces/database/base';`
- `import {`
- `import {`
- `import i18n from '@/locales/config';`
- `import mcpServerService, {`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { useDebounce } from 'ahooks';`
- `import { useState } from 'react';`
- `import {`

## Code Structure Analysis

- Total lines: 271
- Blank lines: 32 (11.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~239


## Dependencies and Imports

- `@/components/ui/message`
- `@/interfaces/database/base`
- `@/locales/config`
- `@tanstack/react-query`
- `ahooks`
- `react`

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
- Potential test file: `test_use-mcp-request.ts`

## Keywords

@/components/ui/message, @/interfaces/database/base, @/locales/config, @tanstack/react-query, CacheMcpServerTool, CreateMcpServer, DeleteMcpServer, Error, ExportMcpServer, GetMcpServer, IExportedMcpServers, IImportMcpServersRequestBody, IMCPTool, IMCPToolRecord, IMcpServer, IMcpServerListResponse, ITestMcpRequestBody, ImportMcpServer, ListMcpServer, ListMcpServerTools, McpApiAction, Record, ResponseType, TestMcpServer, TestMcpServerTool, TypeScript, UpdateMcpServer, ahooks, debouncedSearchString, enum, queryClient, react, tanstack, useCacheMcpServerTool, useCreateMcpServer, useDeleteMcpServer, useExportMcpServer, useGetMcpServer, useImportMcpServer, useListMcpServer, useListMcpServerTools, useTestMcpServer, useTestMcpServerTool, useUpdateMcpServer

---
*Generated by RAGFlow Repository Documentation Generator*
