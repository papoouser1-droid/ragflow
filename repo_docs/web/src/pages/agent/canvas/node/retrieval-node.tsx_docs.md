# File Documentation: web/src/pages/agent/canvas/node/retrieval-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/retrieval-node.tsx`
- **Extension**: `.tsx`
- **Lines**: 74
- **Characters**: 2,461
- **Size**: 2,461 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { NodeCollapsible } from '@/components/collapse';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { useFetchKnowledgeList } from '@/hooks/knowledge-hooks';
import { IRetrievalNode } from '@/interfaces/database/flow';
import { NodeProps, Position } from '@xyflow/react';
import classNames from 'classnames';
import { get } from 'lodash';
import { memo } from 'react';
import { NodeHandleId } from '../../constant';
import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';
import { CommonHandle, LeftEndHandle } from './handle';
import styles from './index.less';
import NodeHeader from './node-header';
import { NodeWrapper } from './node-wrapper';
import { ToolBar } from './toolbar';

function InnerRetrievalNode({
  id,
  data,
  isConnectable = true,
  selected,
}: NodeProps<IRetrievalNode>) {
  const knowledgeBaseIds: string[] = get(data, 'form.kb_ids', []);
  const { list: knowledgeList } = useFetchKnowledgeList(true);

  const { getLabel } = useGetVariableLabelOrTypeByValue(id);

  return (
    <ToolBar selected={selected} id={id} label={data.label}>
      <NodeWrapper selected={selected}>
        <LeftEndHandle></LeftEndHandle>
        <CommonHandle
          id={NodeHandleId.Start}
          type="source"
          position={Position.Right}
          isConnectable={isConnectable}
          nodeId={id}
          isConnectableEnd={false}
        ></CommonHandle>
        <NodeHeader
          id={id}
          name={data.name}
          label={data.label}
          className={classNames({
            [styles.nodeHeader]: knowledgeBaseIds.length > 0,
          })}
        ></NodeHeader>
        <NodeCollapsible items={knowledgeBaseIds}>
          {(id) => {
            const item = knowledgeList.find((y) => id === y.id);
            const label = getLabel(id);

            return (
              <div className={styles.nodeText} key={id}>
                <div className="flex items-center gap-1.5">
                  <RAGFlowAvatar
                    className="size-6 rounded-lg"
                    avatar={id}
                    name={item?.name || (label as string) || 'CN'}
                  />

                  <div className={'truncate flex-1'}>{label || item?.name}</div>
                </div>
              </div>
            );
          }}
        </NodeCollapsible>
      </NodeWrapper>
    </ToolBar>
  );
}

export const RetrievalNode = memo(InnerRetrievalNode);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/retrieval-node.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 74 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `RetrievalNode`: Exported entity

### Functions (2)

- `InnerRetrievalNode()`: Function definition
- `item()`: Function definition

### Imports (15)

- `import { NodeCollapsible } from '@/components/collapse';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { useFetchKnowledgeList } from '@/hooks/knowledge-hooks';`
- `import { IRetrievalNode } from '@/interfaces/database/flow';`
- `import { NodeProps, Position } from '@xyflow/react';`
- `import classNames from 'classnames';`
- `import { get } from 'lodash';`
- `import { memo } from 'react';`
- `import { NodeHandleId } from '../../constant';`
- `import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';`

## Code Structure Analysis

- Total lines: 74
- Blank lines: 7 (9.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~67


## Dependencies and Imports

- `@/components/collapse`
- `@/components/ragflow-avatar`
- `@/hooks/knowledge-hooks`
- `@/interfaces/database/flow`
- `@xyflow/react`
- `classnames`
- `lodash`
- `react`
- `../../constant`
- `../../hooks/use-get-begin-query`
- `./handle`
- `./index.less`
- `./node-header`
- `./node-wrapper`
- `./toolbar`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/canvas/node/` directory
- Potential test file: `test_retrieval-node.tsx`

## Keywords

../../constant, ../../hooks/use-get-begin-query, ./handle, ./index.less, ./node-header, ./node-wrapper, ./toolbar, @/components/collapse, @/components/ragflow-avatar, @/hooks/knowledge-hooks, @/interfaces/database/flow, @xyflow/react, CommonHandle, IRetrievalNode, InnerRetrievalNode, LeftEndHandle, NodeCollapsible, NodeHandleId, NodeHeader, NodeProps, NodeWrapper, Position, RAGFlowAvatar, RetrievalNode, Right, Start, ToolBar, TypeScript, classnames, item, knowledgeBaseIds, label, lodash, react, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
