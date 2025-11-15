# Documentation: web/src/pages/chat/shared-hooks.ts

## File Metadata

- **Path**: `web/src/pages/chat/shared-hooks.ts`
- **Size**: 4142 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chat/shared-hooks.ts`.

## Original Source Code

```ts
import { MessageType, SharedFrom } from '@/constants/chat';
import { useCreateNextSharedConversation } from '@/hooks/chat-hooks';
import {
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
import { useHandleMessageInputChange } from './hooks';

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
    ref,
    removeLatestMessage,
    addNewestAnswer,
    addNewestQuestion,
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
  }, [fetchSessionId, send]);

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
    ref,
    loading: false,
    derivedMessages,
    hasError,
    stopOutputMessage,
  };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chat/shared-hooks.ts` is located in the `web/src/pages/chat` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chat.

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

- [constants.ts](constants.ts_docs.md)
- [context.ts](context.ts_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [interface.ts](interface.ts_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
