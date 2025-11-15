# Documentation: web/src/pages/agent/form/agent-form/agent-tools.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/agent-tools.tsx`
- **Size**: 5831 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/agent-form/agent-tools.tsx`.

## Original Source Code

```tsx
import { BlockButton } from '@/components/ui/button';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { cn } from '@/lib/utils';
import { Position } from '@xyflow/react';
import { t } from 'i18next';
import { PencilLine, X } from 'lucide-react';
import {
  MouseEventHandler,
  PropsWithChildren,
  useCallback,
  useContext,
  useMemo,
} from 'react';
import { LabelCard } from '../../canvas/node/card';
import { Operator } from '../../constant';
import { AgentInstanceContext } from '../../context';
import { useFindMcpById } from '../../hooks/use-find-mcp-by-id';
import { INextOperatorForm } from '../../interface';
import OperatorIcon from '../../operator-icon';
import useGraphStore from '../../store';
import { filterDownstreamAgentNodeIds } from '../../utils/filter-downstream-nodes';
import { ToolPopover } from './tool-popover';
import { useDeleteAgentNodeMCP } from './tool-popover/use-update-mcp';
import { useDeleteAgentNodeTools } from './tool-popover/use-update-tools';
import { useGetAgentMCPIds, useGetAgentToolNames } from './use-get-tools';

type ToolCardProps = React.HTMLAttributes<HTMLLIElement> &
  PropsWithChildren & {
    isNodeTool?: boolean;
  };

export function ToolCard({
  children,
  className,
  isNodeTool = true,
  ...props
}: ToolCardProps) {
  const element = useMemo(() => {
    return (
      <LabelCard
        {...props}
        className={cn(
          'flex justify-between ',
          { 'p-2.5 text-text-primary text-sm': !isNodeTool },
          className,
        )}
      >
        {children}
      </LabelCard>
    );
  }, [children, className, isNodeTool, props]);

  if (children === Operator.Code) {
    return (
      <Tooltip>
        <TooltipTrigger asChild>{element}</TooltipTrigger>
        <TooltipContent>
          <p>It doesn't have any config.</p>
        </TooltipContent>
      </Tooltip>
    );
  }

  return element;
}

type ActionButtonProps<T> = {
  record: T;
  deleteRecord(record: T): void;
  edit: MouseEventHandler<HTMLOrSVGElement>;
};

function ActionButton<T>({ deleteRecord, record, edit }: ActionButtonProps<T>) {
  const handleDelete = useCallback(() => {
    deleteRecord(record);
  }, [deleteRecord, record]);

  return (
    <div className="flex items-center gap-4 text-text-secondary">
      <PencilLine
        className="size-3.5 cursor-pointer"
        data-tool={record}
        onClick={edit}
      />
      <X className="size-3.5 cursor-pointer" onClick={handleDelete} />
    </div>
  );
}

export function AgentTools() {
  const { toolNames } = useGetAgentToolNames();
  const { deleteNodeTool } = useDeleteAgentNodeTools();
  const { mcpIds } = useGetAgentMCPIds();
  const { findMcpById } = useFindMcpById();
  const { deleteNodeMCP } = useDeleteAgentNodeMCP();
  const { showFormDrawer } = useContext(AgentInstanceContext);
  const { clickedNodeId, findAgentToolNodeById, selectNodeIds } = useGraphStore(
    (state) => state,
  );

  const handleEdit: MouseEventHandler<SVGSVGElement> = useCallback(
    (e) => {
      const toolNodeId = findAgentToolNodeById(clickedNodeId);
      if (toolNodeId) {
        selectNodeIds([toolNodeId]);
        showFormDrawer(e, toolNodeId);
      }
    },
    [clickedNodeId, findAgentToolNodeById, selectNodeIds, showFormDrawer],
  );

  return (
    <section className="space-y-2.5">
      <span className="text-text-secondary text-sm">{t('flow.tools')}</span>
      <ul className="space-y-2.5">
        {toolNames.map((x) => (
          <ToolCard key={x} isNodeTool={false}>
            <div className="flex gap-2 items-center">
              <OperatorIcon name={x as Operator}></OperatorIcon>
              {x}
            </div>
            <ActionButton
              record={x}
              deleteRecord={deleteNodeTool(x)}
              edit={handleEdit}
            ></ActionButton>
          </ToolCard>
        ))}
        {mcpIds.map((id) => (
          <ToolCard key={id} isNodeTool={false}>
            {findMcpById(id)?.name}
            <ActionButton
              record={id}
              deleteRecord={deleteNodeMCP(id)}
              edit={handleEdit}
            ></ActionButton>
          </ToolCard>
        ))}
      </ul>
      <ToolPopover>
        <BlockButton>{t('flow.addTools')}</BlockButton>
      </ToolPopover>
    </section>
  );
}

export function Agents({ node }: INextOperatorForm) {
  const { addCanvasNode } = useContext(AgentInstanceContext);
  const { deleteAgentDownstreamNodesById, edges, getNode, selectNodeIds } =
    useGraphStore((state) => state);
  const { showFormDrawer } = useContext(AgentInstanceContext);

  const handleEdit = useCallback(
    (nodeId: string): MouseEventHandler<SVGSVGElement> =>
      (e) => {
        selectNodeIds([nodeId]);
        showFormDrawer(e, nodeId);
      },
    [selectNodeIds, showFormDrawer],
  );

  const subBottomAgentNodeIds = useMemo(() => {
    return filterDownstreamAgentNodeIds(edges, node?.id);
  }, [edges, node?.id]);

  return (
    <section className="space-y-2.5">
      <span className="text-text-secondary text-sm">{t('flow.agent')}</span>
      <ul className="space-y-2.5">
        {subBottomAgentNodeIds.map((id) => {
          const currentNode = getNode(id);

          return (
            <ToolCard key={id} isNodeTool={false}>
              {currentNode?.data.name}
              <ActionButton
                record={id}
                deleteRecord={deleteAgentDownstreamNodesById}
                edit={handleEdit(id)}
              ></ActionButton>
            </ToolCard>
          );
        })}
      </ul>
      <BlockButton
        onClick={addCanvasNode(Operator.Agent, {
          nodeId: node?.id,
          position: Position.Bottom,
        })}
      >
        {t('flow.addAgent')}
      </BlockButton>
    </section>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/agent-form/agent-tools.tsx` is located in the `web/src/pages/agent/form/agent-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to agent-form.

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

- [dynamic-prompt.tsx](dynamic-prompt.tsx_docs.md)
- [dynamic-tool.tsx](dynamic-tool.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [structured-output-dialog.tsx](structured-output-dialog.tsx_docs.md)
- [structured-output-panel.tsx](structured-output-panel.tsx_docs.md)
- [use-build-prompt-options.ts](use-build-prompt-options.ts_docs.md)
- [use-get-tools.ts](use-get-tools.ts_docs.md)
- [use-show-structured-output-dialog.ts](use-show-structured-output-dialog.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
