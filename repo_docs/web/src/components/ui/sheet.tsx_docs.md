# File Documentation: web/src/components/ui/sheet.tsx

## File Metadata

- **Path**: `web/src/components/ui/sheet.tsx`
- **Extension**: `.tsx`
- **Lines**: 150
- **Characters**: 4,477
- **Size**: 4,477 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import * as SheetPrimitive from '@radix-ui/react-dialog';
import { cva, type VariantProps } from 'class-variance-authority';
import { X } from 'lucide-react';
import * as React from 'react';

import { cn } from '@/lib/utils';

const Sheet = SheetPrimitive.Root;

const SheetTrigger = SheetPrimitive.Trigger;

const SheetClose = SheetPrimitive.Close;

const SheetPortal = SheetPrimitive.Portal;

const SheetOverlay = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Overlay
    className={cn(
      'fixed inset-0 z-50 bg-black/80 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className,
    )}
    {...props}
    ref={ref}
  />
));
SheetOverlay.displayName = SheetPrimitive.Overlay.displayName;

const sheetVariants = cva(
  'fixed z-50 gap-4 bg-bg-base rounded-lg p-6 shadow-lg transition ease-in-out data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:duration-300 data-[state=open]:duration-500',
  {
    variants: {
      side: {
        top: 'inset-x-0 top-0 border-b data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top',
        bottom:
          'inset-x-0 bottom-0 border-t data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom',
        left: 'inset-y-0 left-0 h-full w-3/4 border-r data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left sm:max-w-sm',
        right:
          'inset-y-0 right-0 h-full   border-l data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right w-1/3 max-w-[600px]',
      },
    },
    defaultVariants: {
      side: 'right',
    },
  },
);

interface SheetContentProps
  extends React.ComponentPropsWithoutRef<typeof SheetPrimitive.Content>,
    VariantProps<typeof sheetVariants> {
  closeIcon?: boolean;
}

const SheetContent = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Content>,
  SheetContentProps
>(
  (
    { side = 'right', className, children, closeIcon = true, ...props },
    ref,
  ) => (
    <SheetPortal>
      <SheetOverlay />
      <SheetPrimitive.Content
        ref={ref}
        className={cn(sheetVariants({ side }), className)}
        {...props}
      >
        {children}
        {closeIcon ? (
          <SheetPrimitive.Close className="absolute right-4 top-4 rounded-sm opacity-70 ring-offset-background transition-opacity hover:opacity-100 focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:pointer-events-none data-[state=open]:bg-secondary ">
            <X className="h-4 w-4" />
            <span className="sr-only">Close</span>
          </SheetPrimitive.Close>
        ) : null}
      </SheetPrimitive.Content>
    </SheetPortal>
  ),
);
SheetContent.displayName = SheetPrimitive.Content.displayName || 'SheetContent';

const SheetHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col space-y-2 text-center sm:text-left',
      className,
    )}
    {...props}
  />
);
SheetHeader.displayName = 'SheetHeader';

const SheetFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      'flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2',
      className,
    )}
    {...props}
  />
);
SheetFooter.displayName = 'SheetFooter';

const SheetTitle = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Title>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Title
    ref={ref}
    className={cn('text-lg font-semibold text-foreground', className)}
    {...props}
  />
));
SheetTitle.displayName = SheetPrimitive.Title.displayName;

const SheetDescription = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Description>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Description
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
));
SheetDescription.displayName = SheetPrimitive.Description.displayName;

export {
  Sheet,
  SheetClose,
  SheetContent,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetOverlay,
  SheetPortal,
  SheetTitle,
  SheetTrigger,
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/sheet.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 150 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `SheetOverlay()`: Function definition
- `SheetHeader()`: Function definition
- `SheetFooter()`: Function definition
- `SheetTitle()`: Function definition
- `SheetDescription()`: Function definition

### Imports (5)

- `import * as SheetPrimitive from '@radix-ui/react-dialog';`
- `import { cva, type VariantProps } from 'class-variance-authority';`
- `import { X } from 'lucide-react';`
- `import * as React from 'react';`
- `import { cn } from '@/lib/utils';`

## Code Structure Analysis

- Total lines: 150
- Blank lines: 16 (10.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~134


## Dependencies and Imports

- `@radix-ui/react-dialog`
- `class-variance-authority`
- `lucide-react`
- `react`
- `@/lib/utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/ui/` directory
- Potential test file: `test_sheet.tsx`

## Keywords

@/lib/utils, @radix-ui/react-dialog, Close, ComponentPropsWithoutRef, Content, Description, ElementRef, HTMLAttributes, HTMLDivElement, Overlay, Portal, React, Root, Sheet, SheetClose, SheetContent, SheetContentProps, SheetDescription, SheetFooter, SheetHeader, SheetOverlay, SheetPortal, SheetPrimitive, SheetTitle, SheetTrigger, Title, Trigger, TypeScript, VariantProps, class-variance-authority, lucide-react, radix, react, sheetVariants

---
*Generated by RAGFlow Repository Documentation Generator*
