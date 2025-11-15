# File Documentation: web/src/pages/dataflow-result/chunker.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/chunker.tsx`
- **Extension**: `.tsx`
- **Lines**: 242
- **Characters**: 7,670
- **Size**: 7,670 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { TimelineNode } from '@/components/originui/timeline';
import message from '@/components/ui/message';
import {
  RAGFlowPagination,
  RAGFlowPaginationType,
} from '@/components/ui/ragflow-pagination';
import { Spin } from '@/components/ui/spin';
import {
  useFetchNextChunkList,
  useSwitchChunk,
} from '@/hooks/use-chunk-request';
import classNames from 'classnames';
import { useCallback, useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';
import ChunkCard from './components/chunk-card';
import CreatingModal from './components/chunk-creating-modal';
import ChunkResultBar from './components/chunk-result-bar';
import CheckboxSets from './components/chunk-result-bar/checkbox-sets';
import RerunButton from './components/rerun-button';
import {
  useChangeChunkTextMode,
  useDeleteChunkByIds,
  useHandleChunkCardClick,
  useUpdateChunk,
} from './hooks';
import styles from './index.less';

interface IProps {
  isChange: boolean;
  setIsChange: (isChange: boolean) => void;
  step?: TimelineNode;
}
const ChunkerContainer = (props: IProps) => {
  const { isChange, setIsChange, step } = props;
  const [selectedChunkIds, setSelectedChunkIds] = useState<string[]>([]);

  const { t } = useTranslation();
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
  const {
    chunkUpdatingLoading,
    onChunkUpdatingOk,
    showChunkUpdatingModal,
    hideChunkUpdatingModal,
    chunkId,
    chunkUpdatingVisible,
    documentId,
  } = useUpdateChunk();
  const { removeChunk } = useDeleteChunkByIds();
  const { changeChunkTextMode, textMode } = useChangeChunkTextMode();
  const selectAllChunk = useCallback(
    (checked: boolean) => {
      setSelectedChunkIds(checked ? data.map((x) => x.chunk_id) : []);
    },
    [data],
  );
  const showSelectedChunkWarning = useCallback(() => {
    message.warning(t('message.pleaseSelectChunk'));
  }, [t]);
  const { switchChunk } = useSwitchChunk();

  const [chunkList, setChunkList] = useState(data);
  useEffect(() => {
    setChunkList(data);
  }, [data]);
  const onPaginationChange: RAGFlowPaginationType['onChange'] = (
    page,
    size,
  ) => {
    setSelectedChunkIds([]);
    pagination.onChange?.(page, size);
  };

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
      if (ids?.length && resCode === 0) {
        chunkList.forEach((x: any) => {
          if (ids.indexOf(x['chunk_id']) > -1) {
            x['available_int'] = available;
          }
        });
        setChunkList(chunkList);
      }
    },
    [
      switchChunk,
      documentId,
      selectedChunkIds,
      showSelectedChunkWarning,
      chunkList,
    ],
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

  const handleChunkEditSave = (e: any) => {
    setIsChange(true);
    onChunkUpdatingOk(e);
  };

  const handleReRunFunc = () => {
    setIsChange(false);
  };
  return (
    <div className="w-full h-full">
      {isChange && (
        <div className=" absolute top-2 right-6">
          <RerunButton step={step} onRerun={handleReRunFunc} />
        </div>
      )}
      <div className={classNames('flex flex-col w-full')}>
        <Spin spinning={loading} className={styles.spin} size="large">
          <div className="h-[50px] flex flex-row justify-between items-end pb-[5px]">
            <div>
              <h2 className="text-[16px]">{t('chunk.chunkResult')}</h2>
              <div className="text-[12px] text-text-secondary italic">
                {t('chunk.chunkResultTip')}
              </div>
            </div>
            <ChunkResultBar
              handleInputChange={handleInputChange}
              searchString={searchString}
              changeChunkTextMode={changeChunkTextMode}
              createChunk={showChunkUpdatingModal}
              available={available}
              selectAllChunk={selectAllChunk}
              handleSetAvailable={handleSetAvailable}
            />
          </div>
          <div className=" rounded-[16px] box-border	mb-2">
            <div className="pt-[5px] pb-[5px]">
              <CheckboxSets
                selectAllChunk={selectAllChunk}
                switchChunk={handleSwitchChunk}
                removeChunk={handleRemoveChunk}
                checked={selectedChunkIds.length === data.length}
                selectedChunkIds={selectedChunkIds}
              />
            </div>
            <div className="h-[calc(100vh-280px)] overflow-y-auto pr-2 scrollbar-auto">
              <div
                className={classNames(
                  styles.chunkContainer,
                  {
                    [styles.chunkOtherContainer]: !isPdf,
                  },
                  'flex flex-col gap-4',
                )}
              >
                {chunkList.map((item) => (
                  <ChunkCard
                    item={item}
                    key={item.chunk_id}
                    editChunk={showChunkUpdatingModal}
                    checked={selectedChunkIds.some((x) => x === item.chunk_id)}
                    handleCheckboxClick={handleSingleCheckboxClick}
                    switchChunk={handleSwitchChunk}
                    clickChunkCard={handleChunkCardClick}
                    selected={item.chunk_id === selectedChunkId}
                    textMode={textMode}
                  ></ChunkCard>
                ))}
              </div>
            </div>
            <div className={styles.pageFooter}>
              <RAGFlowPagination
                pageSize={pagination.pageSize}
                current={pagination.current}
                total={total}
                onChange={(page, pageSize) => {
                  onPaginationChange(page, pageSize);
                }}
              ></RAGFlowPagination>
            </div>
          </div>
        </Spin>
      </div>
      {chunkUpdatingVisible && (
        <CreatingModal
          doc_id={documentId}
          chunkId={chunkId}
          hideModal={hideChunkUpdatingModal}
          visible={chunkUpdatingVisible}
          loading={chunkUpdatingLoading}
          onOk={(e) => {
            handleChunkEditSave(e);
          }}
          parserId={documentInfo.parser_id}
        />
      )}
    </div>
  );
};

export { ChunkerContainer };

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataflow-result/chunker.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 242 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (9)

- `ChunkerContainer()`: Function definition
- `selectAllChunk()`: Function definition
- `showSelectedChunkWarning()`: Function definition
- `handleSwitchChunk()`: Function definition
- `handleSingleCheckboxClick()`: Function definition
- `idx()`: Function definition
- `handleRemoveChunk()`: Function definition
- `handleChunkEditSave()`: Function definition
- `handleReRunFunc()`: Function definition

### Imports (15)

- `import { TimelineNode } from '@/components/originui/timeline';`
- `import message from '@/components/ui/message';`
- `import {`
- `import { Spin } from '@/components/ui/spin';`
- `import {`
- `import classNames from 'classnames';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import ChunkCard from './components/chunk-card';`
- `import CreatingModal from './components/chunk-creating-modal';`

## Code Structure Analysis

- Total lines: 242
- Blank lines: 9 (3.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~233


## Dependencies and Imports

- `@/components/originui/timeline`
- `@/components/ui/message`
- `@/components/ui/spin`
- `classnames`
- `react`
- `react-i18next`
- `./components/chunk-card`
- `./components/chunk-creating-modal`
- `./components/chunk-result-bar`
- `./components/chunk-result-bar/checkbox-sets`
- `./components/rerun-button`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataflow-result`.

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

- Other files in `web/src/pages/dataflow-result/` directory
- Potential test file: `test_chunker.tsx`

## Keywords

./components/chunk-card, ./components/chunk-creating-modal, ./components/chunk-result-bar, ./components/chunk-result-bar/checkbox-sets, ./components/rerun-button, ./index.less, @/components/originui/timeline, @/components/ui/message, @/components/ui/spin, CheckboxSets, ChunkCard, ChunkResultBar, ChunkerContainer, CreatingModal, IProps, RAGFlowPagination, RAGFlowPaginationType, RerunButton, Spin, TimelineNode, TypeScript, classnames, handleChunkEditSave, handleReRunFunc, handleRemoveChunk, handleSingleCheckboxClick, handleSwitchChunk, ids, idx, isPdf, nextIds, onPaginationChange, react, react-i18next, resCode, selectAllChunk, showSelectedChunkWarning

---
*Generated by RAGFlow Repository Documentation Generator*
