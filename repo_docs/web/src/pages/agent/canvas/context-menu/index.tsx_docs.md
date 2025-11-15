# File Documentation: web/src/pages/agent/canvas/context-menu/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/context-menu/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 108
- **Characters**: 2,911
- **Size**: 2,911 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { NodeMouseHandler, useReactFlow } from '@xyflow/react';
import { useCallback, useRef, useState } from 'react';

import styles from './index.less';

export interface INodeContextMenu {
  id: string;
  top: number;
  left: number;
  right?: number;
  bottom?: number;
  [key: string]: unknown;
}

export function NodeContextMenu({
  id,
  top,
  left,
  right,
  bottom,
  ...props
}: INodeContextMenu) {
  const { getNode, setNodes, addNodes, setEdges } = useReactFlow();

  const duplicateNode = useCallback(() => {
    const node = getNode(id);
    const position = {
      x: node?.position?.x || 0 + 50,
      y: node?.position?.y || 0 + 50,
    };

    addNodes({
      ...(node || {}),
      data: node?.data,
      selected: false,
      dragging: false,
      id: `${node?.id}-copy`,
      position,
    });
  }, [id, getNode, addNodes]);

  const deleteNode = useCallback(() => {
    setNodes((nodes) => nodes.filter((node) => node.id !== id));
    setEdges((edges) => edges.filter((edge) => edge.source !== id));
  }, [id, setNodes, setEdges]);

  return (
    <div
      style={{ top, left, right, bottom }}
      className={styles.contextMenu}
      {...props}
    >
      <p style={{ margin: '0.5em' }}>
        <small>node: {id}</small>
      </p>
      <button onClick={duplicateNode} type={'button'}>
        duplicate
      </button>
      <button onClick={deleteNode} type={'button'}>
        delete
      </button>
    </div>
  );
}

/*  @deprecated
 */
export const useHandleNodeContextMenu = (sideWidth: number) => {
  const [menu, setMenu] = useState<INodeContextMenu>({} as INodeContextMenu);
  const ref = useRef<any>(null);

  const onNodeContextMenu: NodeMouseHandler = useCallback(
    (event, node) => {
      // Prevent native context menu from showing
      event.preventDefault();

      // Calculate position of the context menu. We want to make sure it
      // doesn't get positioned off-screen.
      const pane = ref.current?.getBoundingClientRect();
      // setMenu({
      //   id: node.id,
      //   top: event.clientY < pane.height - 200 ? event.clientY : 0,
      //   left: event.clientX < pane.width - 200 ? event.clientX : 0,
      //   right: event.clientX >= pane.width - 200 ? pane.width - event.clientX : 0,
      //   bottom:
      //     event.clientY >= pane.height - 200 ? pane.height - event.clientY : 0,
      // });

      setMenu({
        id: node.id,
        top: event.clientY - 144,
        left: event.clientX - sideWidth,
        // top: event.clientY < pane.height - 200 ? event.clientY - 72 : 0,
        // left: event.clientX < pane.width - 200 ? event.clientX : 0,
      });
    },
    [sideWidth],
  );

  // Close the context menu if it's open whenever the window is clicked.
  const onPaneClick = useCallback(
    () => setMenu({} as INodeContextMenu),
    [setMenu],
  );

  return { onNodeContextMenu, menu, onPaneClick, ref };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/context-menu/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 108 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `NodeContextMenu`: Exported entity
- `useHandleNodeContextMenu`: Exported entity

### Functions (5)

- `NodeContextMenu()`: Function definition
- `duplicateNode()`: Function definition
- `deleteNode()`: Function definition
- `useHandleNodeContextMenu()`: Function definition
- `onPaneClick()`: Function definition

### Imports (3)

- `import { NodeMouseHandler, useReactFlow } from '@xyflow/react';`
- `import { useCallback, useRef, useState } from 'react';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 108
- Blank lines: 14 (13.0%)
- Comment lines: ~16 (14.8%)
- Code lines: ~78


## Dependencies and Imports

- `@xyflow/react`
- `react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/context-menu`.

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

- Other files in `web/src/pages/agent/canvas/context-menu/` directory
- Potential test file: `test_index.tsx`

## Keywords

./index.less, @xyflow/react, Calculate, Close, INodeContextMenu, NodeContextMenu, NodeMouseHandler, Prevent, TypeScript, deleteNode, deprecated, duplicateNode, node, onNodeContextMenu, onPaneClick, pane, position, react, ref, useHandleNodeContextMenu, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
