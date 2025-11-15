# File Documentation: web/src/components/ui/ragflow-pagination.tsx

## File Metadata

- **Path**: `web/src/components/ui/ragflow-pagination.tsx`
- **Extension**: `.tsx`
- **Lines**: 187
- **Characters**: 4,791
- **Size**: 4,791 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from '@/components/ui/pagination';
import { RAGFlowSelect, RAGFlowSelectOptionType } from '@/components/ui/select';
import { cn } from '@/lib/utils';
import { t } from 'i18next';
import { useCallback, useEffect, useMemo, useState } from 'react';

export type RAGFlowPaginationType = {
  showQuickJumper?: boolean;
  onChange?(page: number, pageSize: number): void;
  total?: number;
  current?: number;
  pageSize?: number;
  showSizeChanger?: boolean;
};

export function RAGFlowPagination({
  current = 1,
  pageSize = 5,
  total = 0,
  onChange,
  showSizeChanger = true,
}: RAGFlowPaginationType) {
  const [currentPage, setCurrentPage] = useState(1);
  const [currentPageSize, setCurrentPageSize] = useState('10');

  const sizeChangerOptions: RAGFlowSelectOptionType[] = useMemo(() => {
    return [10, 20, 50, 100].map((x) => ({
      label: <span>{t('pagination.page', { page: x })}</span>,
      value: x.toString(),
    }));
  }, []);

  const pages = useMemo(() => {
    const num = Math.ceil(total / pageSize);
    return new Array(num).fill(0).map((_, idx) => idx + 1);
  }, [pageSize, total]);

  const changePage = useCallback(
    (page: number) => {
      onChange?.(page, Number(currentPageSize));
    },
    [currentPageSize, onChange],
  );

  const handlePreviousPageChange = useCallback(() => {
    setCurrentPage((page) => {
      const previousPage = page - 1;
      if (previousPage > 0) {
        changePage(previousPage);
        return previousPage;
      }
      changePage(page);
      return page;
    });
  }, [changePage]);

  const handlePageChange = useCallback(
    (page: number) => () => {
      changePage(page);
      setCurrentPage(page);
    },
    [changePage],
  );

  const handleNextPageChange = useCallback(() => {
    setCurrentPage((page) => {
      const nextPage = page + 1;
      if (nextPage <= pages.length) {
        changePage(nextPage);
        return nextPage;
      }
      changePage(page);
      return page;
    });
  }, [changePage, pages.length]);

  const handlePageSizeChange = useCallback(
    (size: string) => {
      onChange?.(currentPage, Number(size));
      setCurrentPageSize(size);
    },
    [currentPage, onChange],
  );

  useEffect(() => {
    setCurrentPage(current);
  }, [current]);

  useEffect(() => {
    setCurrentPageSize(pageSize.toString());
  }, [pageSize]);

  // Generates an array of page numbers to display
  const displayedPages = useMemo(() => {
    const totalPages = pages.length;
    const maxDisplayedPages = 5;

    if (totalPages <= maxDisplayedPages) {
      return pages;
    }

    const left = Math.max(2, currentPage - 2);
    const right = Math.min(totalPages - 1, currentPage + 2);

    const newPages = [];

    newPages.push(1);

    if (left > 2) {
      newPages.push(-1); // Indicates an ellipsis
    }

    for (let i = left; i <= right; i++) {
      newPages.push(i);
    }

    if (right < totalPages - 1) {
      newPages.push(-1);
    }

    if (totalPages > 1) {
      newPages.push(totalPages);
    }

    return newPages;
  }, [pages, currentPage]);

  return (
    <section className="flex items-center justify-end text-text-sub-title-invert">
      <span className="mr-4 text-text-primary">
        {t('pagination.total', { total: total })}
      </span>
      <Pagination className="w-auto mx-0 mr-4">
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious onClick={handlePreviousPageChange} />
          </PaginationItem>

          {displayedPages.map((page, index) =>
            page === -1 ? (
              <PaginationItem key={`ellipsis-${index}`}>
                <PaginationEllipsis />
              </PaginationItem>
            ) : (
              <PaginationItem
                key={page}
                className={cn('text-text-disabled', {
                  ['bg-bg-card rounded-md text-text-primary']:
                    currentPage === page,
                })}
              >
                <PaginationLink
                  onClick={handlePageChange(page)}
                  className="size-8"
                >
                  {page}
                </PaginationLink>
              </PaginationItem>
            ),
          )}

          <PaginationItem>
            <PaginationNext onClick={handleNextPageChange} />
          </PaginationItem>
        </PaginationContent>
      </Pagination>

      {showSizeChanger && (
        <RAGFlowSelect
          options={sizeChangerOptions}
          value={currentPageSize}
          onChange={handlePageSizeChange}
          triggerClassName="bg-bg-card border-transparent"
        />
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/ragflow-pagination.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 187 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `RAGFlowPagination`: Exported entity

### Functions (9)

- `RAGFlowPagination()`: Function definition
- `pages()`: Function definition
- `num()`: Function definition
- `changePage()`: Function definition
- `handlePreviousPageChange()`: Function definition
- `handlePageChange()`: Function definition
- `handleNextPageChange()`: Function definition
- `handlePageSizeChange()`: Function definition
- `displayedPages()`: Function definition

### Imports (5)

- `import {`
- `import { RAGFlowSelect, RAGFlowSelectOptionType } from '@/components/ui/select';`
- `import { cn } from '@/lib/utils';`
- `import { t } from 'i18next';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 187
- Blank lines: 26 (13.9%)
- Comment lines: ~1 (0.5%)
- Code lines: ~160


## Dependencies and Imports

- `@/components/ui/select`
- `@/lib/utils`
- `i18next`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_ragflow-pagination.tsx`

## Keywords

@/components/ui/select, @/lib/utils, Array, Generates, Indicates, Math, Number, Pagination, PaginationContent, PaginationEllipsis, PaginationItem, PaginationLink, PaginationNext, PaginationPrevious, RAGFlowPagination, RAGFlowPaginationType, RAGFlowSelect, RAGFlowSelectOptionType, TypeScript, changePage, displayedPages, handleNextPageChange, handlePageChange, handlePageSizeChange, handlePreviousPageChange, i, i18next, left, maxDisplayedPages, newPages, nextPage, num, pages, previousPage, react, right, sizeChangerOptions, totalPages

---
*Generated by RAGFlow Repository Documentation Generator*
