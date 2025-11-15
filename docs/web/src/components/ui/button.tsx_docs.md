# Documentation: web/src/components/ui/button.tsx

## File Metadata

- **Path**: `web/src/components/ui/button.tsx`
- **Size**: 4061 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/button.tsx`.

## Original Source Code

```tsx
import { Slot } from '@radix-ui/react-slot';
import { cva, type VariantProps } from 'class-variance-authority';
import * as React from 'react';

import { cn } from '@/lib/utils';
import { LucideLoader2, Plus } from 'lucide-react';

const buttonVariants = cva(
  cn(
    'inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-colors outline-0',
    'disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg:not([class*="size-"])]:size-4 shrink-0 [&_svg]:shrink-0',
  ),
  {
    variants: {
      variant: {
        default:
          'bg-text-primary text-bg-base shadow-xs hover:bg-text-primary/90 focus-visible:bg-text-primary/90',

        destructive: `
          bg-state-error text-white shadow-xs
          hover:bg-state-error/90 focus-visible:ring-state-error/20 dark:focus-visible:ring-state-error/40
        `,
        outline: `
          text-text-secondary bg-bg-input border-0.5 border-border-button
          hover:text-text-primary hover:bg-border-button hover:border-border-default
          focus-visible:text-text-primary focus-visible:bg-border-button focus-visible:border-border-button
        `,
        secondary:
          'bg-bg-input text-text-primary shadow-xs hover:bg-bg-input/80 border border-border-button',

        ghost: `
          text-text-secondary
          hover:bg-border-button hover:text-text-primary
          focus-visible:text-text-primary focus-visible:bg-border-button
        `,

        link: 'text-primary underline-offset-4 hover:underline',
        icon: 'bg-colors-background-inverse-standard text-foreground hover:bg-colors-background-inverse-standard/80',
        dashed: 'border border-dashed border-input hover:bg-accent',

        transparent: `
          text-text-secondary bg-transparent border-0.5 border-border-button
          hover:text-text-primary hover:bg-border-button
          focus-visible:text-text-primary focus-visible:bg-border-button focus-visible:border-border-button
        `,

        danger: `
          bg-transparent border border-state-error text-state-error
          hover:bg-state-error/10 focus-visible:bg-state-error/10
        `,

        highlighted: `
          bg-text-primary text-bg-base border-b-4 border-b-accent-primary
          hover:bg-text-primary/90 focus-visible:bg-text-primary/90
        `,
      },
      size: {
        default: 'h-8 px-2.5 py-1.5 ',
        sm: 'h-6 rounded-sm px-2',
        lg: 'h-11 rounded-md px-8',
        icon: 'h-10 w-10',
        auto: 'h-full px-1',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  },
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
  loading?: boolean;
  block?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  (
    {
      children,
      className,
      variant,
      size,
      asChild = false,
      loading = false,
      disabled = false,
      block = false,
      ...props
    },
    ref,
  ) => {
    const Comp = asChild ? Slot : 'button';

    return (
      <Comp
        className={cn(
          'bg-bg-card',
          { 'block w-full': block },
          buttonVariants({ variant, size, className }),
        )}
        ref={ref}
        disabled={loading || disabled}
        {...props}
      >
        {loading && <LucideLoader2 className="animate-spin" />}
        {children}
      </Comp>
    );
  },
);

Button.displayName = 'Button';

export const ButtonLoading = Button;

ButtonLoading.displayName = 'ButtonLoading';

export { Button, buttonVariants };

export const BlockButton = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ children, className, ...props }, ref) => {
    return (
      <Button
        variant={'outline'}
        ref={ref}
        className={cn('w-full border-dashed border-input-border', className)}
        {...props}
      >
        <Plus /> {children}
      </Button>
    );
  },
);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/button.tsx` is located in the `web/src/components/ui` directory.

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
- [card.tsx](card.tsx_docs.md)
- [checkbox.tsx](checkbox.tsx_docs.md)
- [collapsible.tsx](collapsible.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
