# File Documentation: web/src/pages/next-searches/index.tsx

## File Metadata

- **Path**: `web/src/pages/next-searches/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 105
- **Characters**: 3,183
- **Size**: 3,183 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { CardContainer } from '@/components/card-container';
import { IconFont } from '@/components/icon-font';
import ListFilterBar from '@/components/list-filter-bar';
import { RenameDialog } from '@/components/rename-dialog';
import { Button } from '@/components/ui/button';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { useTranslate } from '@/hooks/common-hooks';
import { Plus } from 'lucide-react';
import { useFetchSearchList, useRenameSearch } from './hooks';
import { SearchCard } from './search-card';

export default function SearchList() {
  // const { data } = useFetchFlowList();
  const { t } = useTranslate('search');
  // const [isEdit, setIsEdit] = useState(false);
  const {
    data: list,
    searchParams,
    setSearchListParams,
    refetch: refetchList,
  } = useFetchSearchList();
  const {
    openCreateModal,
    showSearchRenameModal,
    hideSearchRenameModal,
    searchRenameLoading,
    onSearchRenameOk,
    initialSearchName,
  } = useRenameSearch();
  const handleSearchChange = (value: string) => {
    console.log(value);
  };
  const onSearchRenameConfirm = (name: string) => {
    onSearchRenameOk(name, () => {
      refetchList();
    });
  };
  const openCreateModalFun = () => {
    // setIsEdit(false);
    showSearchRenameModal();
  };
  const handlePageChange = (page: number, pageSize: number) => {
    // setIsEdit(false);
    setSearchListParams({ ...searchParams, page, page_size: pageSize });
  };

  return (
    <section className="w-full h-full flex flex-col">
      <div className="px-8 pt-8">
        <ListFilterBar
          icon="searches"
          title={t('searchApps')}
          showFilter={false}
          onSearchChange={(e) => handleSearchChange(e.target.value)}
        >
          <Button
            variant={'default'}
            onClick={() => {
              openCreateModalFun();
            }}
          >
            <Plus className="mr-2 h-4 w-4" />
            {t('createSearch')}
          </Button>
        </ListFilterBar>
      </div>
      <div className="flex-1">
        <CardContainer className="max-h-[calc(100dvh-280px)] overflow-auto px-8">
          {list?.data.search_apps.map((x) => {
            return (
              <SearchCard
                key={x.id}
                data={x}
                showSearchRenameModal={() => {
                  showSearchRenameModal(x);
                }}
              ></SearchCard>
            );
          })}
        </CardContainer>
      </div>
      {list?.data.total && list?.data.total > 0 && (
        <div className="px-8 mb-4">
          <RAGFlowPagination
            current={searchParams.page}
            pageSize={searchParams.page_size}
            total={list?.data.total}
            onChange={handlePageChange}
          />
        </div>
      )}

      {openCreateModal && (
        <RenameDialog
          hideModal={hideSearchRenameModal}
          onOk={onSearchRenameConfirm}
          initialName={initialSearchName}
          loading={searchRenameLoading}
          title={<IconFont name="search" className="size-6"></IconFont>}
        ></RenameDialog>
      )}
    </section>
  );
}

```

## High-Level Overview

  // const { data } = useFetchFlowList();
  // const [isEdit, setIsEdit] = useState(false);
    // setIsEdit(false);
    // setIsEdit(false);

## Detailed Walkthrough

### Exports (1)

- `SearchList`: Exported entity

### Functions (5)

- `SearchList()`: Function definition
- `handleSearchChange()`: Function definition
- `onSearchRenameConfirm()`: Function definition
- `openCreateModalFun()`: Function definition
- `handlePageChange()`: Function definition

### Imports (10)

- `import { CardContainer } from '@/components/card-container';`
- `import { IconFont } from '@/components/icon-font';`
- `import ListFilterBar from '@/components/list-filter-bar';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { Plus } from 'lucide-react';`
- `import { useFetchSearchList, useRenameSearch } from './hooks';`
- `import { SearchCard } from './search-card';`

## Code Structure Analysis

- Total lines: 105
- Blank lines: 4 (3.8%)
- Comment lines: ~4 (3.8%)
- Code lines: ~97


## Dependencies and Imports

- `@/components/card-container`
- `@/components/icon-font`
- `@/components/list-filter-bar`
- `@/components/rename-dialog`
- `@/components/ui/button`
- `@/components/ui/ragflow-pagination`
- `@/hooks/common-hooks`
- `lucide-react`
- `./hooks`
- `./search-card`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-searches`.

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

- Other files in `web/src/pages/next-searches/` directory
- Potential test file: `test_index.tsx`

## Keywords

./hooks, ./search-card, @/components/card-container, @/components/icon-font, @/components/list-filter-bar, @/components/rename-dialog, @/components/ui/button, @/components/ui/ragflow-pagination, @/hooks/common-hooks, Button, CardContainer, IconFont, ListFilterBar, Plus, RAGFlowPagination, RenameDialog, SearchCard, SearchList, TypeScript, handlePageChange, handleSearchChange, lucide-react, onSearchRenameConfirm, openCreateModalFun

---
*Generated by RAGFlow Repository Documentation Generator*
