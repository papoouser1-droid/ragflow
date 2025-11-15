# File Documentation: web/src/pages/agent/canvas/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 416
- **Characters**: 12,839
- **Size**: 12,839 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTheme } from '@/components/theme-provider';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { useSetModalState } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import {
  ConnectionMode,
  ControlButton,
  Controls,
  NodeTypes,
  Position,
  ReactFlow,
  ReactFlowInstance,
} from '@xyflow/react';
import '@xyflow/react/dist/style.css';
import { NotebookPen } from 'lucide-react';
import { useCallback, useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { ChatSheet } from '../chat/chat-sheet';
import {
  AgentChatContext,
  AgentChatLogContext,
  AgentInstanceContext,
  HandleContext,
} from '../context';

import FormSheet from '../form-sheet/next';
import { useSelectCanvasData, useValidateConnection } from '../hooks';
import { useAddNode } from '../hooks/use-add-node';
import { useBeforeDelete } from '../hooks/use-before-delete';
import { useCacheChatLog } from '../hooks/use-cache-chat-log';
import { useConnectionDrag } from '../hooks/use-connection-drag';
import { useDropdownPosition } from '../hooks/use-dropdown-position';
import { useMoveNote } from '../hooks/use-move-note';
import { usePlaceholderManager } from '../hooks/use-placeholder-manager';
import { useDropdownManager } from './context';

import { AgentBackground } from '@/components/canvas/background';
import Spotlight from '@/components/spotlight';
import {
  useHideFormSheetOnNodeDeletion,
  useShowDrawer,
  useShowLogSheet,
} from '../hooks/use-show-drawer';
import { useStopMessageUnmount } from '../hooks/use-stop-message';
import { LogSheet } from '../log-sheet';
import RunSheet from '../run-sheet';
import { ButtonEdge } from './edge';
import styles from './index.less';
import { RagNode } from './node';
import { AgentNode } from './node/agent-node';
import { BeginNode } from './node/begin-node';
import { CategorizeNode } from './node/categorize-node';
import { DataOperationsNode } from './node/data-operations-node';
import { NextStepDropdown } from './node/dropdown/next-step-dropdown';
import { ExtractorNode } from './node/extractor-node';
import { FileNode } from './node/file-node';
import { InvokeNode } from './node/invoke-node';
import { IterationNode, IterationStartNode } from './node/iteration-node';
import { KeywordNode } from './node/keyword-node';
import { ListOperationsNode } from './node/list-operations-node';
import { MessageNode } from './node/message-node';
import NoteNode from './node/note-node';
import ParserNode from './node/parser-node';
import { PlaceholderNode } from './node/placeholder-node';
import { RelevantNode } from './node/relevant-node';
import { RetrievalNode } from './node/retrieval-node';
import { RewriteNode } from './node/rewrite-node';
import { SplitterNode } from './node/splitter-node';
import { SwitchNode } from './node/switch-node';
import { TemplateNode } from './node/template-node';
import TokenizerNode from './node/tokenizer-node';
import { ToolNode } from './node/tool-node';
import { VariableAggregatorNode } from './node/variable-aggregator-node';
import { VariableAssignerNode } from './node/variable-assigner-node';

export const nodeTypes: NodeTypes = {
  ragNode: RagNode,
  categorizeNode: CategorizeNode,
  beginNode: BeginNode,
  placeholderNode: PlaceholderNode,
  relevantNode: RelevantNode,
  noteNode: NoteNode,
  switchNode: SwitchNode,
  retrievalNode: RetrievalNode,
  messageNode: MessageNode,
  rewriteNode: RewriteNode,
  keywordNode: KeywordNode,
  invokeNode: InvokeNode,
  templateNode: TemplateNode,
  // emailNode: EmailNode,
  group: IterationNode,
  iterationStartNode: IterationStartNode,
  agentNode: AgentNode,
  toolNode: ToolNode,
  fileNode: FileNode,
  parserNode: ParserNode,
  tokenizerNode: TokenizerNode,
  splitterNode: SplitterNode,
  contextNode: ExtractorNode,
  dataOperationsNode: DataOperationsNode,
  listOperationsNode: ListOperationsNode,
  variableAssignerNode: VariableAssignerNode,
  variableAggregatorNode: VariableAggregatorNode,
};

const edgeTypes = {
  buttonEdge: ButtonEdge,
};

interface IProps {
  drawerVisible: boolean;
  hideDrawer(): void;
}

function AgentCanvas({ drawerVisible, hideDrawer }: IProps) {
  const { t } = useTranslation();
  const {
    nodes,
    edges,
    onConnect: originalOnConnect,
    onEdgesChange,
    onNodesChange,
    onSelectionChange,
    onEdgeMouseEnter,
    onEdgeMouseLeave,
  } = useSelectCanvasData();
  const isValidConnection = useValidateConnection();

  const [reactFlowInstance, setReactFlowInstance] =
    useState<ReactFlowInstance<any, any>>();

  const {
    onNodeClick,
    clickedNode,
    formDrawerVisible,
    hideFormDrawer,
    singleDebugDrawerVisible,
    hideSingleDebugDrawer,
    showSingleDebugDrawer,
    chatVisible,
    runVisible,
    hideRunOrChatDrawer,
    showChatModal,
    showFormDrawer,
  } = useShowDrawer({
    drawerVisible,
    hideDrawer,
  });

  const {
    addEventList,
    setCurrentMessageId,
    currentEventListWithoutMessageById,
    clearEventList,
    currentMessageId,
    currentTaskId,
  } = useCacheChatLog();

  const { stopMessage } = useStopMessageUnmount(chatVisible, currentTaskId);

  const { showLogSheet, logSheetVisible, hideLogSheet } = useShowLogSheet({
    setCurrentMessageId,
  });
  const [lastSendLoading, setLastSendLoading] = useState(false);

  const { handleBeforeDelete } = useBeforeDelete();

  const { addCanvasNode, addNoteNode } = useAddNode(reactFlowInstance);

  const { ref, showImage, hideImage, imgVisible, mouse } = useMoveNote();

  const { theme } = useTheme();

  useEffect(() => {
    if (!chatVisible) {
      stopMessage(currentTaskId);
      clearEventList();
    }
  }, [chatVisible, clearEventList, currentTaskId, stopMessage]);

  const setLastSendLoadingFunc = (loading: boolean, messageId: string) => {
    if (messageId === currentMessageId) {
      setLastSendLoading(loading);
    } else {
      setLastSendLoading(false);
    }
  };

  useHideFormSheetOnNodeDeletion({ hideFormDrawer });

  const { visible, hideModal, showModal } = useSetModalState();
  const [dropdownPosition, setDropdownPosition] = useState({ x: 0, y: 0 });

  const { clearActiveDropdown } = useDropdownManager();

  const {
    removePlaceholderNode,
    onNodeCreated,
    setCreatedPlaceholderRef,
    checkAndRemoveExistingPlaceholder,
  } = usePlaceholderManager(reactFlowInstance);

  const { calculateDropdownPosition } = useDropdownPosition(reactFlowInstance);

  const {
    onConnectStart,
    onConnectEnd,
    handleConnect,
    getConnectionStartContext,
    shouldPreventClose,
    onMove,
    nodeId,
  } = useConnectionDrag(
    originalOnConnect,
    showModal,
    hideModal,
    setDropdownPosition,
    setCreatedPlaceholderRef,
    calculateDropdownPosition,
    removePlaceholderNode,
    clearActiveDropdown,
    checkAndRemoveExistingPlaceholder,
    reactFlowInstance,
  );

  const onPaneClick = useCallback(() => {
    hideFormDrawer();
    if (visible && !shouldPreventClose()) {
      removePlaceholderNode();
      hideModal();
      clearActiveDropdown();
    }
    if (imgVisible) {
      addNoteNode(mouse);
      hideImage();
    }
  }, [
    hideFormDrawer,
    visible,
    shouldPreventClose,
    hideModal,
    imgVisible,
    addNoteNode,
    mouse,
    hideImage,
    clearActiveDropdown,
    removePlaceholderNode,
  ]);

  return (
    <div className={cn(styles.canvasWrapper, 'px-5 pb-5')}>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        style={{ position: 'absolute', top: 10, left: 0 }}
      >
        <defs>
          <marker
            fill="rgb(var(--accent-primary))"
            id="selected-marker"
            viewBox="0 0 40 40"
            refX="8"
            refY="5"
            markerUnits="strokeWidth"
            markerWidth="20"
            markerHeight="20"
            orient="auto-start-reverse"
          >
            <path d="M 0 0 L 10 5 L 0 10 z" />
          </marker>
          <marker
            fill="var(--text-disabled)"
            id="logo"
            viewBox="0 0 40 40"
            refX="8"
            refY="5"
            markerUnits="strokeWidth"
            markerWidth="20"
            markerHeight="20"
            orient="auto-start-reverse"
          >
            <path d="M 0 0 L 10 5 L 0 10 z" />
          </marker>
        </defs>
      </svg>
      <AgentInstanceContext.Provider value={{ addCanvasNode, showFormDrawer }}>
        <ReactFlow
          connectionMode={ConnectionMode.Loose}
          nodes={nodes}
          onNodesChange={onNodesChange}
          edges={edges}
          onEdgesChange={onEdgesChange}
          fitView
          onConnect={handleConnect}
          nodeTypes={nodeTypes}
          edgeTypes={edgeTypes}
          onConnectStart={onConnectStart}
          onConnectEnd={onConnectEnd}
          onMove={onMove}
          onNodeClick={onNodeClick}
          onPaneClick={onPaneClick}
          onInit={setReactFlowInstance}
          onSelectionChange={onSelectionChange}
          nodeOrigin={[0.5, 0]}
          isValidConnection={isValidConnection}
          onEdgeMouseEnter={onEdgeMouseEnter}
          onEdgeMouseLeave={onEdgeMouseLeave}
          className="h-full"
          colorMode={theme}
          defaultEdgeOptions={{
            type: 'buttonEdge',
            markerEnd: 'logo',
            zIndex: 1001, // https://github.com/xyflow/xyflow/discussions/3498
          }}
          deleteKeyCode={['Delete', 'Backspace']}
          onBeforeDelete={handleBeforeDelete}
        >
          <AgentBackground></AgentBackground>
          <Spotlight className="z-0" opcity={0.7} coverage={70} />
          <Controls
            position={'bottom-center'}
            orientation="horizontal"
            className="bg-bg-base px-4 py-2 h-auto w-auto [&>button]:bg-transparent [&>button]:border-0 [&>button]:text-text-primary [&>button]:hover:bg-bg-base-hover [&>button]:hover:text-text-primary [&>button]:active:bg-bg-base-active [&>button]:p-0 [&>button]:size-4 gap-2.5 rounded-md"
          >
            <ControlButton>
              <Tooltip>
                <TooltipTrigger asChild>
                  <NotebookPen className="!fill-none" onClick={showImage} />
                </TooltipTrigger>
                <TooltipContent>{t('flow.note')}</TooltipContent>
              </Tooltip>
            </ControlButton>
          </Controls>
        </ReactFlow>
        {visible && (
          <HandleContext.Provider
            value={
              getConnectionStartContext() || {
                nodeId: '',
                id: '',
                type: 'source',
                position: Position.Right,
                isFromConnectionDrag: true,
              }
            }
          >
            <NextStepDropdown
              hideModal={() => {
                removePlaceholderNode();
                hideModal();
                clearActiveDropdown();
              }}
              position={dropdownPosition}
              onNodeCreated={onNodeCreated}
              nodeId={nodeId}
            >
              <span></span>
            </NextStepDropdown>
          </HandleContext.Provider>
        )}
      </AgentInstanceContext.Provider>
      <NotebookPen
        className={cn('hidden absolute size-6', { block: imgVisible })}
        ref={ref}
      ></NotebookPen>
      {formDrawerVisible && (
        <AgentInstanceContext.Provider
          value={{ addCanvasNode, showFormDrawer }}
        >
          <FormSheet
            node={clickedNode}
            visible={formDrawerVisible}
            hideModal={hideFormDrawer}
            chatVisible={chatVisible}
            singleDebugDrawerVisible={singleDebugDrawerVisible}
            hideSingleDebugDrawer={hideSingleDebugDrawer}
            showSingleDebugDrawer={showSingleDebugDrawer}
          ></FormSheet>
        </AgentInstanceContext.Provider>
      )}
      {chatVisible && (
        <AgentChatContext.Provider
          value={{ showLogSheet, setLastSendLoadingFunc }}
        >
          <AgentChatLogContext.Provider
            value={{ addEventList, setCurrentMessageId }}
          >
            <ChatSheet hideModal={hideRunOrChatDrawer}></ChatSheet>
          </AgentChatLogContext.Provider>
        </AgentChatContext.Provider>
      )}
      {runVisible && (
        <RunSheet
          hideModal={hideRunOrChatDrawer}
          showModal={showChatModal}
        ></RunSheet>
      )}
      {logSheetVisible && (
        <LogSheet
          hideModal={hideLogSheet}
          currentEventListWithoutMessageById={
            currentEventListWithoutMessageById
          }
          currentMessageId={currentMessageId}
          sendLoading={lastSendLoading}
        ></LogSheet>
      )}
    </div>
  );
}

export default AgentCanvas;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 416 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `nodeTypes`: Exported entity

### Functions (3)

- `AgentCanvas()`: Function definition
- `setLastSendLoadingFunc()`: Function definition
- `onPaneClick()`: Function definition

### Imports (55)

- `import { useTheme } from '@/components/theme-provider';`
- `import {`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { cn } from '@/lib/utils';`
- `import {`
- `import '@xyflow/react/dist/style.css';`
- `import { NotebookPen } from 'lucide-react';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { ChatSheet } from '../chat/chat-sheet';`

## Code Structure Analysis

- Total lines: 416
- Blank lines: 27 (6.5%)
- Comment lines: ~1 (0.2%)
- Code lines: ~388


## Dependencies and Imports

- `@/components/theme-provider`
- `@/hooks/common-hooks`
- `@/lib/utils`
- `lucide-react`
- `react`
- `react-i18next`
- `../chat/chat-sheet`
- `../form-sheet/next`
- `../hooks`
- `../hooks/use-add-node`
- `../hooks/use-before-delete`
- `../hooks/use-cache-chat-log`
- `../hooks/use-connection-drag`
- `../hooks/use-dropdown-position`
- `../hooks/use-move-note`
- `../hooks/use-placeholder-manager`
- `./context`
- `@/components/canvas/background`
- `@/components/spotlight`
- `../hooks/use-stop-message`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/canvas/` directory
- Potential test file: `test_index.tsx`

## Keywords

../chat/chat-sheet, ../form-sheet/next, ../hooks, ../hooks/use-add-node, ../hooks/use-before-delete, ../hooks/use-cache-chat-log, ../hooks/use-connection-drag, ../hooks/use-dropdown-position, ../hooks/use-move-note, ../hooks/use-placeholder-manager, ../hooks/use-stop-message, ../log-sheet, ../run-sheet, ./context, ./edge, ./index.less, ./node, ./node/agent-node, ./node/begin-node, ./node/categorize-node, ./node/data-operations-node, ./node/dropdown/next-step-dropdown, ./node/extractor-node, ./node/file-node, ./node/invoke-node, ./node/iteration-node, ./node/keyword-node, ./node/list-operations-node, ./node/message-node, ./node/note-node, ./node/parser-node, ./node/placeholder-node, ./node/relevant-node, ./node/retrieval-node, ./node/rewrite-node, ./node/splitter-node, ./node/switch-node, ./node/template-node, ./node/tokenizer-node, ./node/tool-node, ./node/variable-aggregator-node, ./node/variable-assigner-node, @/components/canvas/background, @/components/spotlight, @/components/theme-provider, @/hooks/common-hooks, @/lib/utils, AgentBackground, AgentCanvas, AgentChatContext...

---
*Generated by RAGFlow Repository Documentation Generator*
