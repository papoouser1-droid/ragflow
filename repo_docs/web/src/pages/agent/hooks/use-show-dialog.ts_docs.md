# File Documentation: web/src/pages/agent/hooks/use-show-dialog.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-show-dialog.ts`
- **Extension**: `.ts`
- **Lines**: 92
- **Characters**: 2,580
- **Size**: 2,580 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useFetchTokenListBeforeOtherStep } from '@/components/embed-dialog/use-show-embed-dialog';
import { SharedFrom } from '@/constants/chat';
import { useShowDeleteConfirm } from '@/hooks/common-hooks';
import {
  useCreateSystemToken,
  useFetchSystemTokenList,
  useRemoveSystemToken,
} from '@/hooks/user-setting-hooks';
import { IStats } from '@/interfaces/database/chat';
import { useQueryClient } from '@tanstack/react-query';
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

const getUrlWithToken = (token: string, from: string = 'chat') => {
  const { protocol, host } = window.location;
  return `${protocol}//${host}/chat/share?shared_id=${token}&from=${from}`;
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

This file is part of the RAGFlow repository located at `web/src/pages/agent/hooks/use-show-dialog.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 92 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useOperateApiKey`: Exported entity
- `useSelectChartStatsList`: Exported entity
- `usePreviewChat`: Exported entity

### Functions (8)

- `useOperateApiKey()`: Function definition
- `onRemoveToken()`: Function definition
- `onCreateToken()`: Function definition
- `useSelectChartStatsList()`: Function definition
- `getUrlWithToken()`: Function definition
- `usePreviewChat()`: Function definition
- `open()`: Function definition
- `handlePreview()`: Function definition

### Imports (7)

- `import { useFetchTokenListBeforeOtherStep } from '@/components/embed-dialog/use-show-embed-dialog';`
- `import { SharedFrom } from '@/constants/chat';`
- `import { useShowDeleteConfirm } from '@/hooks/common-hooks';`
- `import {`
- `import { IStats } from '@/interfaces/database/chat';`
- `import { useQueryClient } from '@tanstack/react-query';`
- `import { useCallback } from 'react';`

## Code Structure Analysis

- Total lines: 92
- Blank lines: 14 (15.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~78


## Dependencies and Imports

- `@/components/embed-dialog/use-show-embed-dialog`
- `@/constants/chat`
- `@/hooks/common-hooks`
- `@/interfaces/database/chat`
- `@tanstack/react-query`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

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

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-show-dialog.ts`

## Keywords

@/components/embed-dialog/use-show-embed-dialog, @/constants/chat, @/hooks/common-hooks, @/interfaces/database/chat, @tanstack/react-query, Agent, Array, ChartStatsType, Chat, IStats, Object, SharedFrom, TypeScript, data, getUrlWithToken, handlePreview, item, onCreateToken, onRemoveToken, open, queryClient, react, showDeleteConfirm, stats, tanstack, token, useOperateApiKey, usePreviewChat, useSelectChartStatsList

---
*Generated by RAGFlow Repository Documentation Generator*
