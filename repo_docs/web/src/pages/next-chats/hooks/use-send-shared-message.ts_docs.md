# File Documentation: web/src/pages/next-chats/hooks/use-send-shared-message.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-send-shared-message.ts`
- **Extension**: `.ts`
- **Lines**: 156
- **Characters**: 4,288
- **Size**: 4,288 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { MessageType, SharedFrom } from '@/constants/chat';
import { useCreateNextSharedConversation } from '@/hooks/chat-hooks';
import {
  useHandleMessageInputChange,
  useSelectDerivedMessages,
  useSendMessageWithSse,
} from '@/hooks/logic-hooks';
import { Message } from '@/interfaces/database/chat';
import { message } from 'antd';
import { get } from 'lodash';
import trim from 'lodash/trim';
import { useCallback, useEffect, useState } from 'react';
import { useSearchParams } from 'umi';
import { v4 as uuid } from 'uuid';

const isCompletionError = (res: any) =>
  res && (res?.response.status !== 200 || res?.data?.code !== 0);

export const useSendButtonDisabled = (value: string) => {
  return trim(value) === '';
};

export const useGetSharedChatSearchParams = () => {
  const [searchParams] = useSearchParams();
  const data_prefix = 'data_';
  const data = Object.fromEntries(
    searchParams
      .entries()
      .filter(([key]) => key.startsWith(data_prefix))
      .map(([key, value]) => [key.replace(data_prefix, ''), value]),
  );
  return {
    from: searchParams.get('from') as SharedFrom,
    sharedId: searchParams.get('shared_id'),
    locale: searchParams.get('locale'),
    data: data,
    visibleAvatar: searchParams.get('visible_avatar')
      ? searchParams.get('visible_avatar') !== '1'
      : true,
  };
};

export const useSendSharedMessage = () => {
  const {
    from,
    sharedId: conversationId,
    data: data,
  } = useGetSharedChatSearchParams();
  const { createSharedConversation: setConversation } =
    useCreateNextSharedConversation();
  const { handleInputChange, value, setValue } = useHandleMessageInputChange();
  const { send, answer, done, stopOutputMessage } = useSendMessageWithSse(
    `/api/v1/${from === SharedFrom.Agent ? 'agentbots' : 'chatbots'}/${conversationId}/completions`,
  );
  const {
    derivedMessages,
    removeLatestMessage,
    addNewestAnswer,
    addNewestQuestion,
    scrollRef,
    messageContainerRef,
    removeAllMessages,
    removeAllMessagesExceptFirst,
  } = useSelectDerivedMessages();
  const [hasError, setHasError] = useState(false);

  const sendMessage = useCallback(
    async (message: Message, id?: string) => {
      const res = await send({
        conversation_id: id ?? conversationId,
        quote: true,
        question: message.content,
        session_id: get(derivedMessages, '0.session_id'),
      });

      if (isCompletionError(res)) {
        // cancel loading
        setValue(message.content);
        removeLatestMessage();
      }
    },
    [send, conversationId, derivedMessages, setValue, removeLatestMessage],
  );

  const handleSendMessage = useCallback(
    async (message: Message) => {
      if (conversationId !== '') {
        sendMessage(message);
      } else {
        const data = await setConversation('user id');
        if (data.code === 0) {
          const id = data.data.id;
          sendMessage(message, id);
        }
      }
    },
    [conversationId, setConversation, sendMessage],
  );

  const fetchSessionId = useCallback(async () => {
    const payload = { question: '' };
    const ret = await send({ ...payload, ...data });
    if (isCompletionError(ret)) {
      message.error(ret?.data.message);
      setHasError(true);
    }
  }, [send]);

  useEffect(() => {
    fetchSessionId();
  }, [fetchSessionId]);

  useEffect(() => {
    if (answer.answer) {
      addNewestAnswer(answer);
    }
  }, [answer, addNewestAnswer]);

  const handlePressEnter = useCallback(
    (documentIds: string[]) => {
      if (trim(value) === '') return;
      const id = uuid();
      if (done) {
        setValue('');
        addNewestQuestion({
          content: value,
          doc_ids: documentIds,
          id,
          role: MessageType.User,
        });
        handleSendMessage({
          content: value.trim(),
          id,
          role: MessageType.User,
        });
      }
    },
    [addNewestQuestion, done, handleSendMessage, setValue, value],
  );

  return {
    handlePressEnter,
    handleInputChange,
    value,
    sendLoading: !done,
    loading: false,
    derivedMessages,
    hasError,
    stopOutputMessage,
    scrollRef,
    messageContainerRef,
    removeAllMessages,
    removeAllMessagesExceptFirst,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/hooks/use-send-shared-message.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 156 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useSendButtonDisabled`: Exported entity
- `useGetSharedChatSearchParams`: Exported entity
- `useSendSharedMessage`: Exported entity

### Functions (10)

- `isCompletionError()`: Function definition
- `useSendButtonDisabled()`: Function definition
- `useGetSharedChatSearchParams()`: Function definition
- `data()`: Function definition
- `useSendSharedMessage()`: Function definition
- `sendMessage()`: Function definition
- `handleSendMessage()`: Function definition
- `fetchSessionId()`: Function definition
- `ret()`: Function definition
- `handlePressEnter()`: Function definition

### Imports (10)

- `import { MessageType, SharedFrom } from '@/constants/chat';`
- `import { useCreateNextSharedConversation } from '@/hooks/chat-hooks';`
- `import {`
- `import { Message } from '@/interfaces/database/chat';`
- `import { message } from 'antd';`
- `import { get } from 'lodash';`
- `import trim from 'lodash/trim';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { useSearchParams } from 'umi';`
- `import { v4 as uuid } from 'uuid';`

## Code Structure Analysis

- Total lines: 156
- Blank lines: 13 (8.3%)
- Comment lines: ~1 (0.6%)
- Code lines: ~142


## Dependencies and Imports

- `@/constants/chat`
- `@/hooks/chat-hooks`
- `@/interfaces/database/chat`
- `antd`
- `lodash`
- `lodash/trim`
- `react`
- `umi`
- `uuid`

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
- Potential test file: `test_use-send-shared-message.ts`

## Keywords

@/constants/chat, @/hooks/chat-hooks, @/interfaces/database/chat, Agent, Message, MessageType, Object, SharedFrom, TypeScript, User, antd, data, data_prefix, fetchSessionId, handlePressEnter, handleSendMessage, id, isCompletionError, lodash, lodash/trim, payload, react, res, ret, sendMessage, umi, useGetSharedChatSearchParams, useSendButtonDisabled, useSendSharedMessage, uuid

---
*Generated by RAGFlow Repository Documentation Generator*
