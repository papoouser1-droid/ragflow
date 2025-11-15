# File Documentation: web/src/pages/agent/canvas/node/email-node.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/email-node.tsx`
- **Extension**: `.tsx`
- **Lines**: 81
- **Characters**: 2,472
- **Size**: 2,476 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IEmailNode } from '@/interfaces/database/flow';
import { Handle, NodeProps, Position } from '@xyflow/react';
import { Flex } from 'antd';
import classNames from 'classnames';
import { memo, useState } from 'react';
import { LeftHandleStyle, RightHandleStyle } from './handle-icon';
import styles from './index.less';
import NodeHeader from './node-header';

export function InnerEmailNode({
  id,
  data,
  isConnectable = true,
  selected,
}: NodeProps<IEmailNode>) {
  const [showDetails, setShowDetails] = useState(false);

  return (
    <section
      className={classNames(styles.ragNode, {
        [styles.selectedNode]: selected,
      })}
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
      <NodeHeader id={id} name={data.name} label={data.label}></NodeHeader>

      <Flex vertical gap={8} className={styles.emailNodeContainer}>
        <div
          className={styles.emailConfig}
          onClick={() => setShowDetails(!showDetails)}
        >
          <div className={styles.configItem}>
            <span className={styles.configLabel}>SMTP:</span>
            <span className={styles.configValue}>{data.form?.smtp_server}</span>
          </div>
          <div className={styles.configItem}>
            <span className={styles.configLabel}>Port:</span>
            <span className={styles.configValue}>{data.form?.smtp_port}</span>
          </div>
          <div className={styles.configItem}>
            <span className={styles.configLabel}>From:</span>
            <span className={styles.configValue}>{data.form?.email}</span>
          </div>
          <div className={styles.expandIcon}>{showDetails ? '▼' : '▶'}</div>
        </div>

        {showDetails && (
          <div className={styles.jsonExample}>
            <div className={styles.jsonTitle}>Expected Input JSON:</div>
            <pre className={styles.jsonContent}>
              {`{
  "to_email": "...",
  "cc_email": "...", 
  "subject": "...",
  "content": "..."
}`}
            </pre>
          </div>
        )}
      </Flex>
    </section>
  );
}

export const EmailNode = memo(InnerEmailNode);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/email-node.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 81 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `InnerEmailNode`: Exported entity
- `EmailNode`: Exported entity

### Functions (1)

- `InnerEmailNode()`: Function definition

### Imports (8)

- `import { IEmailNode } from '@/interfaces/database/flow';`
- `import { Handle, NodeProps, Position } from '@xyflow/react';`
- `import { Flex } from 'antd';`
- `import classNames from 'classnames';`
- `import { memo, useState } from 'react';`
- `import { LeftHandleStyle, RightHandleStyle } from './handle-icon';`
- `import styles from './index.less';`
- `import NodeHeader from './node-header';`

## Code Structure Analysis

- Total lines: 81
- Blank lines: 6 (7.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~75


## Dependencies and Imports

- `@/interfaces/database/flow`
- `@xyflow/react`
- `antd`
- `classnames`
- `react`
- `./handle-icon`
- `./index.less`
- `./node-header`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/canvas/node/` directory
- Potential test file: `test_email-node.tsx`

## Keywords

./handle-icon, ./index.less, ./node-header, @/interfaces/database/flow, @xyflow/react, EmailNode, Expected, Flex, From, Handle, IEmailNode, InnerEmailNode, Input, JSON, Left, LeftHandleStyle, NodeHeader, NodeProps, Port, Position, Right, RightHandleStyle, SMTP, TypeScript, antd, classnames, react, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
