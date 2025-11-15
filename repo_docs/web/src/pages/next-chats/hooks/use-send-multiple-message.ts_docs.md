# File Documentation: web/src/pages/next-chats/hooks/use-send-multiple-message.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-send-multiple-message.ts`
- **Extension**: `.ts`
- **Lines**: 240
- **Characters**: 6,531
- **Size**: 6,531 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/hooks/use-send-multiple-message.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 240 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useSendMultipleChatMessage`: Exported entity

### Functions (8)

- `useSendMultipleChatMessage()`: Function definition
- `addNewestAnswer()`: Function definition
- `removeLatestMessage()`: Function definition
- `adjustRecordByChatBoxIds()`: Function definition
- `currentRecord()`: Function definition
- `sendMessage()`: Function definition
- `handlePressEnter()`: Function definition
- `id()`: Function definition

### Imports (13)

- `import showMessage from '@/components/ui/message';`
- `import { MessageType } from '@/constants/chat';`
- `import {`
- `import { useGetChatSearchParams } from '@/hooks/use-chat-request';`
- `import { IAnswer, Message } from '@/interfaces/database/chat';`
- `import api from '@/utils/api';`
- `import { buildMessageUuid } from '@/utils/chat';`
- `import { trim } from 'lodash';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { v4 as uuid } from 'uuid';`

## Code Structure Analysis

- Total lines: 240
- Blank lines: 26 (10.8%)
- Comment lines: ~4 (1.7%)
- Code lines: ~210


## Dependencies and Imports

- `@/components/ui/message`
- `@/constants/chat`
- `@/hooks/use-chat-request`
- `@/interfaces/database/chat`
- `@/utils/api`
- `@/utils/chat`
- `lodash`
- `react`
- `uuid`
- `../chat/interface`
- `./use-build-form-refs`
- `./use-upload-file`

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
- Potential test file: `test_use-send-multiple-message.ts`

## Keywords

../chat/interface, ./use-build-form-refs, ./use-upload-file, @/components/ui/message, @/constants/chat, @/hooks/use-chat-request, @/interfaces/database/chat, @/utils/api, @/utils/chat, AbortController, Add, Assistant, IAnswer, IMessage, Message, MessageType, Object, Record, The, TypeScript, User, addNewestAnswer, addNewestQuestion, adjustRecordByChatBoxIds, chatBoxId, currentChatMessages, currentRecord, derivedMessages, handlePressEnter, id, lodash, nextChatMessages, react, removeLatestMessage, res, sendMessage, useSendMultipleChatMessage, uuid

---
*Generated by RAGFlow Repository Documentation Generator*
