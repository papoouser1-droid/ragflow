# Documentation: web/src/pages/dataflow-result/chunker.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/chunker.tsx`
- **Size**: 7670 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataflow-result/chunker.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataflow-result/chunker.tsx` is located in the `web/src/pages/dataflow-result` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataflow-result.

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
- [index.tsx](index.tsx_docs.md)
- [interface.ts](interface.ts_docs.md)
- [parser.tsx](parser.tsx_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
