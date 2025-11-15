# Documentation: web/src/pages/agent/hooks/use-placeholder-manager.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-placeholder-manager.ts`
- **Size**: 5601 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/hooks/use-placeholder-manager.ts`.

## Original Source Code

```ts
import { pick } from 'lodash';
import { useCallback, useRef } from 'react';
import { Operator } from '../constant';
import useGraphStore from '../store';

/**
 * Placeholder node management Hook
 * Responsible for managing placeholder node creation, deletion, and state tracking
 */
export const usePlaceholderManager = (reactFlowInstance: any) => {
  // Reference to the created placeholder node ID
  const createdPlaceholderRef = useRef<string | null>(null);
  // Flag indicating whether user has selected a node
  const userSelectedNodeRef = useRef(false);

  /**
   * Check if placeholder node exists and remove it if found
   * Ensures only one placeholder can exist on the panel
   */
  const checkAndRemoveExistingPlaceholder = useCallback(() => {
    const { nodes, edges } = useGraphStore.getState();

    // Find existing placeholder node
    const existingPlaceholder = nodes.find(
      (node) => node.data?.label === Operator.Placeholder,
    );

    if (existingPlaceholder && reactFlowInstance) {
      // Remove edges related to placeholder
      const edgesToRemove = edges.filter(
        (edge) =>
          edge.target === existingPlaceholder.id ||
          edge.source === existingPlaceholder.id,
      );

      // Remove placeholder node
      const nodesToRemove = [existingPlaceholder];

      if (nodesToRemove.length > 0 || edgesToRemove.length > 0) {
        reactFlowInstance.deleteElements({
          nodes: nodesToRemove,
          edges: edgesToRemove,
        });
      }

      // Update ref reference
      if (createdPlaceholderRef.current === existingPlaceholder.id) {
        createdPlaceholderRef.current = null;
      }
    }
  }, [reactFlowInstance]);

  /**
   * Function to remove placeholder node
   * Called when user clicks blank area or cancels operation
   */
  const removePlaceholderNode = useCallback(() => {
    if (
      createdPlaceholderRef.current &&
      reactFlowInstance &&
      !userSelectedNodeRef.current
    ) {
      const { nodes, edges } = useGraphStore.getState();

      // Remove edges related to placeholder
      const edgesToRemove = edges.filter(
        (edge) =>
          edge.target === createdPlaceholderRef.current ||
          edge.source === createdPlaceholderRef.current,
      );

      // Remove placeholder node
      const nodesToRemove = nodes.filter(
        (node) => node.id === createdPlaceholderRef.current,
      );

      if (nodesToRemove.length > 0 || edgesToRemove.length > 0) {
        reactFlowInstance.deleteElements({
          nodes: nodesToRemove,
          edges: edgesToRemove,
        });
      }

      createdPlaceholderRef.current = null;
    }

    // Reset user selection flag
    userSelectedNodeRef.current = false;
  }, [reactFlowInstance]);

  /**
   * User node selection callback
   * Called when user selects a node type from dropdown menu
   */
  const onNodeCreated = useCallback(
    (newNodeId: string) => {
      // First establish connection between new node and source, then delete placeholder
      if (createdPlaceholderRef.current && reactFlowInstance) {
        const { nodes, edges, addEdge, updateNode } = useGraphStore.getState();

        // Find placeholder node to get its position
        const placeholderNode = nodes.find(
          (node) => node.id === createdPlaceholderRef.current,
        );

        // Find placeholder-related connection and get source node info
        const placeholderEdge = edges.find(
          (edge) => edge.target === createdPlaceholderRef.current,
        );

        // Update new node position to match placeholder position
        if (placeholderNode) {
          const newNode = nodes.find((node) => node.id === newNodeId);
          if (newNode) {
            updateNode({
              ...newNode,
              ...pick(placeholderNode, ['position', 'parentId', 'extent']),
            });
          }
        }

        if (placeholderEdge) {
          // Establish connection between new node and source node
          addEdge({
            source: placeholderEdge.source,
            target: newNodeId,
            sourceHandle: placeholderEdge.sourceHandle || null,
            targetHandle: placeholderEdge.targetHandle || null,
          });
        }

        // Remove placeholder node and related connections
        const edgesToRemove = edges.filter(
          (edge) =>
            edge.target === createdPlaceholderRef.current ||
            edge.source === createdPlaceholderRef.current,
        );

        const nodesToRemove = nodes.filter(
          (node) => node.id === createdPlaceholderRef.current,
        );

        if (nodesToRemove.length > 0 || edgesToRemove.length > 0) {
          reactFlowInstance.deleteElements({
            nodes: nodesToRemove,
            edges: edgesToRemove,
          });
        }
      }

      // Mark that user has selected a node
      userSelectedNodeRef.current = true;
      createdPlaceholderRef.current = null;
    },
    [reactFlowInstance],
  );

  /**
   * Set the created placeholder node ID
   */
  const setCreatedPlaceholderRef = useCallback((nodeId: string | null) => {
    createdPlaceholderRef.current = nodeId;
  }, []);

  /**
   * Reset user selection flag
   */
  const resetUserSelectedFlag = useCallback(() => {
    userSelectedNodeRef.current = false;
  }, []);

  return {
    removePlaceholderNode,
    onNodeCreated,
    setCreatedPlaceholderRef,
    resetUserSelectedFlag,
    checkAndRemoveExistingPlaceholder,
    createdPlaceholderRef: createdPlaceholderRef.current,
    userSelectedNodeRef: userSelectedNodeRef.current,
  };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/hooks/use-placeholder-manager.ts` is located in the `web/src/pages/agent/hooks` directory.

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

- [use-add-node.ts](use-add-node.ts_docs.md)
- [use-agent-tool-initial-values.ts](use-agent-tool-initial-values.ts_docs.md)
- [use-before-delete.tsx](use-before-delete.tsx_docs.md)
- [use-build-dsl.ts](use-build-dsl.ts_docs.md)
- [use-build-options.tsx](use-build-options.tsx_docs.md)
- [use-build-structured-output.ts](use-build-structured-output.ts_docs.md)
- [use-cache-chat-log.ts](use-cache-chat-log.ts_docs.md)
- [use-calculate-sheet-right.ts](use-calculate-sheet-right.ts_docs.md)
- [use-cancel-dataflow.ts](use-cancel-dataflow.ts_docs.md)
- [use-change-node-name.ts](use-change-node-name.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
