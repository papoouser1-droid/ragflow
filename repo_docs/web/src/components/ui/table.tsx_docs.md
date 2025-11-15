# File Documentation: web/src/components/ui/table.tsx

## File Metadata

- **Path**: `web/src/components/ui/table.tsx`
- **Extension**: `.tsx`
- **Lines**: 133
- **Characters**: 3,163
- **Size**: 3,163 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import * as React from 'react';

import { cn } from '@/lib/utils';

const Table = React.forwardRef<
  HTMLTableElement,
  React.HTMLAttributes<HTMLTableElement> & { rootClassName?: string }
>(({ className, rootClassName, ...props }, ref) => (
  <div
    className={cn(
      'relative w-full overflow-auto rounded-2xl bg-bg-card scrollbar-auto',
      rootClassName,
    )}
  >
    <table
      ref={ref}
      className={cn('w-full caption-bottom text-sm ', className)}
      {...props}
    />
  </div>
));
Table.displayName = 'Table';

const TableHeader = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <thead
    ref={ref}
    className={cn(
      '[&_tr]:border-b-0.5 top-0 sticky bg-bg-title z-10',
      className,
    )}
    {...props}
  />
));
TableHeader.displayName = 'TableHeader';

const TableBody = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <tbody
    ref={ref}
    className={cn('[&_tr:last-child]:border-0', className)}
    {...props}
  />
));
TableBody.displayName = 'TableBody';

const TableFooter = React.forwardRef<
  HTMLTableSectionElement,
  React.HTMLAttributes<HTMLTableSectionElement>
>(({ className, ...props }, ref) => (
  <tfoot
    ref={ref}
    className={cn(
      'border-t-0.5 border-border-button bg-muted/50 font-medium [&>tr]:last:border-b-0',
      className,
    )}
    {...props}
  />
));
TableFooter.displayName = 'TableFooter';

const TableRow = React.forwardRef<
  HTMLTableRowElement,
  React.HTMLAttributes<HTMLTableRowElement>
>(({ className, ...props }, ref) => (
  <tr
    ref={ref}
    className={cn(
      'border-b-0.5 border-border-button transition-colors hover:bg-bg-card data-[state=selected]:bg-bg-card',
      className,
    )}
    {...props}
  />
));
TableRow.displayName = 'TableRow';

const TableHead = React.forwardRef<
  HTMLTableCellElement,
  React.ThHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
  <th
    ref={ref}
    className={cn(
      'first-of-type:pl-6 last-of-type:pr-6 h-14 px-4 text-left align-middle font-normal text-text-secondary [&:has([role=checkbox])]:pr-0 ',
      className,
    )}
    {...props}
  />
));
TableHead.displayName = 'TableHead';

const TableCell = React.forwardRef<
  HTMLTableCellElement,
  React.TdHTMLAttributes<HTMLTableCellElement>
>(({ className, ...props }, ref) => (
  <td
    ref={ref}
    className={cn(
      'first-of-type:pl-6 last-of-type:pr-6 px-4 py-3 align-middle [&:has([role=checkbox])]:pr-0 text-text-primary font-normal',
      className,
    )}
    {...props}
  />
));
TableCell.displayName = 'TableCell';

const TableCaption = React.forwardRef<
  HTMLTableCaptionElement,
  React.HTMLAttributes<HTMLTableCaptionElement>
>(({ className, ...props }, ref) => (
  <caption
    ref={ref}
    className={cn('mt-4 text-sm text-muted-foreground', className)}
    {...props}
  />
));
TableCaption.displayName = 'TableCaption';

export {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableFooter,
  TableHead,
  TableHeader,
  TableRow,
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 133 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (8)

- `Table()`: Function definition
- `TableHeader()`: Function definition
- `TableBody()`: Function definition
- `TableFooter()`: Function definition
- `TableRow()`: Function definition
- `TableHead()`: Function definition
- `TableCell()`: Function definition
- `TableCaption()`: Function definition

### Imports (2)

- `import * as React from 'react';`
- `import { cn } from '@/lib/utils';`

## Code Structure Analysis

- Total lines: 133
- Blank lines: 11 (8.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~122


## Dependencies and Imports

- `react`
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
- Potential test file: `test_table.tsx`

## Keywords

@/lib/utils, HTMLAttributes, HTMLTableCaptionElement, HTMLTableCellElement, HTMLTableElement, HTMLTableRowElement, HTMLTableSectionElement, React, Table, TableBody, TableCaption, TableCell, TableFooter, TableHead, TableHeader, TableRow, TdHTMLAttributes, ThHTMLAttributes, TypeScript, react

---
*Generated by RAGFlow Repository Documentation Generator*
