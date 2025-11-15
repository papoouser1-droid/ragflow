# Documentation: web/src/pages/datasets/index.tsx

## File Metadata

- **Path**: `web/src/pages/datasets/index.tsx`
- **Size**: 3204 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/datasets/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/datasets/index.tsx` is located in the `web/src/pages/datasets` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to datasets.

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

- [dataset-card.tsx](dataset-card.tsx_docs.md)
- [dataset-creating-dialog.tsx](dataset-creating-dialog.tsx_docs.md)
- [dataset-dataflow-creating-dialog.tsx](dataset-dataflow-creating-dialog.tsx_docs.md)
- [dataset-dropdown.tsx](dataset-dropdown.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [use-display-owner.ts](use-display-owner.ts_docs.md)
- [use-rename-dataset.ts](use-rename-dataset.ts_docs.md)
- [use-select-owners.ts](use-select-owners.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
