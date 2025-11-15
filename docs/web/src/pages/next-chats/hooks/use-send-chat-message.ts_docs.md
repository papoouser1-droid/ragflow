# Documentation: web/src/pages/next-chats/hooks/use-send-chat-message.ts

## File Metadata

- **Path**: `web/src/pages/next-chats/hooks/use-send-chat-message.ts`
- **Size**: 6432 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/next-chats/hooks/use-send-chat-message.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/next-chats/hooks/use-send-chat-message.ts` is located in the `web/src/pages/next-chats/hooks` directory.

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
- [use-send-multiple-message.ts](use-send-multiple-message.ts_docs.md)
- [use-send-shared-message.ts](use-send-shared-message.ts_docs.md)
- [use-set-chat-route.ts](use-set-chat-route.ts_docs.md)
- [use-set-conversation.ts](use-set-conversation.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
