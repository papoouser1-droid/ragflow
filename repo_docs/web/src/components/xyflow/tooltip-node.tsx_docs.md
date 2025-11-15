# File Documentation: web/src/components/xyflow/tooltip-node.tsx

## File Metadata

- **Path**: `web/src/components/xyflow/tooltip-node.tsx`
- **Extension**: `.tsx`
- **Lines**: 103
- **Characters**: 2,675
- **Size**: 2,675 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { NodeProps, NodeToolbar, NodeToolbarProps } from '@xyflow/react';
import {
  HTMLAttributes,
  ReactNode,
  createContext,
  forwardRef,
  useCallback,
  useContext,
  useState,
} from 'react';
import { BaseNode } from './base-node';

/* TOOLTIP CONTEXT ---------------------------------------------------------- */

const TooltipContext = createContext(false);

/* TOOLTIP NODE ------------------------------------------------------------- */

export type TooltipNodeProps = Partial<NodeProps> & {
  children?: ReactNode;
};

/**
 * A component that wraps a node and provides tooltip visibility context.
 */
export const TooltipNode = forwardRef<HTMLDivElement, TooltipNodeProps>(
  ({ selected, children }, ref) => {
    const [isTooltipVisible, setTooltipVisible] = useState(false);

    const showTooltip = useCallback(() => setTooltipVisible(true), []);
    const hideTooltip = useCallback(() => setTooltipVisible(false), []);

    return (
      <TooltipContext.Provider value={isTooltipVisible}>
        <BaseNode
          ref={ref}
          onMouseEnter={showTooltip}
          onMouseLeave={hideTooltip}
          onFocus={showTooltip}
          onBlur={hideTooltip}
          tabIndex={0}
          selected={selected}
          className="h-full bg-transparent"
        >
          {children}
        </BaseNode>
      </TooltipContext.Provider>
    );
  },
);

TooltipNode.displayName = 'TooltipNode';

/* TOOLTIP CONTENT ---------------------------------------------------------- */

export type TooltipContentProps = NodeToolbarProps;

/**
 * A component that displays the tooltip content based on visibility context.
 */
export const TooltipContent = forwardRef<HTMLDivElement, TooltipContentProps>(
  ({ position, children }, ref) => {
    const isTooltipVisible = useContext(TooltipContext);

    return (
      <div ref={ref}>
        <NodeToolbar
          isVisible={isTooltipVisible}
          className=" bg-transparent  text-primary-foreground"
          tabIndex={1}
          position={position}
          offset={0}
          align={'end'}
        >
          {children}
        </NodeToolbar>
      </div>
    );
  },
);

TooltipContent.displayName = 'TooltipContent';

/* TOOLTIP TRIGGER ---------------------------------------------------------- */

export type TooltipTriggerProps = HTMLAttributes<HTMLParagraphElement>;

/**
 * A component that triggers the tooltip visibility.
 */
export const TooltipTrigger = forwardRef<
  HTMLParagraphElement,
  TooltipTriggerProps
>(({ children, ...props }, ref) => {
  return (
    <div ref={ref} {...props}>
      {children}
    </div>
  );
});

TooltipTrigger.displayName = 'TooltipTrigger';

```

## High-Level Overview

/* TOOLTIP CONTEXT ---------------------------------------------------------- */

## Detailed Walkthrough

### Exports (3)

- `TooltipNode`: Exported entity
- `TooltipContent`: Exported entity
- `TooltipTrigger`: Exported entity

### Functions (5)

- `TooltipNode()`: Function definition
- `showTooltip()`: Function definition
- `hideTooltip()`: Function definition
- `TooltipContent()`: Function definition
- `TooltipTrigger()`: Function definition

### Imports (3)

- `import { NodeProps, NodeToolbar, NodeToolbarProps } from '@xyflow/react';`
- `import {`
- `import { BaseNode } from './base-node';`

## Code Structure Analysis

- Total lines: 103
- Blank lines: 18 (17.5%)
- Comment lines: ~13 (12.6%)
- Code lines: ~72


## Dependencies and Imports

- `@xyflow/react`
- `./base-node`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/xyflow`.

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

- Other files in `web/src/components/xyflow/` directory
- Potential test file: `test_tooltip-node.tsx`

## Keywords

./base-node, @xyflow/react, BaseNode, CONTENT, CONTEXT, HTMLAttributes, HTMLDivElement, HTMLParagraphElement, NODE, NodeProps, NodeToolbar, NodeToolbarProps, Partial, Provider, ReactNode, TOOLTIP, TRIGGER, TooltipContent, TooltipContentProps, TooltipContext, TooltipNode, TooltipNodeProps, TooltipTrigger, TooltipTriggerProps, TypeScript, hideTooltip, isTooltipVisible, showTooltip, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
