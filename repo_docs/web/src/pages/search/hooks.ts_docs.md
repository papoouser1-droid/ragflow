# File Documentation: web/src/pages/search/hooks.ts

## File Metadata

- **Path**: `web/src/pages/search/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 279
- **Characters**: 6,713
- **Size**: 6,716 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useFetchRelatedQuestions } from '@/hooks/chat-hooks';
import { useSetModalState } from '@/hooks/common-hooks';
import {
  useTestChunkAllRetrieval,
  useTestChunkRetrieval,
} from '@/hooks/knowledge-hooks';
import {
  useGetPaginationWithRouter,
  useSendMessageWithSse,
} from '@/hooks/logic-hooks';
import { IAnswer } from '@/interfaces/database/chat';
import api from '@/utils/api';
import { get, isEmpty, isEqual, trim } from 'lodash';
import {
  ChangeEventHandler,
  useCallback,
  useEffect,
  useRef,
  useState,
} from 'react';
import {
  useGetSharedSearchParams,
  useSearchFetchMindMap,
} from '../next-search/hooks';

export const useSendQuestion = (kbIds: string[], tenantId?: string) => {
  const { sharedId } = useGetSharedSearchParams();
  const { send, answer, done, stopOutputMessage } = useSendMessageWithSse(
    sharedId ? api.askShare : api.ask,
  );

  const { testChunk, loading } = useTestChunkRetrieval(tenantId);
  const { testChunkAll } = useTestChunkAllRetrieval(tenantId);
  const [sendingLoading, setSendingLoading] = useState(false);
  const [currentAnswer, setCurrentAnswer] = useState({} as IAnswer);
  const { fetchRelatedQuestions, data: relatedQuestions } =
    useFetchRelatedQuestions(tenantId);
  const [searchStr, setSearchStr] = useState<string>('');
  const [isFirstRender, setIsFirstRender] = useState(true);
  const [selectedDocumentIds, setSelectedDocumentIds] = useState<string[]>([]);

  const { pagination, setPagination } = useGetPaginationWithRouter();

  const sendQuestion = useCallback(
    (question: string) => {
      const q = trim(question);
      if (isEmpty(q)) return;
      setPagination({ page: 1 });
      setIsFirstRender(false);
      setCurrentAnswer({} as IAnswer);
      setSendingLoading(true);
      send({ kb_ids: kbIds, question: q, tenantId });
      testChunk({
        kb_id: kbIds,
        highlight: true,
        question: q,
        page: 1,
        size: pagination.pageSize,
      });

      fetchRelatedQuestions(q);
    },
    [
      send,
      testChunk,
      kbIds,
      fetchRelatedQuestions,
      setPagination,
      pagination.pageSize,
      tenantId,
    ],
  );

  const handleSearchStrChange: ChangeEventHandler<HTMLInputElement> =
    useCallback((e) => {
      setSearchStr(e.target.value);
    }, []);

  const handleClickRelatedQuestion = useCallback(
    (question: string) => () => {
      if (sendingLoading) return;

      setSearchStr(question);
      sendQuestion(question);
    },
    [sendQuestion, sendingLoading],
  );

  const handleTestChunk = useCallback(
    (documentIds: string[], page: number = 1, size: number = 10) => {
      const q = trim(searchStr);
      if (sendingLoading || isEmpty(q)) return;

      testChunk({
        kb_id: kbIds,
        highlight: true,
        question: q,
        doc_ids: documentIds ?? selectedDocumentIds,
        page,
        size,
      });

      testChunkAll({
        kb_id: kbIds,
        highlight: true,
        question: q,
        doc_ids: [],
        page,
        size,
      });
    },
    [
      searchStr,
      sendingLoading,
      testChunk,
      kbIds,
      selectedDocumentIds,
      testChunkAll,
    ],
  );

  useEffect(() => {
    if (!isEmpty(answer)) {
      setCurrentAnswer(answer);
    }
  }, [answer]);

  useEffect(() => {
    if (done) {
      setSendingLoading(false);
    }
  }, [done]);

  return {
    sendQuestion,
    handleSearchStrChange,
    handleClickRelatedQuestion,
    handleTestChunk,
    setSelectedDocumentIds,
    loading,
    sendingLoading,
    answer: currentAnswer,
    relatedQuestions: relatedQuestions?.slice(0, 5) ?? [],
    searchStr,
    setSearchStr,
    isFirstRender,
    selectedDocumentIds,
    isSearchStrEmpty: isEmpty(trim(searchStr)),
    stopOutputMessage,
  };
};

export const useFetchBackgroundImage = () => {
  const [imgUrl, setImgUrl] = useState<string>('');

  const fetchImage = useCallback(async () => {
    try {
      const res = await fetch(
        '/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=zh-CN',
      );
      const ret = await res.json();
      const url = get(ret, 'images.0.url');
      if (url) {
        setImgUrl(url);
      }
    } catch (error) {
      console.log('🚀 ~ fetchImage ~ error:', error);
    }
  }, []);

  useEffect(() => {
    fetchImage();
  }, [fetchImage]);

  return `https://cn.bing.com${imgUrl}`;
};

export const useTestRetrieval = (
  kbIds: string[],
  searchStr: string,
  sendingLoading: boolean,
) => {
  const { testChunk, loading } = useTestChunkRetrieval();
  const { pagination } = useGetPaginationWithRouter();

  const [selectedDocumentIds, setSelectedDocumentIds] = useState<string[]>([]);

  const handleTestChunk = useCallback(() => {
    const q = trim(searchStr);
    if (sendingLoading || isEmpty(q)) return;

    testChunk({
      kb_id: kbIds,
      highlight: true,
      question: q,
      doc_ids: Array.isArray(selectedDocumentIds) ? selectedDocumentIds : [],
      page: pagination.current,
      size: pagination.pageSize,
    });
  }, [
    sendingLoading,
    searchStr,
    kbIds,
    testChunk,
    selectedDocumentIds,
    pagination,
  ]);

  useEffect(() => {
    handleTestChunk();
  }, [handleTestChunk]);

  return {
    loading,
    selectedDocumentIds,
    setSelectedDocumentIds,
  };
};

export const useShowMindMapDrawer = (
  kbIds: string[],
  question: string,
  searchId = '',
) => {
  const { visible, showModal, hideModal } = useSetModalState();
  const ref = useRef<any>();

  const {
    fetchMindMap,
    data: mindMap,
    loading: mindMapLoading,
  } = useSearchFetchMindMap();

  const handleShowModal = useCallback(() => {
    const searchParams = { question: trim(question), kb_ids: kbIds, searchId };
    if (
      !isEmpty(searchParams.question) &&
      !isEqual(searchParams, ref.current)
    ) {
      ref.current = searchParams;
      fetchMindMap(searchParams);
    }
    showModal();
  }, [fetchMindMap, showModal, question, kbIds, searchId]);

  return {
    mindMap,
    mindMapVisible: visible,
    mindMapLoading,
    showMindMapModal: handleShowModal,
    hideMindMapModal: hideModal,
  };
};

export const usePendingMindMap = () => {
  const [count, setCount] = useState<number>(0);
  const ref = useRef<NodeJS.Timeout>();

  const setCountInterval = useCallback(() => {
    ref.current = setInterval(() => {
      setCount((pre) => {
        if (pre > 40) {
          clearInterval(ref?.current);
        }
        return pre + 1;
      });
    }, 1000);
  }, []);

  useEffect(() => {
    setCountInterval();
    return () => {
      clearInterval(ref?.current);
    };
  }, [setCountInterval]);

  return Number(((count / 43) * 100).toFixed(0));
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/search/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 279 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `useSendQuestion`: Exported entity
- `useFetchBackgroundImage`: Exported entity
- `useTestRetrieval`: Exported entity
- `useShowMindMapDrawer`: Exported entity
- `usePendingMindMap`: Exported entity

### Functions (14)

- `useSendQuestion()`: Function definition
- `sendQuestion()`: Function definition
- `handleClickRelatedQuestion()`: Function definition
- `q()`: Function definition
- `useFetchBackgroundImage()`: Function definition
- `fetchImage()`: Function definition
- `url()`: Function definition
- `useTestRetrieval()`: Function definition
- `handleTestChunk()`: Function definition
- `q()`: Function definition
- `useShowMindMapDrawer()`: Function definition
- `handleShowModal()`: Function definition
- `usePendingMindMap()`: Function definition
- `setCountInterval()`: Function definition

### Imports (9)

- `import { useFetchRelatedQuestions } from '@/hooks/chat-hooks';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import {`
- `import {`
- `import { IAnswer } from '@/interfaces/database/chat';`
- `import api from '@/utils/api';`
- `import { get, isEmpty, isEqual, trim } from 'lodash';`
- `import {`
- `import {`

## Code Structure Analysis

- Total lines: 279
- Blank lines: 33 (11.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~246


## Dependencies and Imports

- `@/hooks/chat-hooks`
- `@/hooks/common-hooks`
- `@/interfaces/database/chat`
- `@/utils/api`
- `lodash`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/search`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Code Execution**: Avoid eval/exec with user input - potential code injection

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/search/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/hooks/chat-hooks, @/hooks/common-hooks, @/interfaces/database/chat, @/utils/api, Array, ChangeEventHandler, HPImageArchive, HTMLInputElement, IAnswer, NodeJS, Number, Timeout, TypeScript, fetchImage, handleClickRelatedQuestion, handleSearchStrChange, handleShowModal, handleTestChunk, lodash, q, ref, res, ret, searchParams, sendQuestion, setCountInterval, url, useFetchBackgroundImage, usePendingMindMap, useSendQuestion, useShowMindMapDrawer, useTestRetrieval

---
*Generated by RAGFlow Repository Documentation Generator*
