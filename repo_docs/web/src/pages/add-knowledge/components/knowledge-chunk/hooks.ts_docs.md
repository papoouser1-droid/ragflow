# File Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/hooks.ts

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 130
- **Characters**: 3,693
- **Size**: 3,693 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  useCreateChunk,
  useDeleteChunk,
  useSelectChunkList,
} from '@/hooks/chunk-hooks';
import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';
import { useGetKnowledgeSearchParams } from '@/hooks/route-hook';
import { IChunk } from '@/interfaces/database/knowledge';
import { buildChunkHighlights } from '@/utils/document-util';
import { useCallback, useMemo, useState } from 'react';
import { IHighlight } from 'react-pdf-highlighter';
import { ChunkTextMode } from './constant';

export const useHandleChunkCardClick = () => {
  const [selectedChunkId, setSelectedChunkId] = useState<string>('');

  const handleChunkCardClick = useCallback((chunkId: string) => {
    setSelectedChunkId(chunkId);
  }, []);

  return { handleChunkCardClick, selectedChunkId };
};

export const useGetSelectedChunk = (selectedChunkId: string) => {
  const data = useSelectChunkList();
  return (
    data?.data?.find((x) => x.chunk_id === selectedChunkId) ?? ({} as IChunk)
  );
};

export const useGetChunkHighlights = (selectedChunkId: string) => {
  const [size, setSize] = useState({ width: 849, height: 1200 });
  const selectedChunk: IChunk = useGetSelectedChunk(selectedChunkId);

  const highlights: IHighlight[] = useMemo(() => {
    return buildChunkHighlights(selectedChunk, size);
  }, [selectedChunk, size]);

  const setWidthAndHeight = useCallback((width: number, height: number) => {
    setSize((pre) => {
      if (pre.height !== height || pre.width !== width) {
        return { height, width };
      }
      return pre;
    });
  }, []);

  return { highlights, setWidthAndHeight };
};

// Switch chunk text to be fully displayed or ellipse
export const useChangeChunkTextMode = () => {
  const [textMode, setTextMode] = useState<ChunkTextMode>(ChunkTextMode.Full);

  const changeChunkTextMode = useCallback((mode: ChunkTextMode) => {
    setTextMode(mode);
  }, []);

  return { textMode, changeChunkTextMode };
};

export const useDeleteChunkByIds = (): {
  removeChunk: (chunkIds: string[], documentId: string) => Promise<number>;
} => {
  const { deleteChunk } = useDeleteChunk();
  const showDeleteConfirm = useShowDeleteConfirm();

  const removeChunk = useCallback(
    (chunkIds: string[], documentId: string) => () => {
      return deleteChunk({ chunkIds, doc_id: documentId });
    },
    [deleteChunk],
  );

  const onRemoveChunk = useCallback(
    (chunkIds: string[], documentId: string): Promise<number> => {
      return showDeleteConfirm({ onOk: removeChunk(chunkIds, documentId) });
    },
    [removeChunk, showDeleteConfirm],
  );

  return {
    removeChunk: onRemoveChunk,
  };
};

export const useUpdateChunk = () => {
  const [chunkId, setChunkId] = useState<string | undefined>('');
  const {
    visible: chunkUpdatingVisible,
    hideModal: hideChunkUpdatingModal,
    showModal,
  } = useSetModalState();
  const { createChunk, loading } = useCreateChunk();
  const { documentId } = useGetKnowledgeSearchParams();

  const onChunkUpdatingOk = useCallback(
    async (params: IChunk) => {
      const code = await createChunk({
        ...params,
        doc_id: documentId,
        chunk_id: chunkId,
      });

      if (code === 0) {
        hideChunkUpdatingModal();
      }
    },
    [createChunk, hideChunkUpdatingModal, chunkId, documentId],
  );

  const handleShowChunkUpdatingModal = useCallback(
    async (id?: string) => {
      setChunkId(id);
      showModal();
    },
    [showModal],
  );

  return {
    chunkUpdatingLoading: loading,
    onChunkUpdatingOk,
    chunkUpdatingVisible,
    hideChunkUpdatingModal,
    showChunkUpdatingModal: handleShowChunkUpdatingModal,
    chunkId,
    documentId,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-chunk/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 130 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (6)

- `useHandleChunkCardClick`: Exported entity
- `useGetSelectedChunk`: Exported entity
- `useGetChunkHighlights`: Exported entity
- `useChangeChunkTextMode`: Exported entity
- `useDeleteChunkByIds`: Exported entity
- `useUpdateChunk`: Exported entity

### Functions (14)

- `useHandleChunkCardClick()`: Function definition
- `handleChunkCardClick()`: Function definition
- `useGetSelectedChunk()`: Function definition
- `data()`: Function definition
- `useGetChunkHighlights()`: Function definition
- `setWidthAndHeight()`: Function definition
- `useChangeChunkTextMode()`: Function definition
- `changeChunkTextMode()`: Function definition
- `useDeleteChunkByIds()`: Function definition
- `removeChunk()`: Function definition
- `onRemoveChunk()`: Function definition
- `useUpdateChunk()`: Function definition
- `onChunkUpdatingOk()`: Function definition
- `handleShowChunkUpdatingModal()`: Function definition

### Imports (8)

- `import {`
- `import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';`
- `import { useGetKnowledgeSearchParams } from '@/hooks/route-hook';`
- `import { IChunk } from '@/interfaces/database/knowledge';`
- `import { buildChunkHighlights } from '@/utils/document-util';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { IHighlight } from 'react-pdf-highlighter';`
- `import { ChunkTextMode } from './constant';`

## Code Structure Analysis

- Total lines: 130
- Blank lines: 21 (16.2%)
- Comment lines: ~1 (0.8%)
- Code lines: ~108


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/hooks/route-hook`
- `@/interfaces/database/knowledge`
- `@/utils/document-util`
- `react`
- `react-pdf-highlighter`
- `./constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-chunk`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/add-knowledge/components/knowledge-chunk/` directory
- Potential test file: `test_hooks.ts`

## Keywords

./constant, @/hooks/common-hooks, @/hooks/route-hook, @/interfaces/database/knowledge, @/utils/document-util, ChunkTextMode, Full, IChunk, IHighlight, Promise, Switch, TypeScript, changeChunkTextMode, code, data, handleChunkCardClick, handleShowChunkUpdatingModal, highlights, onChunkUpdatingOk, onRemoveChunk, react, react-pdf-highlighter, removeChunk, selectedChunk, setWidthAndHeight, showDeleteConfirm, useChangeChunkTextMode, useDeleteChunkByIds, useGetChunkHighlights, useGetSelectedChunk, useHandleChunkCardClick, useUpdateChunk

---
*Generated by RAGFlow Repository Documentation Generator*
