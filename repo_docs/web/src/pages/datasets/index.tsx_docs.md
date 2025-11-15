# File Documentation: web/src/pages/datasets/index.tsx

## File Metadata

- **Path**: `web/src/pages/datasets/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 111
- **Characters**: 3,204
- **Size**: 3,204 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { CardContainer } from '@/components/card-container';
import ListFilterBar from '@/components/list-filter-bar';
import { RenameDialog } from '@/components/rename-dialog';
import { Button } from '@/components/ui/button';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { useFetchNextKnowledgeListByPage } from '@/hooks/use-knowledge-request';
import { pick } from 'lodash';
import { Plus } from 'lucide-react';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { DatasetCard } from './dataset-card';
import { DatasetCreatingDialog } from './dataset-creating-dialog';
import { useSaveKnowledge } from './hooks';
import { useRenameDataset } from './use-rename-dataset';
import { useSelectOwners } from './use-select-owners';

export default function Datasets() {
  const { t } = useTranslation();
  const {
    visible,
    hideModal,
    showModal,
    onCreateOk,
    loading: creatingLoading,
  } = useSaveKnowledge();

  const {
    kbs,
    total,
    pagination,
    setPagination,
    handleInputChange,
    searchString,
    filterValue,
    handleFilterSubmit,
  } = useFetchNextKnowledgeListByPage();

  const owners = useSelectOwners();

  const {
    datasetRenameLoading,
    initialDatasetName,
    onDatasetRenameOk,
    datasetRenameVisible,
    hideDatasetRenameModal,
    showDatasetRenameModal,
  } = useRenameDataset();

  const handlePageChange = useCallback(
    (page: number, pageSize?: number) => {
      setPagination({ page, pageSize });
    },
    [setPagination],
  );

  return (
    <section className="py-4 flex-1 flex flex-col">
      <ListFilterBar
        title={t('header.dataset')}
        searchString={searchString}
        onSearchChange={handleInputChange}
        value={filterValue}
        filters={owners}
        onChange={handleFilterSubmit}
        className="px-8"
        icon={'datasets'}
      >
        <Button onClick={showModal}>
          <Plus className=" size-2.5" />
          {t('knowledgeList.createKnowledgeBase')}
        </Button>
      </ListFilterBar>
      <div className="flex-1">
        <CardContainer className="max-h-[calc(100dvh-280px)] overflow-auto px-8">
          {kbs.map((dataset) => {
            return (
              <DatasetCard
                dataset={dataset}
                key={dataset.id}
                showDatasetRenameModal={showDatasetRenameModal}
              ></DatasetCard>
            );
          })}
        </CardContainer>
      </div>
      <div className="mt-8 px-8">
        <RAGFlowPagination
          {...pick(pagination, 'current', 'pageSize')}
          total={total}
          onChange={handlePageChange}
        ></RAGFlowPagination>
      </div>
      {visible && (
        <DatasetCreatingDialog
          hideModal={hideModal}
          onOk={onCreateOk}
          loading={creatingLoading}
        ></DatasetCreatingDialog>
      )}
      {datasetRenameVisible && (
        <RenameDialog
          hideModal={hideDatasetRenameModal}
          onOk={onDatasetRenameOk}
          initialName={initialDatasetName}
          loading={datasetRenameLoading}
        ></RenameDialog>
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/datasets/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 111 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `Datasets`: Exported entity

### Functions (2)

- `Datasets()`: Function definition
- `handlePageChange()`: Function definition

### Imports (15)

- `import { CardContainer } from '@/components/card-container';`
- `import ListFilterBar from '@/components/list-filter-bar';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import { useFetchNextKnowledgeListByPage } from '@/hooks/use-knowledge-request';`
- `import { pick } from 'lodash';`
- `import { Plus } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 111
- Blank lines: 7 (6.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~104


## Dependencies and Imports

- `@/components/card-container`
- `@/components/list-filter-bar`
- `@/components/rename-dialog`
- `@/components/ui/button`
- `@/components/ui/ragflow-pagination`
- `@/hooks/use-knowledge-request`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`
- `./dataset-card`
- `./dataset-creating-dialog`
- `./hooks`
- `./use-rename-dataset`
- `./use-select-owners`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/datasets`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/datasets/` directory
- Potential test file: `test_index.tsx`

## Keywords

./dataset-card, ./dataset-creating-dialog, ./hooks, ./use-rename-dataset, ./use-select-owners, @/components/card-container, @/components/list-filter-bar, @/components/rename-dialog, @/components/ui/button, @/components/ui/ragflow-pagination, @/hooks/use-knowledge-request, Button, CardContainer, DatasetCard, DatasetCreatingDialog, Datasets, ListFilterBar, Plus, RAGFlowPagination, RenameDialog, TypeScript, handlePageChange, lodash, lucide-react, owners, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
