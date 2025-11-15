# File Documentation: web/src/pages/agent/canvas/node/template-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/template-node.tsx`
- **Extension**: `.tsx`
- **Lines**: 79
- **Characters**: 2,139
- **Size**: 2,139 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTheme } from '@/components/theme-provider';
import { Handle, NodeProps, Position } from '@xyflow/react';
import { Flex } from 'antd';
import classNames from 'classnames';
import { get } from 'lodash';
import { useGetComponentLabelByValue } from '../../hooks/use-get-begin-query';
import { IGenerateParameter } from '../../interface';
import { LeftHandleStyle, RightHandleStyle } from './handle-icon';
import NodeHeader from './node-header';

import { ITemplateNode } from '@/interfaces/database/flow';
import { memo } from 'react';
import styles from './index.less';

function InnerTemplateNode({
  id,
  data,
  isConnectable = true,
  selected,
}: NodeProps<ITemplateNode>) {
  const parameters: IGenerateParameter[] = get(data, 'form.parameters', []);
  const getLabel = useGetComponentLabelByValue(id);
  const { theme } = useTheme();
  return (
    <section
      className={classNames(
        styles.logicNode,
        theme === 'dark' ? styles.dark : '',

        {
          [styles.selectedNode]: selected,
        },
      )}
    >
      <Handle
        id="c"
        type="source"
        position={Position.Left}
        isConnectable={isConnectable}
        className={styles.handle}
        style={LeftHandleStyle}
      ></Handle>
      <Handle
        type="source"
        position={Position.Right}
        isConnectable={isConnectable}
        className={styles.handle}
        style={RightHandleStyle}
        id="b"
      ></Handle>

      <NodeHeader
        id={id}
        name={data.name}
        label={data.label}
        className={styles.nodeHeader}
      ></NodeHeader>

      <Flex gap={8} vertical className={styles.generateParameters}>
        {parameters.map((x) => (
          <Flex
            key={x.id}
            align="center"
            gap={6}
            className={styles.conditionBlock}
          >
            <label htmlFor="">{x.key}</label>
            <span className={styles.parameterValue}>
              {getLabel(x.component_id)}
            </span>
          </Flex>
        ))}
      </Flex>
    </section>
  );
}

export const TemplateNode = memo(InnerTemplateNode);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/template-node.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 79 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `TemplateNode`: Exported entity

### Functions (1)

- `InnerTemplateNode()`: Function definition

### Imports (12)

- `import { useTheme } from '@/components/theme-provider';`
- `import { Handle, NodeProps, Position } from '@xyflow/react';`
- `import { Flex } from 'antd';`
- `import classNames from 'classnames';`
- `import { get } from 'lodash';`
- `import { useGetComponentLabelByValue } from '../../hooks/use-get-begin-query';`
- `import { IGenerateParameter } from '../../interface';`
- `import { LeftHandleStyle, RightHandleStyle } from './handle-icon';`
- `import NodeHeader from './node-header';`
- `import { ITemplateNode } from '@/interfaces/database/flow';`

## Code Structure Analysis

- Total lines: 79
- Blank lines: 7 (8.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~72


## Dependencies and Imports

- `@/components/theme-provider`
- `@xyflow/react`
- `antd`
- `classnames`
- `lodash`
- `../../hooks/use-get-begin-query`
- `../../interface`
- `./handle-icon`
- `./node-header`
- `@/interfaces/database/flow`
- `react`
- `./index.less`

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
- Potential test file: `test_template-node.tsx`

## Keywords

../../hooks/use-get-begin-query, ../../interface, ./handle-icon, ./index.less, ./node-header, @/components/theme-provider, @/interfaces/database/flow, @xyflow/react, Flex, Handle, IGenerateParameter, ITemplateNode, InnerTemplateNode, Left, LeftHandleStyle, NodeHeader, NodeProps, Position, Right, RightHandleStyle, TemplateNode, TypeScript, antd, classnames, getLabel, lodash, parameters, react, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
