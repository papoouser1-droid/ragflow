# File Documentation: web/src/components/embed-dialog/use-show-embed-dialog.ts

## File Metadata

- **Path**: `web/src/components/embed-dialog/use-show-embed-dialog.ts`
- **Extension**: `.ts`
- **Lines**: 88
- **Characters**: 2,223
- **Size**: 2,223 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState, useTranslate } from '@/hooks/common-hooks';
import { useFetchManualSystemTokenList } from '@/hooks/user-setting-hooks';
import { useCallback } from 'react';
import message from '../ui/message';

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

export const useFetchTokenListBeforeOtherStep = () => {
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

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/embed-dialog/use-show-embed-dialog.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 88 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `useShowTokenEmptyError`: Exported entity
- `useShowBetaEmptyError`: Exported entity
- `useFetchTokenListBeforeOtherStep`: Exported entity
- `useShowEmbedModal`: Exported entity

### Functions (8)

- `useShowTokenEmptyError()`: Function definition
- `showTokenEmptyError()`: Function definition
- `useShowBetaEmptyError()`: Function definition
- `showBetaEmptyError()`: Function definition
- `useFetchTokenListBeforeOtherStep()`: Function definition
- `handleOperate()`: Function definition
- `useShowEmbedModal()`: Function definition
- `handleShowEmbedModal()`: Function definition

### Imports (4)

- `import { useSetModalState, useTranslate } from '@/hooks/common-hooks';`
- `import { useFetchManualSystemTokenList } from '@/hooks/user-setting-hooks';`
- `import { useCallback } from 'react';`
- `import message from '../ui/message';`

## Code Structure Analysis

- Total lines: 88
- Blank lines: 16 (18.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~72


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/hooks/user-setting-hooks`
- `react`
- `../ui/message`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/embed-dialog`.

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

- Other files in `web/src/components/embed-dialog/` directory
- Potential test file: `test_use-show-embed-dialog.ts`

## Keywords

../ui/message, @/hooks/common-hooks, @/hooks/user-setting-hooks, Array, TypeScript, handleOperate, handleShowEmbedModal, list, react, ret, showBetaEmptyError, showTokenEmptyError, succeed, token, useFetchTokenListBeforeOtherStep, useShowBetaEmptyError, useShowEmbedModal, useShowTokenEmptyError

---
*Generated by RAGFlow Repository Documentation Generator*
