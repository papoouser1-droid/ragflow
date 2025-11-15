# Documentation: web/src/pages/agent/canvas/node/agent-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/agent-node.tsx`
- **Size**: 4042 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/canvas/node/agent-node.tsx`.

## Original Source Code

```tsx
import { IAgentNode } from '@/interfaces/database/flow';
import { cn } from '@/lib/utils';
import { Handle, NodeProps, Position } from '@xyflow/react';
import { get } from 'lodash';
import { memo, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { AgentExceptionMethod, NodeHandleId } from '../../constant';
import { AgentFormSchemaType } from '../../form/agent-form';
import useGraphStore from '../../store';
import { hasSubAgent, isBottomSubAgent } from '../../utils';
import { LLMLabelCard } from './card';
import { CommonHandle, LeftEndHandle } from './handle';
import { RightHandleStyle } from './handle-icon';
import NodeHeader from './node-header';
import { NodeWrapper } from './node-wrapper';
import { ToolBar } from './toolbar';

function InnerAgentNode({
  id,
  data,
  isConnectable = true,
  selected,
}: NodeProps<IAgentNode<AgentFormSchemaType>>) {
  const edges = useGraphStore((state) => state.edges);
  const { t } = useTranslation();

  const isHeadAgent = useMemo(() => {
    return !isBottomSubAgent(edges, id);
  }, [edges, id]);

  const exceptionMethod = useMemo(() => {
    return get(data, 'form.exception_method');
  }, [data]);

  const hasTools = useMemo(() => {
    const tools = get(data, 'form.tools', []);
    const mcp = get(data, 'form.mcp', []);
    return tools.length > 0 || mcp.length > 0;
  }, [data]);

  const isGotoMethod = useMemo(() => {
    return exceptionMethod === AgentExceptionMethod.Goto;
  }, [exceptionMethod]);

  return (
    <ToolBar selected={selected} id={id} label={data.label}>
      <NodeWrapper selected={selected}>
        {isHeadAgent && (
          <>
            <LeftEndHandle></LeftEndHandle>
            <CommonHandle
              type="source"
              position={Position.Right}
              isConnectable={isConnectable}
              style={RightHandleStyle}
              nodeId={id}
              id={NodeHandleId.Start}
              isConnectableEnd={false}
            ></CommonHandle>
          </>
        )}
        {isHeadAgent || (
          <Handle
            type="target"
            position={Position.Top}
            isConnectable={false}
            id={NodeHandleId.AgentTop}
            className="!bg-accent-primary !size-2"
          ></Handle>
        )}
        <Handle
          type="source"
          position={Position.Bottom}
          isConnectable={false}
          id={NodeHandleId.AgentBottom}
          style={{ left: 180 }}
          className={cn('!bg-accent-primary !size-2 invisible', {
            visible: hasSubAgent(edges, id),
          })}
        ></Handle>
        <Handle
          type="source"
          position={Position.Bottom}
          isConnectable={false}
          id={NodeHandleId.Tool}
          style={{ left: 20 }}
          className={cn('!bg-accent-primary !size-2 invisible', {
            visible: hasTools,
          })}
        ></Handle>
        <NodeHeader id={id} name={data.name} label={data.label}></NodeHeader>
        <section className="flex flex-col gap-2">
          <LLMLabelCard llmId={get(data, 'form.llm_id')}></LLMLabelCard>
          {(isGotoMethod ||
            exceptionMethod === AgentExceptionMethod.Comment) && (
            <div className="bg-bg-card rounded-sm p-1 flex justify-between gap-2">
              <span className="text-text-secondary">{t('flow.onFailure')}</span>
              <span className="truncate flex-1 text-right">
                {t(`flow.${exceptionMethod}`)}
              </span>
            </div>
          )}
        </section>
        {isGotoMethod && (
          <CommonHandle
            type="source"
            position={Position.Right}
            isConnectable={isConnectable}
            className="!bg-state-error"
            style={{ ...RightHandleStyle, top: 94 }}
            nodeId={id}
            id={NodeHandleId.AgentException}
            isConnectableEnd={false}
          ></CommonHandle>
        )}
      </NodeWrapper>
    </ToolBar>
  );
}

export const AgentNode = memo(InnerAgentNode);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/canvas/node/agent-node.tsx` is located in the `web/src/pages/agent/canvas/node` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to node.

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

- [begin-node.tsx](begin-node.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [categorize-node.tsx](categorize-node.tsx_docs.md)
- [data-operations-node.tsx](data-operations-node.tsx_docs.md)
- [dropdown.tsx](dropdown.tsx_docs.md)
- [email-node.tsx](email-node.tsx_docs.md)
- [extractor-node.tsx](extractor-node.tsx_docs.md)
- [file-node.tsx](file-node.tsx_docs.md)
- [handle-icon.tsx](handle-icon.tsx_docs.md)
- [handle.tsx](handle.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
