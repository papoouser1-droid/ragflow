# File Documentation: web/src/pages/agent/form/agent-form/tool-popover/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/tool-popover/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 88
- **Characters**: 2,695
- **Size**: 2,695 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';
import { Operator } from '@/pages/agent/constant';
import { AgentFormContext, AgentInstanceContext } from '@/pages/agent/context';
import useGraphStore from '@/pages/agent/store';
import { Position } from '@xyflow/react';
import { t } from 'i18next';
import { PropsWithChildren, useCallback, useContext, useEffect } from 'react';
import { useGetAgentMCPIds, useGetAgentToolNames } from '../use-get-tools';
import { MCPCommand, ToolCommand } from './tool-command';
import { useUpdateAgentNodeMCP } from './use-update-mcp';
import { useUpdateAgentNodeTools } from './use-update-tools';

enum ToolType {
  Common = 'common',
  MCP = 'mcp',
}

export function ToolPopover({ children }: PropsWithChildren) {
  const { addCanvasNode } = useContext(AgentInstanceContext);
  const node = useContext(AgentFormContext);
  const { updateNodeTools } = useUpdateAgentNodeTools();
  const { toolNames } = useGetAgentToolNames();
  const deleteAgentToolNodeById = useGraphStore(
    (state) => state.deleteAgentToolNodeById,
  );
  const { mcpIds } = useGetAgentMCPIds();
  const { updateNodeMCP } = useUpdateAgentNodeMCP();

  const handleChange = useCallback(
    (value: string[]) => {
      if (Array.isArray(value) && node?.id) {
        updateNodeTools(value);
      }
    },
    [node?.id, updateNodeTools],
  );

  useEffect(() => {
    const total = toolNames.length + mcpIds.length;
    if (node?.id) {
      if (total > 0) {
        addCanvasNode(Operator.Tool, {
          position: Position.Bottom,
          nodeId: node?.id,
        })();
      } else {
        deleteAgentToolNodeById(node.id);
      }
    }
  }, [
    addCanvasNode,
    deleteAgentToolNodeById,
    mcpIds.length,
    node?.id,
    toolNames.length,
  ]);

  return (
    <Popover>
      <PopoverTrigger asChild>{children}</PopoverTrigger>
      <PopoverContent className="w-80 p-4">
        <Tabs defaultValue={ToolType.Common}>
          <TabsList>
            <TabsTrigger value={ToolType.Common}>
              {t('flow.builtIn')}
            </TabsTrigger>
            <TabsTrigger value={ToolType.MCP}>MCP</TabsTrigger>
          </TabsList>
          <TabsContent value={ToolType.Common}>
            <ToolCommand
              onChange={handleChange}
              value={toolNames}
            ></ToolCommand>
          </TabsContent>
          <TabsContent value={ToolType.MCP}>
            <MCPCommand value={mcpIds} onChange={updateNodeMCP}></MCPCommand>
          </TabsContent>
        </Tabs>
      </PopoverContent>
    </Popover>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/tool-popover/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 88 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ToolPopover`: Exported entity

### Functions (3)

- `ToolPopover()`: Function definition
- `deleteAgentToolNodeById()`: Function definition
- `handleChange()`: Function definition

### Imports (12)

- `import {`
- `import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';`
- `import { Operator } from '@/pages/agent/constant';`
- `import { AgentFormContext, AgentInstanceContext } from '@/pages/agent/context';`
- `import useGraphStore from '@/pages/agent/store';`
- `import { Position } from '@xyflow/react';`
- `import { t } from 'i18next';`
- `import { PropsWithChildren, useCallback, useContext, useEffect } from 'react';`
- `import { useGetAgentMCPIds, useGetAgentToolNames } from '../use-get-tools';`
- `import { MCPCommand, ToolCommand } from './tool-command';`

## Code Structure Analysis

- Total lines: 88
- Blank lines: 6 (6.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~82


## Dependencies and Imports

- `@/components/ui/tabs`
- `@/pages/agent/constant`
- `@/pages/agent/context`
- `@/pages/agent/store`
- `@xyflow/react`
- `i18next`
- `react`
- `../use-get-tools`
- `./tool-command`
- `./use-update-mcp`
- `./use-update-tools`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form/tool-popover`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/agent-form/tool-popover/` directory
- Potential test file: `test_index.tsx`

## Keywords

../use-get-tools, ./tool-command, ./use-update-mcp, ./use-update-tools, @/components/ui/tabs, @/pages/agent/constant, @/pages/agent/context, @/pages/agent/store, @xyflow/react, AgentFormContext, AgentInstanceContext, Array, Bottom, Common, MCP, MCPCommand, Operator, Popover, PopoverContent, PopoverTrigger, Position, PropsWithChildren, Tabs, TabsContent, TabsList, TabsTrigger, Tool, ToolCommand, ToolPopover, ToolType, TypeScript, deleteAgentToolNodeById, handleChange, i18next, node, react, total, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
