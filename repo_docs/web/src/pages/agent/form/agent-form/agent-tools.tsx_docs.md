# File Documentation: web/src/pages/agent/form/agent-form/agent-tools.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/agent-tools.tsx`
- **Extension**: `.tsx`
- **Lines**: 200
- **Characters**: 5,831
- **Size**: 5,831 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/agent-tools.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 200 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `ToolCard`: Exported entity
- `AgentTools`: Exported entity
- `Agents`: Exported entity

### Functions (8)

- `ToolCard()`: Function definition
- `element()`: Function definition
- `ActionButton()`: Function definition
- `handleDelete()`: Function definition
- `AgentTools()`: Function definition
- `Agents()`: Function definition
- `handleEdit()`: Function definition
- `subBottomAgentNodeIds()`: Function definition

### Imports (19)

- `import { BlockButton } from '@/components/ui/button';`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { Position } from '@xyflow/react';`
- `import { t } from 'i18next';`
- `import { PencilLine, X } from 'lucide-react';`
- `import {`
- `import { LabelCard } from '../../canvas/node/card';`
- `import { Operator } from '../../constant';`
- `import { AgentInstanceContext } from '../../context';`

## Code Structure Analysis

- Total lines: 200
- Blank lines: 16 (8.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~184


## Dependencies and Imports

- `@/components/ui/button`
- `@/lib/utils`
- `@xyflow/react`
- `i18next`
- `lucide-react`
- `../../canvas/node/card`
- `../../constant`
- `../../context`
- `../../hooks/use-find-mcp-by-id`
- `../../interface`
- `../../operator-icon`
- `../../store`
- `../../utils/filter-downstream-nodes`
- `./tool-popover`
- `./tool-popover/use-update-mcp`
- `./tool-popover/use-update-tools`
- `./use-get-tools`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form`.

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

- Other files in `web/src/pages/agent/form/agent-form/` directory
- Potential test file: `test_agent-tools.tsx`

## Keywords

../../canvas/node/card, ../../constant, ../../context, ../../hooks/use-find-mcp-by-id, ../../interface, ../../operator-icon, ../../store, ../../utils/filter-downstream-nodes, ./tool-popover, ./tool-popover/use-update-mcp, ./tool-popover/use-update-tools, ./use-get-tools, @/components/ui/button, @/lib/utils, @xyflow/react, ActionButton, ActionButtonProps, Agent, AgentInstanceContext, AgentTools, Agents, BlockButton, Bottom, Code, HTMLAttributes, HTMLLIElement, HTMLOrSVGElement, INextOperatorForm, LabelCard, MouseEventHandler, Operator, OperatorIcon, PencilLine, Position, PropsWithChildren, React, SVGSVGElement, ToolCard, ToolCardProps, ToolPopover, Tooltip, TooltipContent, TooltipTrigger, TypeScript, currentNode, element, handleDelete, handleEdit, i18next, lucide-react...

---
*Generated by RAGFlow Repository Documentation Generator*
