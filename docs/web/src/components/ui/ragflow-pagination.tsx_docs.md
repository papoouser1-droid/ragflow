# Documentation: web/src/components/ui/ragflow-pagination.tsx

## File Metadata

- **Path**: `web/src/components/ui/ragflow-pagination.tsx`
- **Size**: 4791 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/ragflow-pagination.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/ragflow-pagination.tsx` is located in the `web/src/components/ui` directory.

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
