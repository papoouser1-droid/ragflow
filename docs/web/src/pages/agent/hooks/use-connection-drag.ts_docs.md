# Documentation: web/src/pages/agent/hooks/use-connection-drag.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-connection-drag.ts`
- **Size**: 6430 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/hooks/use-connection-drag.ts`.

## Original Source Code

```ts
import {
  Connection,
  OnConnectEnd,
  OnConnectStart,
  Position,
  ReactFlowInstance,
} from '@xyflow/react';
import { useCallback, useRef } from 'react';
import { useDropdownManager } from '../canvas/context';
import { Operator, PREVENT_CLOSE_DELAY } from '../constant';
import { useAddNode } from './use-add-node';

interface ConnectionStartParams {
  nodeId: string;
  handleId: string;
}

/**
 * Connection drag management Hook
 * Responsible for handling connection drag start and end logic
 */
export const useConnectionDrag = (
  onConnect: (connection: Connection) => void,
  showModal: () => void,
  hideModal: () => void,
  setDropdownPosition: (position: { x: number; y: number }) => void,
  setCreatedPlaceholderRef: (nodeId: string | null) => void,
  calculateDropdownPosition: (
    clientX: number,
    clientY: number,
  ) => { x: number; y: number },
  removePlaceholderNode: () => void,
  clearActiveDropdown: () => void,
  checkAndRemoveExistingPlaceholder: () => void,
  reactFlowInstance?: ReactFlowInstance<any, any>,
) => {
  // Reference for whether connection is established
  const isConnectedRef = useRef(false);
  // Reference for connection start parameters
  const connectionStartRef = useRef<ConnectionStartParams | null>(null);
  // Reference to prevent immediate close
  const preventCloseRef = useRef(false);
  // Reference to track mouse position for click detection
  const mouseStartPosRef = useRef<{ x: number; y: number } | null>(null);

  const { addCanvasNode } = useAddNode(reactFlowInstance);
  const { setActiveDropdown } = useDropdownManager();

  /**
   * Connection start handler function
   */
  const onConnectStart: OnConnectStart = useCallback((event, params) => {
    isConnectedRef.current = false;

    // Record mouse start position to detect click vs drag
    if ('clientX' in event && 'clientY' in event) {
      mouseStartPosRef.current = { x: event.clientX, y: event.clientY };
    }

    if (params && params.nodeId && params.handleId) {
      connectionStartRef.current = {
        nodeId: params.nodeId,
        handleId: params.handleId,
      };
    } else {
      connectionStartRef.current = null;
    }
  }, []);

  /**
   * Connection end handler function
   */
  const onConnectEnd: OnConnectEnd = useCallback(
    (event) => {
      if ('clientX' in event && 'clientY' in event) {
        const { clientX, clientY } = event;
        setDropdownPosition({ x: clientX, y: clientY });

        if (!isConnectedRef.current && connectionStartRef.current) {
          // Check mouse movement distance to distinguish click from drag
          let isHandleClick = false;
          if (mouseStartPosRef.current) {
            const movementDistance = Math.sqrt(
              Math.pow(clientX - mouseStartPosRef.current.x, 2) +
                Math.pow(clientY - mouseStartPosRef.current.y, 2),
            );
            isHandleClick = movementDistance < 5; // Consider clicks within 5px as handle clicks
          }

          if (isHandleClick) {
            removePlaceholderNode();
            hideModal();
            clearActiveDropdown();
            connectionStartRef.current = null;
            mouseStartPosRef.current = null;
            return;
          }

          // Check and remove existing placeholder-node before creating new one
          checkAndRemoveExistingPlaceholder();

          // Create placeholder node and establish connection
          const mockEvent = { clientX, clientY };
          const contextData = {
            nodeId: connectionStartRef.current.nodeId,
            id: connectionStartRef.current.handleId,
            type: 'source' as const,
            position: Position.Right,
            isFromConnectionDrag: true,
          };

          // Use Placeholder operator to create node
          const newNodeId = addCanvasNode(
            Operator.Placeholder,
            contextData,
          )(mockEvent);

          if (newNodeId) {
            setCreatedPlaceholderRef(newNodeId);
          }

          // Calculate placeholder node position and display dropdown menu
          if (newNodeId && reactFlowInstance) {
            const dropdownScreenPosition = calculateDropdownPosition(
              clientX,
              clientY,
            );

            setDropdownPosition({
              x: dropdownScreenPosition.x,
              y: dropdownScreenPosition.y,
            });

            setActiveDropdown('drag');
            showModal();
            preventCloseRef.current = true;
            setTimeout(() => {
              preventCloseRef.current = false;
            }, PREVENT_CLOSE_DELAY);
          }

          // Reset connection state
          connectionStartRef.current = null;
          mouseStartPosRef.current = null;
        }
      }
    },
    [
      setDropdownPosition,
      checkAndRemoveExistingPlaceholder,
      addCanvasNode,
      reactFlowInstance,
      removePlaceholderNode,
      hideModal,
      clearActiveDropdown,
      setCreatedPlaceholderRef,
      calculateDropdownPosition,
      setActiveDropdown,
      showModal,
    ],
  );

  /**
   * Connection establishment handler function
   */
  const handleConnect = useCallback(
    (connection: Connection) => {
      onConnect(connection);
      isConnectedRef.current = true;
    },
    [onConnect],
  );

  /**
   * Get connection start context data
   */
  const getConnectionStartContext = useCallback(() => {
    if (!connectionStartRef.current) {
      return null;
    }

    return {
      nodeId: connectionStartRef.current.nodeId,
      id: connectionStartRef.current.handleId,
      type: 'source' as const,
      position: Position.Right,
      isFromConnectionDrag: true,
    };
  }, []);

  /**
   * Check if close should be prevented
   */
  const shouldPreventClose = useCallback(() => {
    return preventCloseRef.current;
  }, []);

  /**
   * Handle canvas move/zoom events
   * Hide dropdown and remove placeholder when user scrolls or moves canvas
   */
  const onMove = useCallback(() => {
    // Clean up placeholder and dropdown when canvas moves/zooms
    removePlaceholderNode();
    hideModal();
    clearActiveDropdown();
  }, [removePlaceholderNode, hideModal, clearActiveDropdown]);

  return {
    nodeId: connectionStartRef.current?.nodeId,
    onConnectStart,
    onConnectEnd,
    handleConnect,
    getConnectionStartContext,
    shouldPreventClose,
    onMove,
  };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/hooks/use-connection-drag.ts` is located in the `web/src/pages/agent/hooks` directory.

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
