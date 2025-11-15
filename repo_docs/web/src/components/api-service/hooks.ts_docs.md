# File Documentation: web/src/components/api-service/hooks.ts

## File Metadata

- **Path**: `web/src/components/api-service/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 180
- **Characters**: 4,579
- **Size**: 4,579 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { SharedFrom } from '@/constants/chat';
import {
  useSetModalState,
  useShowDeleteConfirm,
  useTranslate,
} from '@/hooks/common-hooks';
import {
  useCreateSystemToken,
  useFetchManualSystemTokenList,
  useFetchSystemTokenList,
  useRemoveSystemToken,
} from '@/hooks/user-setting-hooks';
import { IStats } from '@/interfaces/database/chat';
import { useQueryClient } from '@tanstack/react-query';
import { message } from 'antd';
import { useCallback } from 'react';

export const useOperateApiKey = (idKey: string, dialogId?: string) => {
  const { removeToken } = useRemoveSystemToken();
  const { createToken, loading: creatingLoading } = useCreateSystemToken();
  const { data: tokenList, loading: listLoading } = useFetchSystemTokenList();

  const showDeleteConfirm = useShowDeleteConfirm();

  const onRemoveToken = (token: string) => {
    showDeleteConfirm({
      onOk: () => removeToken(token),
    });
  };

  const onCreateToken = useCallback(() => {
    createToken({ [idKey]: dialogId });
  }, [createToken, idKey, dialogId]);

  return {
    removeToken: onRemoveToken,
    createToken: onCreateToken,
    tokenList,
    creatingLoading,
    listLoading,
  };
};

type ChartStatsType = {
  [k in keyof IStats]: Array<{ xAxis: string; yAxis: number }>;
};

export const useSelectChartStatsList = (): ChartStatsType => {
  const queryClient = useQueryClient();
  const data = queryClient.getQueriesData({ queryKey: ['fetchStats'] });
  const stats: IStats = (data.length > 0 ? data[0][1] : {}) as IStats;

  return Object.keys(stats).reduce((pre, cur) => {
    const item = stats[cur as keyof IStats];
    if (item.length > 0) {
      pre[cur as keyof IStats] = item.map((x) => ({
        xAxis: x[0] as string,
        yAxis: x[1] as number,
      }));
    }
    return pre;
  }, {} as ChartStatsType);
};

export const useShowTokenEmptyError = () => {
  const { t } = useTranslate('chat');

  const showTokenEmptyError = useCallback(() => {
    message.error(t('tokenError'));
  }, [t]);
  return { showTokenEmptyError };
};

export const useShowBetaEmptyError = () => {
  const { t } = useTranslate('chat');

  const showBetaEmptyError = useCallback(() => {
    message.error(t('betaError'));
  }, [t]);
  return { showBetaEmptyError };
};

const getUrlWithToken = (token: string, from: string = 'chat') => {
  const { protocol, host } = window.location;
  return `${protocol}//${host}/chat/share?shared_id=${token}&from=${from}`;
};

const useFetchTokenListBeforeOtherStep = () => {
  const { showTokenEmptyError } = useShowTokenEmptyError();
  const { showBetaEmptyError } = useShowBetaEmptyError();

  const { data: tokenList, fetchSystemTokenList } =
    useFetchManualSystemTokenList();

  let token = '',
    beta = '';

  if (Array.isArray(tokenList) && tokenList.length > 0) {
    token = tokenList[0].token;
    beta = tokenList[0].beta;
  }

  token =
    Array.isArray(tokenList) && tokenList.length > 0 ? tokenList[0].token : '';

  const handleOperate = useCallback(async () => {
    const ret = await fetchSystemTokenList();
    const list = ret;
    if (Array.isArray(list) && list.length > 0) {
      if (!list[0].beta) {
        showBetaEmptyError();
        return false;
      }
      return list[0]?.token;
    } else {
      showTokenEmptyError();
      return false;
    }
  }, [fetchSystemTokenList, showBetaEmptyError, showTokenEmptyError]);

  return {
    token,
    beta,
    handleOperate,
  };
};

export const useShowEmbedModal = () => {
  const {
    visible: embedVisible,
    hideModal: hideEmbedModal,
    showModal: showEmbedModal,
  } = useSetModalState();

  const { handleOperate, token, beta } = useFetchTokenListBeforeOtherStep();

  const handleShowEmbedModal = useCallback(async () => {
    const succeed = await handleOperate();
    if (succeed) {
      showEmbedModal();
    }
  }, [handleOperate, showEmbedModal]);

  return {
    showEmbedModal: handleShowEmbedModal,
    hideEmbedModal,
    embedVisible,
    embedToken: token,
    beta,
  };
};

export const usePreviewChat = (idKey: string) => {
  const { handleOperate } = useFetchTokenListBeforeOtherStep();

  const open = useCallback(
    (t: string) => {
      window.open(
        getUrlWithToken(
          t,
          idKey === 'canvasId' ? SharedFrom.Agent : SharedFrom.Chat,
        ),
        '_blank',
      );
    },
    [idKey],
  );

  const handlePreview = useCallback(async () => {
    const token = await handleOperate();
    if (token) {
      open(token);
    }
  }, [handleOperate, open]);

  return {
    handlePreview,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/api-service/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 180 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (6)

- `useOperateApiKey`: Exported entity
- `useSelectChartStatsList`: Exported entity
- `useShowTokenEmptyError`: Exported entity
- `useShowBetaEmptyError`: Exported entity
- `useShowEmbedModal`: Exported entity
- `usePreviewChat`: Exported entity

### Functions (16)

- `useOperateApiKey()`: Function definition
- `onRemoveToken()`: Function definition
- `onCreateToken()`: Function definition
- `useSelectChartStatsList()`: Function definition
- `useShowTokenEmptyError()`: Function definition
- `showTokenEmptyError()`: Function definition
- `useShowBetaEmptyError()`: Function definition
- `showBetaEmptyError()`: Function definition
- `getUrlWithToken()`: Function definition
- `useFetchTokenListBeforeOtherStep()`: Function definition
- `handleOperate()`: Function definition
- `useShowEmbedModal()`: Function definition
- `handleShowEmbedModal()`: Function definition
- `usePreviewChat()`: Function definition
- `open()`: Function definition
- `handlePreview()`: Function definition

### Imports (7)

- `import { SharedFrom } from '@/constants/chat';`
- `import {`
- `import {`
- `import { IStats } from '@/interfaces/database/chat';`
- `import { useQueryClient } from '@tanstack/react-query';`
- `import { message } from 'antd';`
- `import { useCallback } from 'react';`

## Code Structure Analysis

- Total lines: 180
- Blank lines: 29 (16.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~151


## Dependencies and Imports

- `@/constants/chat`
- `@/interfaces/database/chat`
- `@tanstack/react-query`
- `antd`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/api-service`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/api-service/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/constants/chat, @/interfaces/database/chat, @tanstack/react-query, Agent, Array, ChartStatsType, Chat, IStats, Object, SharedFrom, TypeScript, antd, data, getUrlWithToken, handleOperate, handlePreview, handleShowEmbedModal, item, list, onCreateToken, onRemoveToken, open, queryClient, react, ret, showBetaEmptyError, showDeleteConfirm, showTokenEmptyError, stats, succeed, tanstack, token, useFetchTokenListBeforeOtherStep, useOperateApiKey, usePreviewChat, useSelectChartStatsList, useShowBetaEmptyError, useShowEmbedModal, useShowTokenEmptyError

---
*Generated by RAGFlow Repository Documentation Generator*
