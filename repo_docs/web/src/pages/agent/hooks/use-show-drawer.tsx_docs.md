# File Documentation: web/src/pages/agent/hooks/use-show-drawer.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-show-drawer.tsx`
- **Extension**: `.tsx`
- **Lines**: 187
- **Characters**: 4,724
- **Size**: 4,724 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useSetModalState } from '@/hooks/common-hooks';
import { NodeMouseHandler } from '@xyflow/react';
import get from 'lodash/get';
import React, { useCallback, useEffect } from 'react';
import { Operator } from '../constant';
import useGraphStore from '../store';
import { useCacheChatLog } from './use-cache-chat-log';
import { useGetBeginNodeDataInputs } from './use-get-begin-query';
import { useSaveGraph } from './use-save-graph';

export const useShowFormDrawer = () => {
  const {
    clickedNodeId: clickNodeId,
    setClickedNodeId,
    getNode,
    setClickedToolId,
  } = useGraphStore((state) => state);
  const {
    visible: formDrawerVisible,
    hideModal: hideFormDrawer,
    showModal: showFormDrawer,
  } = useSetModalState();

  const handleShow = useCallback(
    (e: React.MouseEvent<Element>, nodeId: string) => {
      const tool = get(e.target, 'dataset.tool');
      // TODO: Operator type judgment should be used
      if (nodeId.startsWith(Operator.Tool) && !tool) {
        return;
      }
      setClickedNodeId(nodeId);
      setClickedToolId(tool);
      showFormDrawer();
    },
    [setClickedNodeId, setClickedToolId, showFormDrawer],
  );

  return {
    formDrawerVisible,
    hideFormDrawer,
    showFormDrawer: handleShow,
    clickedNode: getNode(clickNodeId),
  };
};

export const useShowSingleDebugDrawer = () => {
  const { visible, showModal, hideModal } = useSetModalState();
  const { saveGraph } = useSaveGraph();

  const showSingleDebugDrawer = useCallback(async () => {
    const saveRet = await saveGraph();
    if (saveRet?.code === 0) {
      showModal();
    }
  }, [saveGraph, showModal]);

  return {
    singleDebugDrawerVisible: visible,
    hideSingleDebugDrawer: hideModal,
    showSingleDebugDrawer,
  };
};

const ExcludedNodes = [Operator.Note, Operator.Placeholder, Operator.File];

export function useShowDrawer({
  drawerVisible,
  hideDrawer,
}: {
  drawerVisible: boolean;
  hideDrawer(): void;
}) {
  const {
    visible: runVisible,
    showModal: showRunModal,
    hideModal: hideRunModal,
  } = useSetModalState();
  const {
    visible: chatVisible,
    showModal: showChatModal,
    hideModal: hideChatModal,
  } = useSetModalState();
  const {
    singleDebugDrawerVisible,
    showSingleDebugDrawer,
    hideSingleDebugDrawer,
  } = useShowSingleDebugDrawer();
  const { formDrawerVisible, hideFormDrawer, showFormDrawer, clickedNode } =
    useShowFormDrawer();
  const inputs = useGetBeginNodeDataInputs();

  useEffect(() => {
    if (drawerVisible) {
      if (inputs.length > 0) {
        showRunModal();
        hideChatModal();
      } else {
        showChatModal();
        hideRunModal();
      }
    }
  }, [
    hideChatModal,
    hideRunModal,
    showChatModal,
    showRunModal,
    drawerVisible,
    inputs,
  ]);

  const hideRunOrChatDrawer = useCallback(() => {
    hideChatModal();
    hideRunModal();
    hideDrawer();
  }, [hideChatModal, hideDrawer, hideRunModal]);

  const onPaneClick = useCallback(() => {
    hideFormDrawer();
  }, [hideFormDrawer]);

  const onNodeClick: NodeMouseHandler = useCallback(
    (e, node) => {
      if (!ExcludedNodes.some((x) => x === node.data.label)) {
        hideSingleDebugDrawer();
        // hideRunOrChatDrawer();
        showFormDrawer(e, node.id);
      }
      // handle single debug icon click
      if (
        get(e.target, 'dataset.play') === 'true' ||
        get(e.target, 'parentNode.dataset.play') === 'true'
      ) {
        showSingleDebugDrawer();
      }
    },
    [hideSingleDebugDrawer, showFormDrawer, showSingleDebugDrawer],
  );

  return {
    chatVisible,
    runVisible,
    onPaneClick,
    singleDebugDrawerVisible,
    showSingleDebugDrawer,
    hideSingleDebugDrawer,
    formDrawerVisible,
    showFormDrawer,
    clickedNode,
    onNodeClick,
    hideFormDrawer,
    hideRunOrChatDrawer,
    showChatModal,
  };
}

export function useShowLogSheet({
  setCurrentMessageId,
}: Pick<ReturnType<typeof useCacheChatLog>, 'setCurrentMessageId'>) {
  const { visible, showModal, hideModal } = useSetModalState();

  const handleShow = useCallback(
    (messageId: string) => {
      setCurrentMessageId(messageId);
      showModal();
    },
    [setCurrentMessageId, showModal],
  );

  return {
    logSheetVisible: visible,
    hideLogSheet: hideModal,
    showLogSheet: handleShow,
  };
}

export function useHideFormSheetOnNodeDeletion({
  hideFormDrawer,
}: Pick<ReturnType<typeof useShowFormDrawer>, 'hideFormDrawer'>) {
  const { nodes, clickedNodeId } = useGraphStore((state) => state);

  useEffect(() => {
    if (!nodes.some((x) => x.id === clickedNodeId)) {
      hideFormDrawer();
    }
  }, [clickedNodeId, hideFormDrawer, nodes]);
}

```

## High-Level Overview

      // TODO: Operator type judgment should be used

## Detailed Walkthrough

### Exports (5)

- `useShowFormDrawer`: Exported entity
- `useShowSingleDebugDrawer`: Exported entity
- `useShowDrawer`: Exported entity
- `useShowLogSheet`: Exported entity
- `useHideFormSheetOnNodeDeletion`: Exported entity

### Functions (11)

- `useShowFormDrawer()`: Function definition
- `handleShow()`: Function definition
- `useShowSingleDebugDrawer()`: Function definition
- `showSingleDebugDrawer()`: Function definition
- `useShowDrawer()`: Function definition
- `inputs()`: Function definition
- `hideRunOrChatDrawer()`: Function definition
- `onPaneClick()`: Function definition
- `useShowLogSheet()`: Function definition
- `handleShow()`: Function definition
- `useHideFormSheetOnNodeDeletion()`: Function definition

### Imports (9)

- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { NodeMouseHandler } from '@xyflow/react';`
- `import get from 'lodash/get';`
- `import React, { useCallback, useEffect } from 'react';`
- `import { Operator } from '../constant';`
- `import useGraphStore from '../store';`
- `import { useCacheChatLog } from './use-cache-chat-log';`
- `import { useGetBeginNodeDataInputs } from './use-get-begin-query';`
- `import { useSaveGraph } from './use-save-graph';`

## Code Structure Analysis

- Total lines: 187
- Blank lines: 19 (10.2%)
- Comment lines: ~3 (1.6%)
- Code lines: ~165


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@xyflow/react`
- `lodash/get`
- `react`
- `../constant`
- `../store`
- `./use-cache-chat-log`
- `./use-get-begin-query`
- `./use-save-graph`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

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

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-show-drawer.tsx`

## Keywords

../constant, ../store, ./use-cache-chat-log, ./use-get-begin-query, ./use-save-graph, @/hooks/common-hooks, @xyflow/react, Element, ExcludedNodes, File, MouseEvent, NodeMouseHandler, Note, Operator, Pick, Placeholder, React, ReturnType, TODO, Tool, TypeScript, handleShow, hideRunOrChatDrawer, inputs, judgment, lodash/get, onNodeClick, onPaneClick, react, saveRet, showSingleDebugDrawer, tool, useHideFormSheetOnNodeDeletion, useShowDrawer, useShowFormDrawer, useShowLogSheet, useShowSingleDebugDrawer, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
