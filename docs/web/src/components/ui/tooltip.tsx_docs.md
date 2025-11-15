# Documentation: web/src/components/ui/tooltip.tsx

## File Metadata

- **Path**: `web/src/components/ui/tooltip.tsx`
- **Size**: 4368 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/tooltip.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/tooltip.tsx` is located in the `web/src/components/ui` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to ui.

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

- [accordion.tsx](accordion.tsx_docs.md)
- [alert-dialog.tsx](alert-dialog.tsx_docs.md)
- [aspect-ratio.tsx](aspect-ratio.tsx_docs.md)
- [async-tree-select.tsx](async-tree-select.tsx_docs.md)
- [avatar.tsx](avatar.tsx_docs.md)
- [badge.tsx](badge.tsx_docs.md)
- [breadcrumb.tsx](breadcrumb.tsx_docs.md)
- [button.tsx](button.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [checkbox.tsx](checkbox.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
