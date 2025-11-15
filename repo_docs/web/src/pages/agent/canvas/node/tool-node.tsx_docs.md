# File Documentation: web/src/pages/agent/canvas/node/tool-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/tool-node.tsx`
- **Extension**: `.tsx`
- **Lines**: 94
- **Characters**: 2,827
- **Size**: 2,827 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { NodeCollapsible } from '@/components/collapse';
import { IAgentForm, IToolNode } from '@/interfaces/database/agent';
import { Handle, NodeProps, Position } from '@xyflow/react';
import { get } from 'lodash';
import { MouseEventHandler, memo, useCallback } from 'react';
import { NodeHandleId, Operator } from '../../constant';
import { ToolCard } from '../../form/agent-form/agent-tools';
import { useFindMcpById } from '../../hooks/use-find-mcp-by-id';
import OperatorIcon from '../../operator-icon';
import useGraphStore from '../../store';
import { NodeWrapper } from './node-wrapper';

function InnerToolNode({
  id,
  isConnectable = true,
  selected,
}: NodeProps<IToolNode>) {
  const { edges, getNode } = useGraphStore((state) => state);
  const upstreamAgentNodeId = edges.find((x) => x.target === id)?.source;
  const upstreamAgentNode = getNode(upstreamAgentNodeId);
  const { findMcpById } = useFindMcpById();

  const handleClick = useCallback(
    (operator: string): MouseEventHandler<HTMLLIElement> =>
      (e) => {
        if (operator === Operator.Code) {
          e.preventDefault();
          e.stopPropagation();
        }
      },
    [],
  );

  const tools: IAgentForm['tools'] = get(
    upstreamAgentNode,
    'data.form.tools',
    [],
  );

  const mcpList: IAgentForm['mcp'] = get(
    upstreamAgentNode,
    'data.form.mcp',
    [],
  );

  return (
    <NodeWrapper selected={selected}>
      <Handle
        id={NodeHandleId.End}
        type="target"
        position={Position.Top}
        isConnectable={isConnectable}
        className="!bg-accent-primary !size-2"
      ></Handle>
      <NodeCollapsible items={[tools, mcpList]}>
        {(x) => {
          if ('mcp_id' in x) {
            const mcp = x as unknown as IAgentForm['mcp'][number];
            return (
              <ToolCard
                key={mcp.mcp_id}
                onClick={handleClick(mcp.mcp_id)}
                className="cursor-pointer"
                data-tool={x.mcp_id}
              >
                {findMcpById(mcp.mcp_id)?.name}
              </ToolCard>
            );
          }

          const tool = x as unknown as IAgentForm['tools'][number];
          return (
            <ToolCard
              key={tool.component_name}
              onClick={handleClick(tool.component_name)}
              className="cursor-pointer"
              data-tool={tool.component_name}
            >
              <div className="flex gap-1 items-center pointer-events-none">
                <OperatorIcon
                  name={tool.component_name as Operator}
                ></OperatorIcon>
                {tool.component_name}
              </div>
            </ToolCard>
          );
        }}
      </NodeCollapsible>
    </NodeWrapper>
  );
}

export const ToolNode = memo(InnerToolNode);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/tool-node.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 94 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ToolNode`: Exported entity

### Functions (3)

- `InnerToolNode()`: Function definition
- `upstreamAgentNodeId()`: Function definition
- `handleClick()`: Function definition

### Imports (11)

- `import { NodeCollapsible } from '@/components/collapse';`
- `import { IAgentForm, IToolNode } from '@/interfaces/database/agent';`
- `import { Handle, NodeProps, Position } from '@xyflow/react';`
- `import { get } from 'lodash';`
- `import { MouseEventHandler, memo, useCallback } from 'react';`
- `import { NodeHandleId, Operator } from '../../constant';`
- `import { ToolCard } from '../../form/agent-form/agent-tools';`
- `import { useFindMcpById } from '../../hooks/use-find-mcp-by-id';`
- `import OperatorIcon from '../../operator-icon';`
- `import useGraphStore from '../../store';`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 8 (8.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~86


## Dependencies and Imports

- `@/components/collapse`
- `@/interfaces/database/agent`
- `@xyflow/react`
- `lodash`
- `react`
- `../../constant`
- `../../form/agent-form/agent-tools`
- `../../hooks/use-find-mcp-by-id`
- `../../operator-icon`
- `../../store`
- `./node-wrapper`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node`.

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

- Other files in `web/src/pages/agent/canvas/node/` directory
- Potential test file: `test_tool-node.tsx`

## Keywords

../../constant, ../../form/agent-form/agent-tools, ../../hooks/use-find-mcp-by-id, ../../operator-icon, ../../store, ./node-wrapper, @/components/collapse, @/interfaces/database/agent, @xyflow/react, Code, End, HTMLLIElement, Handle, IAgentForm, IToolNode, InnerToolNode, MouseEventHandler, NodeCollapsible, NodeHandleId, NodeProps, NodeWrapper, Operator, OperatorIcon, Position, ToolCard, ToolNode, Top, TypeScript, handleClick, lodash, mcp, mcpList, react, tool, tools, upstreamAgentNode, upstreamAgentNodeId, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
