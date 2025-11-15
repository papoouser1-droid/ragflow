# File Documentation: web/src/pages/agent/hooks/use-before-delete.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-before-delete.tsx`
- **Extension**: `.tsx`
- **Lines**: 83
- **Characters**: 2,740
- **Size**: 2,740 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { Node, OnBeforeDelete } from '@xyflow/react';
import { Operator } from '../constant';
import useGraphStore from '../store';
import { deleteAllDownstreamAgentsAndTool } from '../utils/delete-node';

const UndeletableNodes = [Operator.Begin, Operator.IterationStart];

export function useBeforeDelete() {
  const { getOperatorTypeFromId, getNode } = useGraphStore((state) => state);

  const agentPredicate = (node: Node) => {
    return getOperatorTypeFromId(node.id) === Operator.Agent;
  };

  const handleBeforeDelete: OnBeforeDelete<RAGFlowNodeType> = async ({
    nodes, // Nodes to be deleted
    edges, // Edges to be deleted
  }) => {
    const toBeDeletedNodes = nodes.filter((node) => {
      const operatorType = node.data?.label as Operator;
      if (operatorType === Operator.Begin) {
        return false;
      }

      if (
        operatorType === Operator.IterationStart &&
        !nodes.some((x) => x.id === node.parentId)
      ) {
        return false;
      }

      return true;
    });

    const toBeDeletedEdges = edges.filter((edge) => {
      const sourceType = getOperatorTypeFromId(edge.source) as Operator;
      const downStreamNodes = nodes.filter((x) => x.id === edge.target);

      // This edge does not need to be deleted, the range of edges that do not need to be deleted is smaller, so consider the case where it does not need to be deleted
      if (
        UndeletableNodes.includes(sourceType) && // Upstream node is Begin or IterationStart
        downStreamNodes.length === 0 // Downstream node does not exist in the nodes to be deleted
      ) {
        if (!nodes.some((x) => x.id === edge.source)) {
          return true; // Can be deleted
        }
        return false; // Cannot be deleted
      }

      return true;
    });

    // Delete the agent and tool nodes downstream of the agent node
    if (nodes.some(agentPredicate)) {
      nodes.filter(agentPredicate).forEach((node) => {
        const { downstreamAgentAndToolEdges, downstreamAgentAndToolNodeIds } =
          deleteAllDownstreamAgentsAndTool(node.id, edges);

        downstreamAgentAndToolNodeIds.forEach((nodeId) => {
          const currentNode = getNode(nodeId);
          if (toBeDeletedNodes.every((x) => x.id !== nodeId) && currentNode) {
            toBeDeletedNodes.push(currentNode);
          }
        });

        downstreamAgentAndToolEdges.forEach((edge) => {
          if (toBeDeletedEdges.every((x) => x.id !== edge.id)) {
            toBeDeletedEdges.push(edge);
          }
        });
      }, []);
    }

    return {
      nodes: toBeDeletedNodes,
      edges: toBeDeletedEdges,
    };
  };

  return { handleBeforeDelete };
}

```

## High-Level Overview

      // This edge does not need to be deleted, the range of edges that do not need to be deleted is smaller, so consider the case where it does not need to be deleted

## Detailed Walkthrough

### Exports (1)

- `useBeforeDelete`: Exported entity

### Functions (6)

- `useBeforeDelete()`: Function definition
- `agentPredicate()`: Function definition
- `toBeDeletedNodes()`: Function definition
- `toBeDeletedEdges()`: Function definition
- `downStreamNodes()`: Function definition
- `currentNode()`: Function definition

### Imports (5)

- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { Node, OnBeforeDelete } from '@xyflow/react';`
- `import { Operator } from '../constant';`
- `import useGraphStore from '../store';`
- `import { deleteAllDownstreamAgentsAndTool } from '../utils/delete-node';`

## Code Structure Analysis

- Total lines: 83
- Blank lines: 15 (18.1%)
- Comment lines: ~2 (2.4%)
- Code lines: ~66


## Dependencies and Imports

- `@/interfaces/database/flow`
- `@xyflow/react`
- `../constant`
- `../store`
- `../utils/delete-node`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

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

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-before-delete.tsx`

## Keywords

../constant, ../store, ../utils/delete-node, @/interfaces/database/flow, @xyflow/react, Agent, Begin, Can, Cannot, Delete, Downstream, Edges, IterationStart, Node, Nodes, OnBeforeDelete, Operator, RAGFlowNodeType, This, TypeScript, UndeletableNodes, Upstream, agentPredicate, currentNode, downStreamNodes, handleBeforeDelete, operatorType, sourceType, toBeDeletedEdges, toBeDeletedNodes, useBeforeDelete, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
