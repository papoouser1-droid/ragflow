# Documentation: web/src/pages/agent/hooks/use-show-drawer.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-show-drawer.tsx`
- **Size**: 4724 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/hooks/use-show-drawer.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/hooks/use-show-drawer.tsx` is located in the `web/src/pages/agent/hooks` directory.

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
