# File Documentation: web/src/components/collapse.tsx

## File Metadata

- **Path**: `web/src/components/collapse.tsx`
- **Extension**: `.tsx`
- **Lines**: 131
- **Characters**: 3,383
- **Size**: 3,383 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible';
import { cn } from '@/lib/utils';
import { CollapsibleProps } from '@radix-ui/react-collapsible';
import {
  ChevronDown,
  ChevronUp,
  ListChevronsDownUp,
  ListChevronsUpDown,
} from 'lucide-react';
import * as React from 'react';
import {
  PropsWithChildren,
  ReactNode,
  useCallback,
  useEffect,
  useState,
} from 'react';

type CollapseProps = Omit<CollapsibleProps, 'title'> & {
  title?: ReactNode;
  rightContent?: ReactNode;
} & PropsWithChildren;

export function Collapse({
  title,
  children,
  rightContent,
  open = true,
  defaultOpen = false,
  onOpenChange,
  disabled,
}: CollapseProps) {
  const [currentOpen, setCurrentOpen] = useState(open);

  useEffect(() => {
    setCurrentOpen(open);
  }, [open]);

  const handleOpenChange = useCallback(
    (open: boolean) => {
      setCurrentOpen(open);
      onOpenChange?.(open);
    },
    [onOpenChange],
  );

  return (
    <Collapsible
      defaultOpen={defaultOpen}
      open={currentOpen}
      onOpenChange={handleOpenChange}
      disabled={disabled}
    >
      <CollapsibleTrigger className={'w-full'}>
        <section className="flex justify-between items-center">
          <div className="flex items-center gap-1">
            {currentOpen ? (
              <ListChevronsUpDown className="size-4" />
            ) : (
              <ListChevronsDownUp className="size-4 text-text-secondary" />
            )}
            <div
              className={cn('text-text-secondary', {
                'text-text-primary': open,
              })}
            >
              {title}
            </div>
          </div>
          <div>{rightContent}</div>
        </section>
      </CollapsibleTrigger>
      <CollapsibleContent className="pt-5">{children}</CollapsibleContent>
    </Collapsible>
  );
}

export type NodeCollapsibleProps<T extends any[]> = {
  items?: T;
  children: (item: T[0], idx: number) => ReactNode;
  className?: string;
};
export function NodeCollapsible<T extends any[]>({
  items = [] as unknown as T,
  children,
  className,
}: NodeCollapsibleProps<T>) {
  const [isOpen, setIsOpen] = React.useState(false);

  const nextClassName = cn('space-y-2', className);

  const nextItems = items.every((x) => Array.isArray(x)) ? items.flat() : items;

  return (
    <Collapsible
      open={isOpen}
      onOpenChange={setIsOpen}
      className={cn('relative', nextClassName)}
    >
      {nextItems.slice(0, 3).map(children)}
      <CollapsibleContent className={nextClassName}>
        {nextItems.slice(3).map((x, idx) => children(x, idx + 3))}
      </CollapsibleContent>
      {nextItems.length > 3 && (
        <CollapsibleTrigger
          asChild
          onClick={(e) => e.stopPropagation()}
          className="absolute left-1/2 -translate-x-1/2 bottom-0 translate-y-1/2 cursor-pointer"
        >
          <div
            className={cn(
              'size-3 bg-text-secondary rounded-full flex items-center justify-center',
              { 'bg-text-primary': isOpen },
            )}
          >
            {isOpen ? (
              <ChevronUp className="stroke-bg-component" />
            ) : (
              <ChevronDown className="stroke-bg-component" />
            )}
          </div>
        </CollapsibleTrigger>
      )}
    </Collapsible>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/collapse.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 131 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `Collapse`: Exported entity
- `NodeCollapsible`: Exported entity

### Functions (4)

- `Collapse()`: Function definition
- `handleOpenChange()`: Function definition
- `NodeCollapsible()`: Function definition
- `nextItems()`: Function definition

### Imports (6)

- `import {`
- `import { cn } from '@/lib/utils';`
- `import { CollapsibleProps } from '@radix-ui/react-collapsible';`
- `import {`
- `import * as React from 'react';`
- `import {`

## Code Structure Analysis

- Total lines: 131
- Blank lines: 10 (7.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~121


## Dependencies and Imports

- `@/lib/utils`
- `@radix-ui/react-collapsible`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components`.

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

- Other files in `web/src/components/` directory
- Potential test file: `test_collapse.tsx`

## Keywords

@/lib/utils, @radix-ui/react-collapsible, Array, ChevronDown, ChevronUp, Collapse, CollapseProps, Collapsible, CollapsibleContent, CollapsibleProps, CollapsibleTrigger, ListChevronsDownUp, ListChevronsUpDown, NodeCollapsible, NodeCollapsibleProps, Omit, PropsWithChildren, React, ReactNode, TypeScript, handleOpenChange, nextClassName, nextItems, radix, react

---
*Generated by RAGFlow Repository Documentation Generator*
