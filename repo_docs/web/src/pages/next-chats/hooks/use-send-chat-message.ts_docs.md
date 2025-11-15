# File Documentation: web/src/pages/next-chats/hooks/use-send-chat-message.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-send-chat-message.ts`
- **Extension**: `.ts`
- **Lines**: 253
- **Characters**: 6,432
- **Size**: 6,432 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { FileUploadProps } from '@/components/file-upload';
import { MessageType } from '@/constants/chat';
import {
  useHandleMessageInputChange,
  useRegenerateMessage,
  useSelectDerivedMessages,
  useSendMessageWithSse,
} from '@/hooks/logic-hooks';
import {
  useFetchConversation,
  useGetChatSearchParams,
} from '@/hooks/use-chat-request';
import { Message } from '@/interfaces/database/chat';
import api from '@/utils/api';
import { trim } from 'lodash';
import { useCallback, useEffect } from 'react';
import { useParams } from 'umi';
import { v4 as uuid } from 'uuid';
import { IMessage } from '../chat/interface';
import { useFindPrologueFromDialogList } from './use-select-conversation-list';
import { useSetChatRouteParams } from './use-set-chat-route';
import { useSetConversation } from './use-set-conversation';
import { useUploadFile } from './use-upload-file';

export const useSelectNextMessages = () => {
  const {
    scrollRef,
    messageContainerRef,
    setDerivedMessages,
    derivedMessages,
    addNewestAnswer,
    addNewestQuestion,
    removeLatestMessage,
    removeMessageById,
    removeMessagesAfterCurrentMessage,
  } = useSelectDerivedMessages();
  const { data: conversation, loading } = useFetchConversation();
  const { conversationId, isNew } = useGetChatSearchParams();
  const { id: dialogId } = useParams();
  const prologue = useFindPrologueFromDialogList();

  const addPrologue = useCallback(() => {
    if (dialogId !== '' && isNew === 'true') {
      const nextMessage = {
        role: MessageType.Assistant,
        content: prologue,
        id: uuid(),
      } as IMessage;

      setDerivedMessages([nextMessage]);
    }
  }, [dialogId, isNew, prologue, setDerivedMessages]);

  useEffect(() => {
    addPrologue();
  }, [addPrologue]);

  useEffect(() => {
    if (
      conversationId &&
      isNew !== 'true' &&
      conversation.message?.length > 0
    ) {
      setDerivedMessages(conversation.message);
    }

    if (!conversationId) {
      setDerivedMessages([]);
    }
  }, [conversation.message, conversationId, setDerivedMessages, isNew]);

  return {
    scrollRef,
    messageContainerRef,
    derivedMessages,
    loading,
    addNewestAnswer,
    addNewestQuestion,
    removeLatestMessage,
    removeMessageById,
    removeMessagesAfterCurrentMessage,
  };
};

export const useSendMessage = (controller: AbortController) => {
  const { setConversation } = useSetConversation();
  const { conversationId, isNew } = useGetChatSearchParams();
  const { handleInputChange, value, setValue } = useHandleMessageInputChange();

  const { handleUploadFile, fileIds, clearFileIds, isUploading, removeFile } =
    useUploadFile();

  const { send, answer, done } = useSendMessageWithSse(
    api.completeConversation,
  );
  const {
    scrollRef,
    messageContainerRef,
    derivedMessages,
    loading,
    addNewestAnswer,
    addNewestQuestion,
    removeLatestMessage,
    removeMessageById,
    removeMessagesAfterCurrentMessage,
  } = useSelectNextMessages();
  const { setConversationIsNew, getConversationIsNew } =
    useSetChatRouteParams();

  const onUploadFile: NonNullable<FileUploadProps['onUpload']> = useCallback(
    async (files, options) => {
      const isNew = getConversationIsNew();

      if (isNew === 'true' && Array.isArray(files) && files.length) {
        const data = await setConversation(files[0].name, true);
        if (data.code === 0) {
          handleUploadFile(files, options, data.data?.id);
        }
      } else {
        handleUploadFile(files, options);
      }
    },
    [getConversationIsNew, handleUploadFile, setConversation],
  );

  const sendMessage = useCallback(
    async ({
      message,
      currentConversationId,
      messages,
    }: {
      message: Message;
      currentConversationId?: string;
      messages?: Message[];
    }) => {
      const res = await send(
        {
          conversation_id: currentConversationId ?? conversationId,
          messages: [...(messages ?? derivedMessages ?? []), message],
        },
        controller,
      );

      if (res && (res?.response.status !== 200 || res?.data?.code !== 0)) {
        // cancel loading
        setValue(message.content);
        console.info('removeLatestMessage111');
        removeLatestMessage();
      }
    },
    [
      derivedMessages,
      conversationId,
      removeLatestMessage,
      setValue,
      send,
      controller,
    ],
  );

  const handleSendMessage = useCallback(
    async (message: Message) => {
      const isNew = getConversationIsNew();
      if (isNew !== 'true') {
        sendMessage({ message });
      } else {
        const data = await setConversation(
          message.content,
          true,
          conversationId,
        );
        if (data.code === 0) {
          setConversationIsNew('');
          const id = data.data.id;
          // currentConversationIdRef.current = id;
          sendMessage({
            message,
            currentConversationId: id,
            messages: data.data.message,
          });
        }
      }
    },
    [
      setConversation,
      sendMessage,
      setConversationIsNew,
      getConversationIsNew,
      conversationId,
    ],
  );

  const { regenerateMessage } = useRegenerateMessage({
    removeMessagesAfterCurrentMessage,
    sendMessage,
    messages: derivedMessages,
  });

  useEffect(() => {
    //  #1289
    if (answer.answer && conversationId && isNew !== 'true') {
      addNewestAnswer(answer);
    }
  }, [answer, addNewestAnswer, conversationId, isNew]);

  const handlePressEnter = useCallback(() => {
    if (trim(value) === '') return;
    const id = uuid();

    addNewestQuestion({
      content: value,
      doc_ids: fileIds,
      id,
      role: MessageType.User,
    });
    if (done) {
      setValue('');
      handleSendMessage({
        id,
        content: value.trim(),
        role: MessageType.User,
        doc_ids: fileIds,
      });
    }
    clearFileIds();
  }, [
    value,
    addNewestQuestion,
    fileIds,
    done,
    clearFileIds,
    setValue,
    handleSendMessage,
  ]);

  return {
    handlePressEnter,
    handleInputChange,
    value,
    setValue,
    regenerateMessage,
    sendLoading: !done,
    loading,
    scrollRef,
    messageContainerRef,
    derivedMessages,
    removeMessageById,
    handleUploadFile: onUploadFile,
    isUploading,
    removeFile,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/hooks/use-send-chat-message.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 253 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `useSelectNextMessages`: Exported entity
- `useSendMessage`: Exported entity

### Functions (7)

- `useSelectNextMessages()`: Function definition
- `addPrologue()`: Function definition
- `nextMessage()`: Function definition
- `useSendMessage()`: Function definition
- `sendMessage()`: Function definition
- `handleSendMessage()`: Function definition
- `handlePressEnter()`: Function definition

### Imports (15)

- `import { FileUploadProps } from '@/components/file-upload';`
- `import { MessageType } from '@/constants/chat';`
- `import {`
- `import {`
- `import { Message } from '@/interfaces/database/chat';`
- `import api from '@/utils/api';`
- `import { trim } from 'lodash';`
- `import { useCallback, useEffect } from 'react';`
- `import { useParams } from 'umi';`
- `import { v4 as uuid } from 'uuid';`

## Code Structure Analysis

- Total lines: 253
- Blank lines: 21 (8.3%)
- Comment lines: ~3 (1.2%)
- Code lines: ~229


## Dependencies and Imports

- `@/components/file-upload`
- `@/constants/chat`
- `@/interfaces/database/chat`
- `@/utils/api`
- `lodash`
- `react`
- `umi`
- `uuid`
- `../chat/interface`
- `./use-select-conversation-list`
- `./use-set-chat-route`
- `./use-set-conversation`
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
- Potential test file: `test_use-send-chat-message.ts`

## Keywords

../chat/interface, ./use-select-conversation-list, ./use-set-chat-route, ./use-set-conversation, ./use-upload-file, @/components/file-upload, @/constants/chat, @/interfaces/database/chat, @/utils/api, AbortController, Array, Assistant, FileUploadProps, IMessage, Message, MessageType, NonNullable, TypeScript, User, addPrologue, data, handlePressEnter, handleSendMessage, id, isNew, lodash, nextMessage, onUploadFile, prologue, react, res, sendMessage, umi, useSelectNextMessages, useSendMessage, uuid

---
*Generated by RAGFlow Repository Documentation Generator*
