# File Documentation: web/src/pages/agent/form/agent-form/tool-popover/use-update-tools.ts

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/tool-popover/use-update-tools.ts`
- **Extension**: `.ts`
- **Lines**: 67
- **Characters**: 2,058
- **Size**: 2,058 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { IAgentForm } from '@/interfaces/database/agent';
import { Operator } from '@/pages/agent/constant';
import { AgentFormContext } from '@/pages/agent/context';
import { useAgentToolInitialValues } from '@/pages/agent/hooks/use-agent-tool-initial-values';
import useGraphStore from '@/pages/agent/store';
import { get } from 'lodash';
import { useCallback, useContext, useMemo } from 'react';

export function useGetNodeTools() {
  const node = useContext(AgentFormContext);

  return useMemo(() => {
    const tools: IAgentForm['tools'] = get(node, 'data.form.tools');
    return tools;
  }, [node]);
}

export function useUpdateAgentNodeTools() {
  const { updateNodeForm } = useGraphStore((state) => state);
  const node = useContext(AgentFormContext);
  const tools = useGetNodeTools();
  const { initializeAgentToolValues } = useAgentToolInitialValues();

  const updateNodeTools = useCallback(
    (value: string[]) => {
      if (node?.id) {
        const nextValue = value.reduce<IAgentForm['tools']>((pre, cur) => {
          const tool = tools.find((x) => x.component_name === cur);
          pre.push(
            tool
              ? tool
              : {
                  component_name: cur,
                  name: cur,
                  params: initializeAgentToolValues(cur as Operator),
                },
          );
          return pre;
        }, []);

        updateNodeForm(node?.id, nextValue, ['tools']);
      }
    },
    [initializeAgentToolValues, node?.id, tools, updateNodeForm],
  );

  return { updateNodeTools };
}

export function useDeleteAgentNodeTools() {
  const { updateNodeForm } = useGraphStore((state) => state);
  const tools = useGetNodeTools();
  const node = useContext(AgentFormContext);

  const deleteNodeTool = useCallback(
    (value: string) => () => {
      const nextTools = tools.filter((x) => x.component_name !== value);
      if (node?.id) {
        updateNodeForm(node?.id, nextTools, ['tools']);
      }
    },
    [node?.id, tools, updateNodeForm],
  );

  return { deleteNodeTool };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/tool-popover/use-update-tools.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 67 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useGetNodeTools`: Exported entity
- `useUpdateAgentNodeTools`: Exported entity
- `useDeleteAgentNodeTools`: Exported entity

### Functions (9)

- `useGetNodeTools()`: Function definition
- `node()`: Function definition
- `useUpdateAgentNodeTools()`: Function definition
- `updateNodeTools()`: Function definition
- `nextValue()`: Function definition
- `tool()`: Function definition
- `useDeleteAgentNodeTools()`: Function definition
- `deleteNodeTool()`: Function definition
- `nextTools()`: Function definition

### Imports (7)

- `import { IAgentForm } from '@/interfaces/database/agent';`
- `import { Operator } from '@/pages/agent/constant';`
- `import { AgentFormContext } from '@/pages/agent/context';`
- `import { useAgentToolInitialValues } from '@/pages/agent/hooks/use-agent-tool-initial-values';`
- `import useGraphStore from '@/pages/agent/store';`
- `import { get } from 'lodash';`
- `import { useCallback, useContext, useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 67
- Blank lines: 10 (14.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~57


## Dependencies and Imports

- `@/interfaces/database/agent`
- `@/pages/agent/constant`
- `@/pages/agent/context`
- `@/pages/agent/hooks/use-agent-tool-initial-values`
- `@/pages/agent/store`
- `lodash`
- `react`

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
- Potential test file: `test_use-update-tools.ts`

## Keywords

@/interfaces/database/agent, @/pages/agent/constant, @/pages/agent/context, @/pages/agent/hooks/use-agent-tool-initial-values, @/pages/agent/store, AgentFormContext, IAgentForm, Operator, TypeScript, deleteNodeTool, lodash, nextTools, nextValue, node, react, tool, tools, updateNodeTools, useDeleteAgentNodeTools, useGetNodeTools, useUpdateAgentNodeTools

---
*Generated by RAGFlow Repository Documentation Generator*
