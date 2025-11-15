# File Documentation: web/src/components/ui/modal/modal-manage.tsx

## File Metadata

- **Path**: `web/src/components/ui/modal/modal-manage.tsx`
- **Extension**: `.tsx`
- **Lines**: 103
- **Characters**: 2,418
- **Size**: 2,418 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ReactNode, useEffect, useState } from 'react';
import { createPortal } from 'react-dom';
import { createRoot } from 'react-dom/client';
import { Modal, ModalProps } from './modal';

type PortalModalProps = Omit<ModalProps, 'open' | 'onOpenChange'> & {
  visible: boolean;
  onVisibleChange: (visible: boolean) => void;
  container?: HTMLElement;
  children: ReactNode;
  [key: string]: any;
};

const PortalModal = ({
  visible,
  onVisibleChange,
  container,
  children,
  ...restProps
}: PortalModalProps) => {
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
    return () => setMounted(false);
  }, []);

  if (!mounted || !visible) return null;
  console.log('PortalModal:', visible);
  return createPortal(
    <Modal open={visible} onOpenChange={onVisibleChange} {...restProps}>
      {children}
    </Modal>,
    container || document.body,
  );
};

export const createPortalModal = () => {
  let container = document.createElement('div');
  document.body.appendChild(container);

  let currentProps: any = {};
  let isVisible = false;
  let root: ReturnType<typeof createRoot> | null = null;

  root = createRoot(container);
  const destroy = () => {
    if (root && container) {
      root.unmount();
      if (container.parentNode) {
        container.parentNode.removeChild(container);
      }
      root = null;
    }
    isVisible = false;
    currentProps = {};
  };
  const render = () => {
    const { onVisibleChange, ...props } = currentProps;
    const modalParam = {
      visible: isVisible,

      onVisibleChange: (visible: boolean) => {
        isVisible = visible;
        if (onVisibleChange) {
          onVisibleChange(visible);
        }

        if (!visible) {
          render();
        }
      },
      ...props,
    };
    root?.render(isVisible ? <PortalModal {...modalParam} /> : null);
  };

  const show = (props: PortalModalProps) => {
    if (!container) {
      container = document.createElement('div');
      document.body.appendChild(container);
    }
    if (!root) {
      root = createRoot(container);
    }
    currentProps = { ...currentProps, ...props };
    isVisible = true;
    render();
  };

  const hide = () => {
    isVisible = false;
    render();
  };

  const update = (props = {}) => {
    currentProps = { ...currentProps, ...props };
    render();
  };

  return { show, hide, update, destroy };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/modal/modal-manage.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 103 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `createPortalModal`: Exported entity

### Functions (8)

- `PortalModal()`: Function definition
- `createPortalModal()`: Function definition
- `destroy()`: Function definition
- `render()`: Function definition
- `modalParam()`: Function definition
- `show()`: Function definition
- `hide()`: Function definition
- `update()`: Function definition

### Imports (4)

- `import { ReactNode, useEffect, useState } from 'react';`
- `import { createPortal } from 'react-dom';`
- `import { createRoot } from 'react-dom/client';`
- `import { Modal, ModalProps } from './modal';`

## Code Structure Analysis

- Total lines: 103
- Blank lines: 14 (13.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~89


## Dependencies and Imports

- `react`
- `react-dom`
- `react-dom/client`
- `./modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui/modal`.

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

- Other files in `web/src/components/ui/modal/` directory
- Potential test file: `test_modal-manage.tsx`

## Keywords

./modal, HTMLElement, Modal, ModalProps, Omit, PortalModal, PortalModalProps, ReactNode, ReturnType, TypeScript, container, createPortalModal, currentProps, destroy, hide, isVisible, modalParam, react, react-dom, react-dom/client, render, root, show, update

---
*Generated by RAGFlow Repository Documentation Generator*
