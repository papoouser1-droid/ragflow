# Documentation: web/src/components/collapse.tsx

## File Metadata

- **Path**: `web/src/components/collapse.tsx`
- **Size**: 3383 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/collapse.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/collapse.tsx` is located in the `web/src/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [auto-keywords-form-field.tsx](auto-keywords-form-field.tsx_docs.md)
- [auto-keywords-item.tsx](auto-keywords-item.tsx_docs.md)
- [avatar-upload.tsx](avatar-upload.tsx_docs.md)
- [bulk-operate-bar.tsx](bulk-operate-bar.tsx_docs.md)
- [card-container.tsx](card-container.tsx_docs.md)
- [confirm-delete-dialog.tsx](confirm-delete-dialog.tsx_docs.md)
- [copy-to-clipboard.tsx](copy-to-clipboard.tsx_docs.md)
- [cross-language-form-field.tsx](cross-language-form-field.tsx_docs.md)
- [cross-language-item.tsx](cross-language-item.tsx_docs.md)
- [dataset-configuration-container.tsx](dataset-configuration-container.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
