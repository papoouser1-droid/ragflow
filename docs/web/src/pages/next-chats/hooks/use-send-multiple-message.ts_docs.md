# Documentation: web/src/pages/next-chats/hooks/use-send-multiple-message.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-send-multiple-message.ts`
- **Size**: 6531 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/next-chats/hooks/use-send-multiple-message.ts`.

## Original Source Code

```ts
import showMessage from '@/components/ui/message';
import { MessageType } from '@/constants/chat';
import {
  useHandleMessageInputChange,
  useSendMessageWithSse,
} from '@/hooks/logic-hooks';
import { useGetChatSearchParams } from '@/hooks/use-chat-request';
import { IAnswer, Message } from '@/interfaces/database/chat';
import api from '@/utils/api';
import { buildMessageUuid } from '@/utils/chat';
import { trim } from 'lodash';
import { useCallback, useEffect, useState } from 'react';
import { v4 as uuid } from 'uuid';
import { IMessage } from '../chat/interface';
import { useBuildFormRefs } from './use-build-form-refs';
import { useUploadFile } from './use-upload-file';

export function useSendMultipleChatMessage(
  controller: AbortController,
  chatBoxIds: string[],
) {
  const [messageRecord, setMessageRecord] = useState<
    Record<string, IMessage[]>
  >({});

  const { conversationId } = useGetChatSearchParams();

  const { handleInputChange, value, setValue } = useHandleMessageInputChange();
  const { send, answer, allDone } = useSendMessageWithSse(
    api.completeConversation,
  );

  const { handleUploadFile, fileIds, clearFileIds } = useUploadFile();

  const { setFormRef, getLLMConfigById, isLLMConfigEmpty } =
    useBuildFormRefs(chatBoxIds);

  const addNewestQuestion = useCallback(
    (message: Message, answer: string = '') => {
      setMessageRecord((pre) => {
        const currentRecord = { ...pre };
        const chatBoxId = message.chatBoxId;
        if (typeof chatBoxId === 'string') {
          const currentChatMessages = currentRecord[chatBoxId];

          const nextChatMessages = [
            ...currentChatMessages,
            {
              ...message,
              id: buildMessageUuid(message), // The message id is generated on the front end,
              // and the message id returned by the back end is the same as the question id,
              //  so that the pair of messages can be deleted together when deleting the message
            },
            {
              role: MessageType.Assistant,
              content: answer,
              id: buildMessageUuid({ ...message, role: MessageType.Assistant }),
            },
          ];

          currentRecord[chatBoxId] = nextChatMessages;
        }

        return currentRecord;
      });
    },
    [],
  );

  // Add the streaming message to the last item in the message list
  const addNewestAnswer = useCallback((answer: IAnswer) => {
    setMessageRecord((pre) => {
      const currentRecord = { ...pre };
      const chatBoxId = answer.chatBoxId;
      if (typeof chatBoxId === 'string') {
        const currentChatMessages = currentRecord[chatBoxId];

        const nextChatMessages = [
          ...(currentChatMessages?.slice(0, -1) ?? []),
          {
            role: MessageType.Assistant,
            content: answer.answer,
            reference: answer.reference,
            id: buildMessageUuid({
              id: answer.id,
              role: MessageType.Assistant,
            }),
            prompt: answer.prompt,
            audio_binary: answer.audio_binary,
          },
        ];

        currentRecord[chatBoxId] = nextChatMessages;
      }

      return currentRecord;
    });
  }, []);

  const removeLatestMessage = useCallback((chatBoxId?: string) => {
    setMessageRecord((pre) => {
      const currentRecord = { ...pre };
      if (chatBoxId) {
        const currentChatMessages = currentRecord[chatBoxId];
        if (currentChatMessages) {
          currentRecord[chatBoxId] = currentChatMessages.slice(0, -1);
        }
      }
      return currentRecord;
    });
  }, []);

  const adjustRecordByChatBoxIds = useCallback(() => {
    setMessageRecord((pre) => {
      const currentRecord = { ...pre };
      chatBoxIds.forEach((chatBoxId) => {
        if (!currentRecord[chatBoxId]) {
          currentRecord[chatBoxId] = [];
        }
      });
      Object.keys(currentRecord).forEach((chatBoxId) => {
        if (!chatBoxIds.includes(chatBoxId)) {
          delete currentRecord[chatBoxId];
        }
      });
      return currentRecord;
    });
  }, [chatBoxIds, setMessageRecord]);

  const sendMessage = useCallback(
    async ({
      message,
      currentConversationId,
      messages,
      chatBoxId,
    }: {
      message: Message;
      currentConversationId?: string;
      chatBoxId: string;
      messages?: Message[];
    }) => {
      let derivedMessages: IMessage[] = [];

      derivedMessages = messageRecord[chatBoxId];

      const res = await send(
        {
          chatBoxId,
          conversation_id: currentConversationId ?? conversationId,
          messages: [...(messages ?? derivedMessages ?? []), message],
          ...getLLMConfigById(chatBoxId),
        },
        controller,
      );

      if (res && (res?.response.status !== 200 || res?.data?.code !== 0)) {
        // cancel loading
        setValue(message.content);
        showMessage.error(res.data.message);
        removeLatestMessage(chatBoxId);
      }
    },
    [
      send,
      conversationId,
      getLLMConfigById,
      controller,
      messageRecord,
      setValue,
      removeLatestMessage,
    ],
  );

  const handlePressEnter = useCallback(() => {
    if (trim(value) === '') return;
    const id = uuid();

    chatBoxIds.forEach((chatBoxId) => {
      if (!isLLMConfigEmpty(chatBoxId)) {
        addNewestQuestion({
          content: value,
          id,
          role: MessageType.User,
          chatBoxId,
          doc_ids: fileIds,
        });
      }
    });

    if (allDone) {
      setValue('');
      chatBoxIds.forEach((chatBoxId) => {
        if (!isLLMConfigEmpty(chatBoxId)) {
          sendMessage({
            message: {
              id,
              content: value.trim(),
              role: MessageType.User,
              doc_ids: fileIds,
            },
            chatBoxId,
          });
        }
      });
    }
    clearFileIds();
  }, [
    value,
    chatBoxIds,
    allDone,
    clearFileIds,
    isLLMConfigEmpty,
    addNewestQuestion,
    fileIds,
    setValue,
    sendMessage,
  ]);

  useEffect(() => {
    if (answer.answer && conversationId) {
      addNewestAnswer(answer);
    }
  }, [answer, addNewestAnswer, conversationId]);

  useEffect(() => {
    adjustRecordByChatBoxIds();
  }, [adjustRecordByChatBoxIds]);

  return {
    value,
    messageRecord,
    sendMessage,
    handleInputChange,
    handlePressEnter,
    sendLoading: !allDone,
    setFormRef,
    handleUploadFile,
  };
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/next-chats/hooks/use-send-multiple-message.ts` is located in the `web/src/pages/next-chats/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [use-build-form-refs.ts](use-build-form-refs.ts_docs.md)
- [use-button-disabled.tsx](use-button-disabled.tsx_docs.md)
- [use-click-card.ts](use-click-card.ts_docs.md)
- [use-create-conversation.ts](use-create-conversation.ts_docs.md)
- [use-rename-chat.ts](use-rename-chat.ts_docs.md)
- [use-select-conversation-list.ts](use-select-conversation-list.ts_docs.md)
- [use-send-chat-message.ts](use-send-chat-message.ts_docs.md)
- [use-send-shared-message.ts](use-send-shared-message.ts_docs.md)
- [use-set-chat-route.ts](use-set-chat-route.ts_docs.md)
- [use-set-conversation.ts](use-set-conversation.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
