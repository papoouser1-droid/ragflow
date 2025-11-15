# File Documentation: web/src/pages/dataset/testing/testing-result.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/testing/testing-result.tsx`
- **Extension**: `.tsx`
- **Lines**: 122
- **Characters**: 3,802
- **Size**: 3,802 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```tsx
import Empty from '@/components/empty/empty';
import { FormContainer } from '@/components/form-container';
import { FilterButton } from '@/components/list-filter-bar';
import { FilterPopover } from '@/components/list-filter-bar/filter-popover';
import { FilterCollection } from '@/components/list-filter-bar/interface';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { useTranslate } from '@/hooks/common-hooks';
import { useTestRetrieval } from '@/hooks/use-knowledge-request';
import { ITestingChunk } from '@/interfaces/database/knowledge';
import { t } from 'i18next';
import camelCase from 'lodash/camelCase';
import { useMemo } from 'react';

const similarityList: Array<{ field: keyof ITestingChunk; label: string }> = [
  { field: 'similarity', label: 'Hybrid Similarity' },
  { field: 'term_similarity', label: 'Term Similarity' },
  { field: 'vector_similarity', label: 'Vector Similarity' },
];

const ChunkTitle = ({ item }: { item: ITestingChunk }) => {
  const { t } = useTranslate('knowledgeDetails');
  return (
    <div className="flex gap-3 text-xs text-text-sub-title-invert italic">
      {similarityList.map((x) => (
        <div key={x.field} className="space-x-1">
          <span>{((item[x.field] as number) * 100).toFixed(2)}</span>
          <span>{t(camelCase(x.field))}</span>
        </div>
      ))}
    </div>
  );
};

type TestingResultProps = Pick<
  ReturnType<typeof useTestRetrieval>,
  | 'data'
  | 'filterValue'
  | 'handleFilterSubmit'
  | 'page'
  | 'pageSize'
  | 'onPaginationChange'
  | 'loading'
>;

export function TestingResult({
  filterValue,
  handleFilterSubmit,
  page,
  pageSize,
  loading,
  onPaginationChange,
  data,
}: TestingResultProps) {
  const filters: FilterCollection[] = useMemo(() => {
    return [
      {
        field: 'doc_ids',
        label: 'File',
        list:
          data.doc_aggs?.map((x) => ({
            id: x.doc_id,
            label: x.doc_name,
            count: x.count,
          })) ?? [],
      },
    ];
  }, [data.doc_aggs]);

  return (
    <div className="p-4 flex-1">
      <div className="flex justify-between pb-2.5">
        <span className="text-text-primary font-semibold text-2xl">
          {t('knowledgeDetails.testResults')}
        </span>
        <FilterPopover
          filters={filters}
          onChange={handleFilterSubmit}
          value={filterValue}
        >
          <FilterButton></FilterButton>
        </FilterPopover>
      </div>
      {data.chunks?.length > 0 && !loading && (
        <>
          <section className="flex flex-col gap-5 overflow-auto h-[calc(100vh-241px)] scrollbar-thin mb-5">
            {data.chunks?.map((x) => (
              <FormContainer key={x.chunk_id} className="px-5 py-2.5">
                <ChunkTitle item={x}></ChunkTitle>
                <p className="!mt-2.5"> {x.content_with_weight}</p>
              </FormContainer>
            ))}
          </section>
          <RAGFlowPagination
            total={data.total}
            onChange={onPaginationChange}
            current={page}
            pageSize={pageSize}
          ></RAGFlowPagination>
        </>
      )}
      {!data.chunks?.length && !loading && (
        <div className="flex justify-center items-center w-full h-[calc(100vh-241px)]">
          <div>
            <Empty>
              {data.isRuned && (
                <div className="text-text-secondary">
                  {t('knowledgeDetails.noTestResultsForRuned')}
                </div>
              )}
              {!data.isRuned && (
                <div className="text-text-secondary">
                  {t('knowledgeDetails.noTestResultsForNotRuned')}
                </div>
              )}
            </Empty>
          </div>
        </div>
      )}
    </div>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/testing/testing-result.tsx`.

Based on the file structure and naming, it appears to be a testing - contains unit tests, integration tests, or test utilities.

The file contains approximately 122 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `TestingResult`: Exported entity

### Functions (2)

- `ChunkTitle()`: Function definition
- `TestingResult()`: Function definition

### Imports (12)

- `import Empty from '@/components/empty/empty';`
- `import { FormContainer } from '@/components/form-container';`
- `import { FilterButton } from '@/components/list-filter-bar';`
- `import { FilterPopover } from '@/components/list-filter-bar/filter-popover';`
- `import { FilterCollection } from '@/components/list-filter-bar/interface';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useTestRetrieval } from '@/hooks/use-knowledge-request';`
- `import { ITestingChunk } from '@/interfaces/database/knowledge';`
- `import { t } from 'i18next';`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 6 (4.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~116


## Dependencies and Imports

- `@/components/empty/empty`
- `@/components/form-container`
- `@/components/list-filter-bar`
- `@/components/list-filter-bar/filter-popover`
- `@/components/list-filter-bar/interface`
- `@/components/ui/ragflow-pagination`
- `@/hooks/common-hooks`
- `@/hooks/use-knowledge-request`
- `@/interfaces/database/knowledge`
- `i18next`
- `lodash/camelCase`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/testing`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `web/src/pages/dataset/testing/` directory

## Keywords

@/components/empty/empty, @/components/form-container, @/components/list-filter-bar, @/components/list-filter-bar/filter-popover, @/components/list-filter-bar/interface, @/components/ui/ragflow-pagination, @/hooks/common-hooks, @/hooks/use-knowledge-request, @/interfaces/database/knowledge, Array, ChunkTitle, Empty, File, FilterButton, FilterCollection, FilterPopover, FormContainer, Hybrid, ITestingChunk, Pick, RAGFlowPagination, ReturnType, Similarity, Term, TestingResult, TestingResultProps, TypeScript, Vector, filters, i18next, lodash/camelCase, react, similarityList

---
*Generated by RAGFlow Repository Documentation Generator*
