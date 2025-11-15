# Documentation: web/src/components/api-service/hooks.ts

## File Metadata

- **Path**: `web/src/components/api-service/hooks.ts`
- **Size**: 4579 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/api-service/hooks.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/api-service/hooks.ts` is located in the `web/src/components/api-service` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to api-service.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
