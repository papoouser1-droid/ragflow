# File Documentation: web/src/components/ui/tooltip.tsx

## File Metadata

- **Path**: `web/src/components/ui/tooltip.tsx`
- **Extension**: `.tsx`
- **Lines**: 149
- **Characters**: 4,368
- **Size**: 4,368 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import * as TooltipPrimitive from '@radix-ui/react-tooltip';
import * as React from 'react';

import { cn } from '@/lib/utils';
import { Info } from 'lucide-react';

const TooltipProvider = TooltipPrimitive.Provider;

const Tooltip = TooltipPrimitive.Root;

const TooltipTrigger = TooltipPrimitive.Trigger;

const TooltipContent = React.forwardRef<
  React.ElementRef<typeof TooltipPrimitive.Content>,
  React.ComponentPropsWithoutRef<typeof TooltipPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <TooltipPrimitive.Content
    ref={ref}
    sideOffset={sideOffset}
    className={cn(
      'z-50 overflow-auto scrollbar-auto rounded-md whitespace-pre-wrap border bg-popover px-3 py-1.5 text-sm text-popover-foreground shadow-md animate-in fade-in-0 zoom-in-95 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2 max-w-[30vw]',
      className,
    )}
    {...props}
  />
));
TooltipContent.displayName = TooltipPrimitive.Content.displayName;

export { Tooltip, TooltipContent, TooltipProvider, TooltipTrigger };

export const FormTooltip = ({ tooltip }: { tooltip: React.ReactNode }) => {
  return (
    <Tooltip>
      <TooltipTrigger
        tabIndex={-1}
        onClick={(e) => {
          e.preventDefault(); // Prevent clicking the tooltip from triggering form save
        }}
      >
        <Info className="size-3 ml-2" />
      </TooltipTrigger>
      <TooltipContent>{tooltip}</TooltipContent>
    </Tooltip>
  );
};

export function RAGFlowTooltip({
  children,
  tooltip,
}: React.PropsWithChildren & { tooltip: React.ReactNode }) {
  return (
    <Tooltip>
      <TooltipTrigger>{children}</TooltipTrigger>
      <TooltipContent>{tooltip}</TooltipContent>
    </Tooltip>
  );
}

export interface AntToolTipProps {
  title: React.ReactNode;
  children: React.ReactNode;
  placement?: 'top' | 'bottom' | 'left' | 'right';
  trigger?: 'hover' | 'click' | 'focus';
  className?: string;
}

export const AntToolTip: React.FC<AntToolTipProps> = ({
  title,
  children,
  placement = 'top',
  trigger = 'hover',
  className,
}) => {
  const [visible, setVisible] = React.useState(false);

  const showTooltip = () => {
    if (trigger === 'hover' || trigger === 'focus') {
      setVisible(true);
    }
  };

  const hideTooltip = () => {
    if (trigger === 'hover' || trigger === 'focus') {
      setVisible(false);
    }
  };

  const toggleTooltip = () => {
    if (trigger === 'click') {
      setVisible(!visible);
    }
  };

  const getPlacementClasses = () => {
    switch (placement) {
      case 'top':
        return 'bottom-full left-1/2 transform -translate-x-1/2 mb-2';
      case 'bottom':
        return 'top-full left-1/2 transform -translate-x-1/2 mt-2';
      case 'left':
        return 'right-full top-1/2 transform -translate-y-1/2 mr-2';
      case 'right':
        return 'left-full top-1/2 transform -translate-y-1/2 ml-2';
      default:
        return 'bottom-full left-1/2 transform -translate-x-1/2 mb-2';
    }
  };

  return (
    <div className="inline-block relative">
      <div
        onMouseEnter={showTooltip}
        onMouseLeave={hideTooltip}
        onClick={toggleTooltip}
        onFocus={showTooltip}
        onBlur={hideTooltip}
      >
        {children}
      </div>
      {visible && title && (
        <div
          className={cn(
            'absolute z-50 px-2.5 py-2 text-xs text-text-primary bg-muted rounded-sm shadow-sm whitespace-wrap w-max',
            getPlacementClasses(),
            className,
          )}
        >
          {title}
          <div
            className={cn(
              'absolute w-2 h-2  bg-muted ',
              placement === 'top' &&
                'bottom-[-4px] left-1/2 transform -translate-x-1/2 rotate-45',
              placement === 'bottom' &&
                'top-[-4px] left-1/2 transform -translate-x-1/2 rotate-45',
              placement === 'left' &&
                'right-[-4px] top-1/2 transform -translate-y-1/2 rotate-45',
              placement === 'right' &&
                'left-[-4px] top-1/2 transform -translate-y-1/2 rotate-45',
            )}
          />
        </div>
      )}
    </div>
  );
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/tooltip.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 149 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `FormTooltip`: Exported entity
- `RAGFlowTooltip`: Exported entity
- `AntToolTip`: Exported entity

### Functions (6)

- `FormTooltip()`: Function definition
- `RAGFlowTooltip()`: Function definition
- `showTooltip()`: Function definition
- `hideTooltip()`: Function definition
- `toggleTooltip()`: Function definition
- `getPlacementClasses()`: Function definition

### Imports (4)

- `import * as TooltipPrimitive from '@radix-ui/react-tooltip';`
- `import * as React from 'react';`
- `import { cn } from '@/lib/utils';`
- `import { Info } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 149
- Blank lines: 17 (11.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~132


## Dependencies and Imports

- `@radix-ui/react-tooltip`
- `react`
- `@/lib/utils`
- `lucide-react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui`.

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

- Other files in `web/src/components/ui/` directory
- Potential test file: `test_tooltip.tsx`

## Keywords

@/lib/utils, @radix-ui/react-tooltip, AntToolTip, AntToolTipProps, ComponentPropsWithoutRef, Content, ElementRef, FormTooltip, Info, Prevent, PropsWithChildren, Provider, RAGFlowTooltip, React, ReactNode, Root, Tooltip, TooltipContent, TooltipPrimitive, TooltipProvider, TooltipTrigger, Trigger, TypeScript, getPlacementClasses, hideTooltip, lucide-react, radix, react, showTooltip, toggleTooltip

---
*Generated by RAGFlow Repository Documentation Generator*
