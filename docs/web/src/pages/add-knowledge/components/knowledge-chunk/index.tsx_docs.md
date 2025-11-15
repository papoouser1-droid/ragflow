# Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx`
- **Size**: 6343 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-chunk/index.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-chunk` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge-chunk.

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

- [constant.ts](constant.ts_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
