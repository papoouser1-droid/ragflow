# File Documentation: web/src/pages/next-chats/hooks/use-select-conversation-list.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-select-conversation-list.ts`
- **Extension**: `.ts`
- **Lines**: 97
- **Characters**: 2,869
- **Size**: 2,869 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { ChatSearchParams, MessageType } from '@/constants/chat';
import { useTranslate } from '@/hooks/common-hooks';
import {
  useFetchConversationList,
  useFetchDialogList,
} from '@/hooks/use-chat-request';
import { IConversation } from '@/interfaces/database/chat';
import { getConversationId } from '@/utils/chat';
import { useCallback, useEffect, useMemo, useState } from 'react';
import { useParams, useSearchParams } from 'umi';

export const useFindPrologueFromDialogList = () => {
  const { id: dialogId } = useParams();
  const { data } = useFetchDialogList();

  const prologue = useMemo(() => {
    return data.dialogs.find((x) => x.id === dialogId)?.prompt_config.prologue;
  }, [dialogId, data]);

  return prologue;
};

export const useSetNewConversationRouteParams = () => {
  const [currentQueryParameters, setSearchParams] = useSearchParams();
  const newQueryParameters: URLSearchParams = useMemo(
    () => new URLSearchParams(currentQueryParameters.toString()),
    [currentQueryParameters],
  );

  const setNewConversationRouteParams = useCallback(
    (conversationId: string, isNew: string) => {
      newQueryParameters.set(ChatSearchParams.ConversationId, conversationId);
      newQueryParameters.set(ChatSearchParams.isNew, isNew);
      setSearchParams(newQueryParameters);
    },
    [newQueryParameters, setSearchParams],
  );

  return { setNewConversationRouteParams };
};

export const useSelectDerivedConversationList = () => {
  const { t } = useTranslate('chat');

  const [list, setList] = useState<Array<IConversation>>([]);
  const {
    data: conversationList,
    loading,
    handleInputChange,
    searchString,
  } = useFetchConversationList();
  const { id: dialogId } = useParams();
  const { setNewConversationRouteParams } = useSetNewConversationRouteParams();
  const prologue = useFindPrologueFromDialogList();

  const addTemporaryConversation = useCallback(() => {
    const conversationId = getConversationId();
    setList((pre) => {
      if (dialogId) {
        setNewConversationRouteParams(conversationId, 'true');
        const nextList = [
          {
            id: conversationId,
            name: t('newConversation'),
            dialog_id: dialogId,
            is_new: true,
            message: [
              {
                content: prologue,
                role: MessageType.Assistant,
              },
            ],
          } as any,
          ...conversationList,
        ];
        return nextList;
      }

      return pre;
    });
  }, [conversationList, dialogId, prologue, t, setNewConversationRouteParams]);

  // When you first enter the page, select the top conversation card

  useEffect(() => {
    setList([...conversationList]);
  }, [conversationList]);

  return {
    list,
    addTemporaryConversation,
    loading,
    handleInputChange,
    searchString,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/hooks/use-select-conversation-list.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 97 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useFindPrologueFromDialogList`: Exported entity
- `useSetNewConversationRouteParams`: Exported entity
- `useSelectDerivedConversationList`: Exported entity

### Functions (8)

- `useFindPrologueFromDialogList()`: Function definition
- `prologue()`: Function definition
- `useSetNewConversationRouteParams()`: Function definition
- `setNewConversationRouteParams()`: Function definition
- `useSelectDerivedConversationList()`: Function definition
- `addTemporaryConversation()`: Function definition
- `conversationId()`: Function definition
- `nextList()`: Function definition

### Imports (7)

- `import { ChatSearchParams, MessageType } from '@/constants/chat';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import { IConversation } from '@/interfaces/database/chat';`
- `import { getConversationId } from '@/utils/chat';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`
- `import { useParams, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 97
- Blank lines: 14 (14.4%)
- Comment lines: ~1 (1.0%)
- Code lines: ~82


## Dependencies and Imports

- `@/constants/chat`
- `@/hooks/common-hooks`
- `@/interfaces/database/chat`
- `@/utils/chat`
- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

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
- Potential test file: `test_use-select-conversation-list.ts`

## Keywords

@/constants/chat, @/hooks/common-hooks, @/interfaces/database/chat, @/utils/chat, Array, Assistant, ChatSearchParams, ConversationId, IConversation, MessageType, TypeScript, URLSearchParams, When, addTemporaryConversation, conversationId, newQueryParameters, nextList, prologue, react, setNewConversationRouteParams, umi, useFindPrologueFromDialogList, useSelectDerivedConversationList, useSetNewConversationRouteParams

---
*Generated by RAGFlow Repository Documentation Generator*
