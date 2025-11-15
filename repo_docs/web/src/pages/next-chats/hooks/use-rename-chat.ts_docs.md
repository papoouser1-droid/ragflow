# File Documentation: web/src/pages/next-chats/hooks/use-rename-chat.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-rename-chat.ts`
- **Extension**: `.ts`
- **Lines**: 94
- **Characters**: 2,545
- **Size**: 2,545 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState } from '@/hooks/common-hooks';
import { useSetDialog } from '@/hooks/use-chat-request';
import { useFetchTenantInfo } from '@/hooks/use-user-setting-request';
import { IDialog } from '@/interfaces/database/chat';
import { isEmpty, omit } from 'lodash';
import { useCallback, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';

export const useRenameChat = () => {
  const [chat, setChat] = useState<IDialog>({} as IDialog);
  const {
    visible: chatRenameVisible,
    hideModal: hideChatRenameModal,
    showModal: showChatRenameModal,
  } = useSetModalState();
  const { setDialog, loading } = useSetDialog();
  const { t } = useTranslation();
  const tenantInfo = useFetchTenantInfo();

  const InitialData = useMemo(
    () => ({
      name: '',
      icon: '',
      language: 'English',
      description: '',
      prompt_config: {
        empty_response: '',
        prologue: t('chat.setAnOpenerInitial'),
        quote: true,
        keyword: false,
        tts: false,
        system: t('chat.systemInitialValue'),
        refine_multiturn: false,
        use_kg: false,
        reasoning: false,
        parameters: [{ key: 'knowledge', optional: false }],
        toc_enhance: false,
      },
      llm_id: tenantInfo.data.llm_id,
      llm_setting: {},
      similarity_threshold: 0.2,
      vector_similarity_weight: 0.3,
      top_n: 8,
    }),
    [t, tenantInfo.data.llm_id],
  );

  const onChatRenameOk = useCallback(
    async (name: string) => {
      const nextChat = {
        ...(isEmpty(chat)
          ? InitialData
          : {
              ...omit(chat, 'nickname', 'tenant_avatar', 'operator_permission'),
              dialog_id: chat.id,
            }),
        name,
      };
      const ret = await setDialog(nextChat);

      if (ret === 0) {
        hideChatRenameModal();
      }
    },
    [chat, InitialData, setDialog, hideChatRenameModal],
  );

  const handleShowChatRenameModal = useCallback(
    (record?: IDialog) => {
      if (record) {
        setChat(record);
      } else {
        setChat({} as IDialog);
      }
      showChatRenameModal();
    },
    [showChatRenameModal],
  );

  const handleHideModal = useCallback(() => {
    hideChatRenameModal();
    setChat({} as IDialog);
  }, [hideChatRenameModal]);

  return {
    chatRenameLoading: loading,
    initialChatName: chat?.name,
    onChatRenameOk,
    chatRenameVisible,
    hideChatRenameModal: handleHideModal,
    showChatRenameModal: handleShowChatRenameModal,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/hooks/use-rename-chat.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 94 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useRenameChat`: Exported entity

### Functions (5)

- `useRenameChat()`: Function definition
- `InitialData()`: Function definition
- `onChatRenameOk()`: Function definition
- `handleShowChatRenameModal()`: Function definition
- `handleHideModal()`: Function definition

### Imports (7)

- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useSetDialog } from '@/hooks/use-chat-request';`
- `import { useFetchTenantInfo } from '@/hooks/use-user-setting-request';`
- `import { IDialog } from '@/interfaces/database/chat';`
- `import { isEmpty, omit } from 'lodash';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 8 (8.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~86


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/hooks/use-chat-request`
- `@/hooks/use-user-setting-request`
- `@/interfaces/database/chat`
- `lodash`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats/hooks`.

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

- Other files in `web/src/pages/next-chats/hooks/` directory
- Potential test file: `test_use-rename-chat.ts`

## Keywords

@/hooks/common-hooks, @/hooks/use-chat-request, @/hooks/use-user-setting-request, @/interfaces/database/chat, English, IDialog, InitialData, TypeScript, handleHideModal, handleShowChatRenameModal, lodash, nextChat, onChatRenameOk, react, react-i18next, ret, tenantInfo, useRenameChat

---
*Generated by RAGFlow Repository Documentation Generator*
