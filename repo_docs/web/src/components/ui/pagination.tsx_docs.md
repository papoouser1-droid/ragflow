# File Documentation: web/src/components/ui/pagination.tsx

## File Metadata

- **Path**: `web/src/components/ui/pagination.tsx`
- **Extension**: `.tsx`
- **Lines**: 118
- **Characters**: 2,744
- **Size**: 2,744 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ChevronLeft, ChevronRight, MoreHorizontal } from 'lucide-react';
import * as React from 'react';

import { ButtonProps, buttonVariants } from '@/components/ui/button';
import { cn } from '@/lib/utils';

const Pagination = ({ className, ...props }: React.ComponentProps<'nav'>) => (
  <nav
    role="navigation"
    aria-label="pagination"
    className={cn('mx-auto flex w-full justify-center', className)}
    {...props}
  />
);
Pagination.displayName = 'Pagination';

const PaginationContent = React.forwardRef<
  HTMLUListElement,
  React.ComponentProps<'ul'>
>(({ className, ...props }, ref) => (
  <ul
    ref={ref}
    className={cn('flex flex-row items-center gap-1', className)}
    {...props}
  />
));
PaginationContent.displayName = 'PaginationContent';

const PaginationItem = React.forwardRef<
  HTMLLIElement,
  React.ComponentProps<'li'>
>(({ className, ...props }, ref) => (
  <li ref={ref} className={cn('', className)} {...props} />
));
PaginationItem.displayName = 'PaginationItem';

type PaginationLinkProps = {
  isActive?: boolean;
} & Pick<ButtonProps, 'size'> &
  React.ComponentProps<'a'>;

const PaginationLink = ({
  className,
  isActive,
  size = 'icon',
  ...props
}: PaginationLinkProps) => (
  <a
    href="#"
    aria-current={isActive ? 'page' : undefined}
    className={cn(
      'size-8',
      buttonVariants({
        variant: isActive ? 'outline' : 'ghost',
        size,
      }),
      className,
    )}
    {...props}
  />
);
PaginationLink.displayName = 'PaginationLink';

const PaginationPrevious = ({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink
    aria-label="Go to previous page"
    size="default"
    className={cn('gap-1 pl-2.5', className)}
    {...props}
  >
    <ChevronLeft className="size-4" />
  </PaginationLink>
);
PaginationPrevious.displayName = 'PaginationPrevious';

const PaginationNext = ({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink
    aria-label="Go to next page"
    size="default"
    className={cn('gap-1 pr-2.5', className)}
    {...props}
  >
    <ChevronRight className="size-4" />
  </PaginationLink>
);
PaginationNext.displayName = 'PaginationNext';

const PaginationEllipsis = ({
  className,
  ...props
}: React.ComponentProps<'span'>) => (
  <span
    aria-hidden
    className={cn('flex items-center justify-center', className)}
    {...props}
  >
    <MoreHorizontal className="h-4 w-4" />
    <span className="sr-only">More pages</span>
  </span>
);
PaginationEllipsis.displayName = 'PaginationEllipsis';

export {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/pagination.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 118 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (7)

- `Pagination()`: Function definition
- `PaginationContent()`: Function definition
- `PaginationItem()`: Function definition
- `PaginationLink()`: Function definition
- `PaginationPrevious()`: Function definition
- `PaginationNext()`: Function definition
- `PaginationEllipsis()`: Function definition

### Imports (4)

- `import { ChevronLeft, ChevronRight, MoreHorizontal } from 'lucide-react';`
- `import * as React from 'react';`
- `import { ButtonProps, buttonVariants } from '@/components/ui/button';`
- `import { cn } from '@/lib/utils';`

## Code Structure Analysis

- Total lines: 118
- Blank lines: 11 (9.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~107


## Dependencies and Imports

- `lucide-react`
- `react`
- `@/components/ui/button`
- `@/lib/utils`

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
- Potential test file: `test_pagination.tsx`

## Keywords

@/components/ui/button, @/lib/utils, ButtonProps, ChevronLeft, ChevronRight, ComponentProps, HTMLLIElement, HTMLUListElement, More, MoreHorizontal, Pagination, PaginationContent, PaginationEllipsis, PaginationItem, PaginationLink, PaginationLinkProps, PaginationNext, PaginationPrevious, Pick, React, TypeScript, lucide-react, react

---
*Generated by RAGFlow Repository Documentation Generator*
