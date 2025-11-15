# File Documentation: web/src/pages/agent/form/agent-form/tool-popover/use-update-mcp.ts

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/tool-popover/use-update-mcp.ts`
- **Extension**: `.ts`
- **Lines**: 75
- **Characters**: 2,092
- **Size**: 2,092 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useListMcpServer } from '@/hooks/use-mcp-request';
import { IAgentForm } from '@/interfaces/database/agent';
import { AgentFormContext } from '@/pages/agent/context';
import useGraphStore from '@/pages/agent/store';
import { get } from 'lodash';
import { useCallback, useContext, useMemo } from 'react';

export function useGetNodeMCP() {
  const node = useContext(AgentFormContext);

  return useMemo(() => {
    const mcp: IAgentForm['mcp'] = get(node, 'data.form.mcp');
    return mcp;
  }, [node]);
}

export function useUpdateAgentNodeMCP() {
  const { updateNodeForm } = useGraphStore((state) => state);
  const node = useContext(AgentFormContext);
  const mcpList = useGetNodeMCP();
  const { data } = useListMcpServer();
  const mcpServers = data.mcp_servers;

  const findMcpTools = useCallback(
    (mcpId: string) => {
      const mcp = mcpServers.find((x) => x.id === mcpId);
      return mcp?.variables.tools;
    },
    [mcpServers],
  );

  const updateNodeMCP = useCallback(
    (value: string[]) => {
      if (node?.id) {
        const nextValue = value.reduce<IAgentForm['mcp']>((pre, cur) => {
          const mcp = mcpList.find((x) => x.mcp_id === cur);
          const tools = findMcpTools(cur);
          if (mcp) {
            pre.push(mcp);
          } else if (tools) {
            pre.push({
              mcp_id: cur,
              tools: {},
            });
          }
          return pre;
        }, []);

        updateNodeForm(node?.id, nextValue, ['mcp']);
      }
    },
    [node?.id, updateNodeForm, mcpList, findMcpTools],
  );

  return { updateNodeMCP };
}

export function useDeleteAgentNodeMCP() {
  const { updateNodeForm } = useGraphStore((state) => state);
  const mcpList = useGetNodeMCP();
  const node = useContext(AgentFormContext);

  const deleteNodeMCP = useCallback(
    (value: string) => () => {
      const nextMCP = mcpList.filter((x) => x.mcp_id !== value);
      if (node?.id) {
        updateNodeForm(node?.id, nextMCP, ['mcp']);
      }
    },
    [node?.id, mcpList, updateNodeForm],
  );

  return { deleteNodeMCP };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/tool-popover/use-update-mcp.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 75 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useGetNodeMCP`: Exported entity
- `useUpdateAgentNodeMCP`: Exported entity
- `useDeleteAgentNodeMCP`: Exported entity

### Functions (11)

- `useGetNodeMCP()`: Function definition
- `node()`: Function definition
- `useUpdateAgentNodeMCP()`: Function definition
- `findMcpTools()`: Function definition
- `mcp()`: Function definition
- `updateNodeMCP()`: Function definition
- `nextValue()`: Function definition
- `mcp()`: Function definition
- `useDeleteAgentNodeMCP()`: Function definition
- `deleteNodeMCP()`: Function definition
- `nextMCP()`: Function definition

### Imports (6)

- `import { useListMcpServer } from '@/hooks/use-mcp-request';`
- `import { IAgentForm } from '@/interfaces/database/agent';`
- `import { AgentFormContext } from '@/pages/agent/context';`
- `import useGraphStore from '@/pages/agent/store';`
- `import { get } from 'lodash';`
- `import { useCallback, useContext, useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 75
- Blank lines: 11 (14.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~64


## Dependencies and Imports

- `@/hooks/use-mcp-request`
- `@/interfaces/database/agent`
- `@/pages/agent/context`
- `@/pages/agent/store`
- `lodash`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form/tool-popover`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/agent-form/tool-popover/` directory
- Potential test file: `test_use-update-mcp.ts`

## Keywords

@/hooks/use-mcp-request, @/interfaces/database/agent, @/pages/agent/context, @/pages/agent/store, AgentFormContext, IAgentForm, TypeScript, deleteNodeMCP, findMcpTools, lodash, mcp, mcpList, mcpServers, nextMCP, nextValue, node, react, tools, updateNodeMCP, useDeleteAgentNodeMCP, useGetNodeMCP, useUpdateAgentNodeMCP

---
*Generated by RAGFlow Repository Documentation Generator*
