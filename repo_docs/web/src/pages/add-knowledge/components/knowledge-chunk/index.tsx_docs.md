# File Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 203
- **Characters**: 6,343
- **Size**: 6,343 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useFetchNextChunkList, useSwitchChunk } from '@/hooks/chunk-hooks';
import type { PaginationProps } from 'antd';
import { Divider, Flex, Pagination, Space, Spin, message } from 'antd';
import classNames from 'classnames';
import { useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';
import ChunkCard from './components/chunk-card';
import CreatingModal from './components/chunk-creating-modal';
import ChunkToolBar from './components/chunk-toolbar';
import DocumentPreview from './components/document-preview/preview';
import {
  useChangeChunkTextMode,
  useDeleteChunkByIds,
  useGetChunkHighlights,
  useHandleChunkCardClick,
  useUpdateChunk,
} from './hooks';

import styles from './index.less';

const Chunk = () => {
  const [selectedChunkIds, setSelectedChunkIds] = useState<string[]>([]);
  const { removeChunk } = useDeleteChunkByIds();
  const {
    data: { documentInfo, data = [], total },
    pagination,
    loading,
    searchString,
    handleInputChange,
    available,
    handleSetAvailable,
  } = useFetchNextChunkList();
  const { handleChunkCardClick, selectedChunkId } = useHandleChunkCardClick();
  const isPdf = documentInfo?.type === 'pdf';

  const { t } = useTranslation();
  const { changeChunkTextMode, textMode } = useChangeChunkTextMode();
  const { switchChunk } = useSwitchChunk();
  const {
    chunkUpdatingLoading,
    onChunkUpdatingOk,
    showChunkUpdatingModal,
    hideChunkUpdatingModal,
    chunkId,
    chunkUpdatingVisible,
    documentId,
  } = useUpdateChunk();

  const onPaginationChange: PaginationProps['onShowSizeChange'] = (
    page,
    size,
  ) => {
    setSelectedChunkIds([]);
    pagination.onChange?.(page, size);
  };

  const selectAllChunk = useCallback(
    (checked: boolean) => {
      setSelectedChunkIds(checked ? data.map((x) => x.chunk_id) : []);
    },
    [data],
  );

  const handleSingleCheckboxClick = useCallback(
    (chunkId: string, checked: boolean) => {
      setSelectedChunkIds((previousIds) => {
        const idx = previousIds.findIndex((x) => x === chunkId);
        const nextIds = [...previousIds];
        if (checked && idx === -1) {
          nextIds.push(chunkId);
        } else if (!checked && idx !== -1) {
          nextIds.splice(idx, 1);
        }
        return nextIds;
      });
    },
    [],
  );

  const showSelectedChunkWarning = useCallback(() => {
    message.warning(t('message.pleaseSelectChunk'));
  }, [t]);

  const handleRemoveChunk = useCallback(async () => {
    if (selectedChunkIds.length > 0) {
      const resCode: number = await removeChunk(selectedChunkIds, documentId);
      if (resCode === 0) {
        setSelectedChunkIds([]);
      }
    } else {
      showSelectedChunkWarning();
    }
  }, [selectedChunkIds, documentId, removeChunk, showSelectedChunkWarning]);

  const handleSwitchChunk = useCallback(
    async (available?: number, chunkIds?: string[]) => {
      let ids = chunkIds;
      if (!chunkIds) {
        ids = selectedChunkIds;
        if (selectedChunkIds.length === 0) {
          showSelectedChunkWarning();
          return;
        }
      }

      const resCode: number = await switchChunk({
        chunk_ids: ids,
        available_int: available,
        doc_id: documentId,
      });
      if (!chunkIds && resCode === 0) {
      }
    },
    [switchChunk, documentId, selectedChunkIds, showSelectedChunkWarning],
  );

  const { highlights, setWidthAndHeight } =
    useGetChunkHighlights(selectedChunkId);

  return (
    <>
      <div className={styles.chunkPage}>
        <ChunkToolBar
          selectAllChunk={selectAllChunk}
          createChunk={showChunkUpdatingModal}
          removeChunk={handleRemoveChunk}
          checked={selectedChunkIds.length === data.length}
          switchChunk={handleSwitchChunk}
          changeChunkTextMode={changeChunkTextMode}
          searchString={searchString}
          handleInputChange={handleInputChange}
          available={available}
          handleSetAvailable={handleSetAvailable}
        ></ChunkToolBar>
        <Divider></Divider>
        <Flex flex={1} gap={'middle'}>
          <Flex
            vertical
            className={isPdf ? styles.pagePdfWrapper : styles.pageWrapper}
          >
            <Spin spinning={loading} className={styles.spin} size="large">
              <div className={styles.pageContent}>
                <Space
                  direction="vertical"
                  size={'middle'}
                  className={classNames(styles.chunkContainer, {
                    [styles.chunkOtherContainer]: !isPdf,
                  })}
                >
                  {data.map((item) => (
                    <ChunkCard
                      item={item}
                      key={item.chunk_id}
                      editChunk={showChunkUpdatingModal}
                      checked={selectedChunkIds.some(
                        (x) => x === item.chunk_id,
                      )}
                      handleCheckboxClick={handleSingleCheckboxClick}
                      switchChunk={handleSwitchChunk}
                      clickChunkCard={handleChunkCardClick}
                      selected={item.chunk_id === selectedChunkId}
                      textMode={textMode}
                    ></ChunkCard>
                  ))}
                </Space>
              </div>
            </Spin>
            <div className={styles.pageFooter}>
              <Pagination
                {...pagination}
                total={total}
                size={'small'}
                onChange={onPaginationChange}
              />
            </div>
          </Flex>
          {isPdf && (
            <section className={styles.documentPreview}>
              <DocumentPreview
                highlights={highlights}
                setWidthAndHeight={setWidthAndHeight}
              ></DocumentPreview>
            </section>
          )}
        </Flex>
      </div>
      {chunkUpdatingVisible && (
        <CreatingModal
          doc_id={documentId}
          chunkId={chunkId}
          hideModal={hideChunkUpdatingModal}
          visible={chunkUpdatingVisible}
          loading={chunkUpdatingLoading}
          onOk={onChunkUpdatingOk}
          parserId={documentInfo.parser_id}
        />
      )}
    </>
  );
};

export default Chunk;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 203 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (7)

- `Chunk()`: Function definition
- `selectAllChunk()`: Function definition
- `handleSingleCheckboxClick()`: Function definition
- `idx()`: Function definition
- `showSelectedChunkWarning()`: Function definition
- `handleRemoveChunk()`: Function definition
- `handleSwitchChunk()`: Function definition

### Imports (12)

- `import { useFetchNextChunkList, useSwitchChunk } from '@/hooks/chunk-hooks';`
- `import type { PaginationProps } from 'antd';`
- `import { Divider, Flex, Pagination, Space, Spin, message } from 'antd';`
- `import classNames from 'classnames';`
- `import { useCallback, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import ChunkCard from './components/chunk-card';`
- `import CreatingModal from './components/chunk-creating-modal';`
- `import ChunkToolBar from './components/chunk-toolbar';`
- `import DocumentPreview from './components/document-preview/preview';`

## Code Structure Analysis

- Total lines: 203
- Blank lines: 14 (6.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~189


## Dependencies and Imports

- `@/hooks/chunk-hooks`
- `antd`
- `antd`
- `classnames`
- `react`
- `react-i18next`
- `./components/chunk-card`
- `./components/chunk-creating-modal`
- `./components/chunk-toolbar`
- `./components/document-preview/preview`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-chunk`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/add-knowledge/components/knowledge-chunk/` directory
- Potential test file: `test_index.tsx`

## Keywords

./components/chunk-card, ./components/chunk-creating-modal, ./components/chunk-toolbar, ./components/document-preview/preview, ./index.less, @/hooks/chunk-hooks, Chunk, ChunkCard, ChunkToolBar, CreatingModal, Divider, DocumentPreview, Flex, Pagination, PaginationProps, Space, Spin, TypeScript, antd, classnames, handleRemoveChunk, handleSingleCheckboxClick, handleSwitchChunk, ids, idx, isPdf, nextIds, onPaginationChange, react, react-i18next, resCode, selectAllChunk, showSelectedChunkWarning

---
*Generated by RAGFlow Repository Documentation Generator*
