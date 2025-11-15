# Documentation: web/src/pages/agent/canvas/node/switch-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/switch-node.tsx`
- **Size**: 4062 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/canvas/node/switch-node.tsx`.

## Original Source Code

```tsx
import { Card, CardContent } from '@/components/ui/card';
import { SwitchOperatorOptions } from '@/constants/agent';
import { LogicalOperatorIcon } from '@/hooks/logic-hooks/use-build-operator-options';
import { ISwitchCondition, ISwitchNode } from '@/interfaces/database/flow';
import { NodeProps, Position } from '@xyflow/react';
import { memo, useCallback } from 'react';
import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';
import { CommonHandle, LeftEndHandle } from './handle';
import { RightHandleStyle } from './handle-icon';
import NodeHeader from './node-header';
import { NodeWrapper } from './node-wrapper';
import { ToolBar } from './toolbar';
import { useBuildSwitchHandlePositions } from './use-build-switch-handle-positions';

const getConditionKey = (idx: number, length: number) => {
  if (idx === 0 && length !== 1) {
    return 'If';
  } else if (idx === length - 1) {
    return 'Else';
  }

  return 'ElseIf';
};

const ConditionBlock = ({
  condition,
  nodeId,
}: { condition: ISwitchCondition } & { nodeId: string }) => {
  const items = condition?.items ?? [];
  const { getLabel } = useGetVariableLabelOrTypeByValue(nodeId);

  const renderOperatorIcon = useCallback((operator?: string) => {
    const item = SwitchOperatorOptions.find((x) => x.value === operator);
    if (item) {
      return (
        <LogicalOperatorIcon
          icon={item?.icon}
          value={item?.value}
        ></LogicalOperatorIcon>
      );
    }
    return <></>;
  }, []);

  return (
    <Card className="bg-bg-card border-transparent rounded-md">
      <CardContent className="p-0 divide-y divide-background-card">
        {items.map((x, idx) => (
          <div key={idx}>
            <section className="flex justify-between gap-2 items-center text-xs p-1">
              <div className="flex-1 truncate text-accent-primary">
                {getLabel(x?.cpn_id)}
              </div>
              <span>{renderOperatorIcon(x?.operator)}</span>
              <div className="flex-1 truncate">{x?.value}</div>
            </section>
          </div>
        ))}
      </CardContent>
    </Card>
  );
};

function InnerSwitchNode({ id, data, selected }: NodeProps<ISwitchNode>) {
  const { positions } = useBuildSwitchHandlePositions({ data, id });
  return (
    <ToolBar selected={selected} id={id} label={data.label} showRun={false}>
      <NodeWrapper selected={selected}>
        <LeftEndHandle></LeftEndHandle>
        <NodeHeader id={id} name={data.name} label={data.label}></NodeHeader>
        <section className="gap-2.5 flex flex-col">
          {positions.map((position, idx) => {
            return (
              <div key={idx}>
                <section className="flex flex-col text-xs">
                  <div className="text-right">
                    <span>{getConditionKey(idx, positions.length)}</span>
                    <div className="text-text-secondary">
                      {idx < positions.length - 1 && position.text}
                    </div>
                  </div>
                  <span className="text-accent-primary">
                    {idx < positions.length - 1 &&
                      position.condition?.logical_operator?.toUpperCase()}
                  </span>
                  {position.condition && (
                    <ConditionBlock
                      condition={position.condition}
                      nodeId={id}
                    ></ConditionBlock>
                  )}
                </section>
                <CommonHandle
                  key={position.text}
                  id={position.text}
                  type="source"
                  position={Position.Right}
                  isConnectable
                  style={{ ...RightHandleStyle, top: position.top }}
                  nodeId={id}
                  isConnectableEnd={false}
                ></CommonHandle>
              </div>
            );
          })}
        </section>
      </NodeWrapper>
    </ToolBar>
  );
}

export const SwitchNode = memo(InnerSwitchNode);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/canvas/node/switch-node.tsx` is located in the `web/src/pages/agent/canvas/node` directory.

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

- [agent-node.tsx](agent-node.tsx_docs.md)
- [begin-node.tsx](begin-node.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [categorize-node.tsx](categorize-node.tsx_docs.md)
- [data-operations-node.tsx](data-operations-node.tsx_docs.md)
- [dropdown.tsx](dropdown.tsx_docs.md)
- [email-node.tsx](email-node.tsx_docs.md)
- [extractor-node.tsx](extractor-node.tsx_docs.md)
- [file-node.tsx](file-node.tsx_docs.md)
- [handle-icon.tsx](handle-icon.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
