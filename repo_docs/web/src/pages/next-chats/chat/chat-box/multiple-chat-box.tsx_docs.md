# File Documentation: web/src/pages/next-chats/chat/chat-box/multiple-chat-box.tsx

## File Metadata

- **Path**: `web/src/pages/next-chats/chat/chat-box/multiple-chat-box.tsx`
- **Extension**: `.tsx`
- **Lines**: 270
- **Characters**: 8,678
- **Size**: 8,678 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { LargeModelFormFieldWithoutFilter } from '@/components/large-model-form-field';
import { LlmSettingSchema } from '@/components/llm-setting-items/next';
import { NextMessageInput } from '@/components/message-input/next';
import MessageItem from '@/components/message-item';
import PdfDrawer from '@/components/pdf-drawer';
import { useClickDrawer } from '@/components/pdf-drawer/hooks';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Form } from '@/components/ui/form';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { MessageType } from '@/constants/chat';
import { useScrollToBottom } from '@/hooks/logic-hooks';
import {
  useFetchConversation,
  useFetchDialog,
  useGetChatSearchParams,
  useSetDialog,
} from '@/hooks/use-chat-request';
import { useFetchUserInfo } from '@/hooks/user-setting-hooks';
import { buildMessageUuidWithRole } from '@/utils/chat';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { isEmpty, omit } from 'lodash';
import { ListCheck, Plus, Trash2 } from 'lucide-react';
import { forwardRef, useCallback, useImperativeHandle, useRef } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useParams } from 'umi';
import { z } from 'zod';
import {
  useGetSendButtonDisabled,
  useSendButtonDisabled,
} from '../../hooks/use-button-disabled';
import { useCreateConversationBeforeUploadDocument } from '../../hooks/use-create-conversation';
import { useSendMessage } from '../../hooks/use-send-chat-message';
import { useSendMultipleChatMessage } from '../../hooks/use-send-multiple-message';
import { buildMessageItemReference } from '../../utils';
import { IMessage } from '../interface';
import { useAddChatBox } from '../use-add-box';

type MultipleChatBoxProps = {
  controller: AbortController;
  chatBoxIds: string[];
  stopOutputMessage(): void;
} & Pick<
  ReturnType<typeof useAddChatBox>,
  'removeChatBox' | 'addChatBox' | 'chatBoxIds'
>;

type ChatCardProps = {
  id: string;
  idx: number;
  derivedMessages: IMessage[];
  sendLoading: boolean;
} & Pick<
  MultipleChatBoxProps,
  'controller' | 'removeChatBox' | 'addChatBox' | 'chatBoxIds'
> &
  Pick<ReturnType<typeof useClickDrawer>, 'clickDocumentButton'>;

const ChatCard = forwardRef(function ChatCard(
  {
    controller,
    removeChatBox,
    id,
    idx,
    addChatBox,
    chatBoxIds,
    derivedMessages,
    sendLoading,
    clickDocumentButton,
  }: ChatCardProps,
  ref,
) {
  const { id: dialogId } = useParams();
  const { setDialog } = useSetDialog();

  const { regenerateMessage, removeMessageById } = useSendMessage(controller);

  const messageContainerRef = useRef<HTMLDivElement>(null);

  const { scrollRef } = useScrollToBottom(derivedMessages, messageContainerRef);

  const FormSchema = z.object(LlmSettingSchema);

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      llm_id: '',
    },
  });

  const llmId = useWatch({ control: form.control, name: 'llm_id' });

  const { data: userInfo } = useFetchUserInfo();
  const { data: currentDialog } = useFetchDialog();
  const { data: conversation } = useFetchConversation();

  const isLatestChat = idx === chatBoxIds.length - 1;

  const handleRemoveChatBox = useCallback(() => {
    removeChatBox(id);
  }, [id, removeChatBox]);

  const handleApplyConfig = useCallback(() => {
    const values = form.getValues();
    setDialog({
      ...currentDialog,
      llm_id: values.llm_id,
      llm_setting: omit(values, 'llm_id'),
      dialog_id: dialogId,
    });
  }, [currentDialog, dialogId, form, setDialog]);

  useImperativeHandle(ref, () => ({
    getFormData: () => form.getValues(),
  }));

  return (
    <Card className="bg-transparent border flex-1 flex flex-col">
      <CardHeader className="border-b px-5 py-3">
        <CardTitle className="flex justify-between items-center">
          <div className="flex items-center gap-3">
            <span className="text-base">{idx + 1}</span>
            <Form {...form}>
              <LargeModelFormFieldWithoutFilter></LargeModelFormFieldWithoutFilter>
            </Form>
          </div>
          <div className="space-x-2">
            <Tooltip>
              <TooltipTrigger>
                <Button
                  variant={'ghost'}
                  disabled={isEmpty(llmId)}
                  onClick={handleApplyConfig}
                >
                  <ListCheck />
                </Button>
              </TooltipTrigger>
              <TooltipContent>
                <p>{t('chat.applyModelConfigs')}</p>
              </TooltipContent>
            </Tooltip>
            {!isLatestChat || chatBoxIds.length === 3 ? (
              <Button variant={'ghost'} onClick={handleRemoveChatBox}>
                <Trash2 />
              </Button>
            ) : (
              <Button variant={'ghost'} onClick={addChatBox}>
                <Plus></Plus>
              </Button>
            )}
          </div>
        </CardTitle>
      </CardHeader>
      <CardContent className="flex-1 min-h-0">
        <div ref={messageContainerRef} className="h-full overflow-auto">
          <div className="w-full">
            {derivedMessages?.map((message, i) => {
              return (
                <MessageItem
                  loading={
                    message.role === MessageType.Assistant &&
                    sendLoading &&
                    derivedMessages.length - 1 === i
                  }
                  key={buildMessageUuidWithRole(message)}
                  item={message}
                  nickname={userInfo.nickname}
                  avatar={userInfo.avatar}
                  avatarDialog={currentDialog.icon}
                  reference={buildMessageItemReference(
                    {
                      message: derivedMessages,
                      reference: conversation.reference,
                    },
                    message,
                  )}
                  // clickDocumentButton={clickDocumentButton}
                  index={i}
                  removeMessageById={removeMessageById}
                  regenerateMessage={regenerateMessage}
                  sendLoading={sendLoading}
                  clickDocumentButton={clickDocumentButton}
                ></MessageItem>
              );
            })}
          </div>
          <div ref={scrollRef} />
        </div>
      </CardContent>
    </Card>
  );
});

export function MultipleChatBox({
  controller,
  chatBoxIds,
  removeChatBox,
  addChatBox,
  stopOutputMessage,
}: MultipleChatBoxProps) {
  const {
    value,
    sendLoading,
    messageRecord,
    handleInputChange,
    handlePressEnter,
    setFormRef,
    handleUploadFile,
  } = useSendMultipleChatMessage(controller, chatBoxIds);

  const { createConversationBeforeUploadDocument } =
    useCreateConversationBeforeUploadDocument();
  const { conversationId } = useGetChatSearchParams();
  const disabled = useGetSendButtonDisabled();
  const sendDisabled = useSendButtonDisabled(value);
  const { visible, hideModal, documentId, selectedChunk, clickDocumentButton } =
    useClickDrawer();

  return (
    <section className="h-full flex flex-col px-5">
      <div className="flex gap-4 flex-1 px-5 pb-14 min-h-0">
        {chatBoxIds.map((id, idx) => (
          <ChatCard
            key={id}
            idx={idx}
            controller={controller}
            id={id}
            chatBoxIds={chatBoxIds}
            removeChatBox={removeChatBox}
            addChatBox={addChatBox}
            derivedMessages={messageRecord[id]}
            ref={setFormRef(id)}
            sendLoading={sendLoading}
            clickDocumentButton={clickDocumentButton}
          ></ChatCard>
        ))}
      </div>
      <div className="px-[20%]">
        <NextMessageInput
          disabled={disabled}
          sendDisabled={sendDisabled}
          sendLoading={sendLoading}
          value={value}
          onInputChange={handleInputChange}
          onPressEnter={handlePressEnter}
          conversationId={conversationId}
          createConversationBeforeUploadDocument={
            createConversationBeforeUploadDocument
          }
          stopOutputMessage={stopOutputMessage}
          onUpload={handleUploadFile}
        />
      </div>
      {visible && (
        <PdfDrawer
          visible={visible}
          hideModal={hideModal}
          documentId={documentId}
          chunk={selectedChunk}
        ></PdfDrawer>
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/chat/chat-box/multiple-chat-box.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 270 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `MultipleChatBox`: Exported entity

### Functions (5)

- `ChatCard()`: Function definition
- `handleRemoveChatBox()`: Function definition
- `handleApplyConfig()`: Function definition
- `values()`: Function definition
- `MultipleChatBox()`: Function definition

### Imports (30)

- `import { LargeModelFormFieldWithoutFilter } from '@/components/large-model-form-field';`
- `import { LlmSettingSchema } from '@/components/llm-setting-items/next';`
- `import { NextMessageInput } from '@/components/message-input/next';`
- `import MessageItem from '@/components/message-item';`
- `import PdfDrawer from '@/components/pdf-drawer';`
- `import { useClickDrawer } from '@/components/pdf-drawer/hooks';`
- `import { Button } from '@/components/ui/button';`
- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import { Form } from '@/components/ui/form';`
- `import {`

## Code Structure Analysis

- Total lines: 270
- Blank lines: 19 (7.0%)
- Comment lines: ~1 (0.4%)
- Code lines: ~250


## Dependencies and Imports

- `@/components/large-model-form-field`
- `@/components/llm-setting-items/next`
- `@/components/message-input/next`
- `@/components/message-item`
- `@/components/pdf-drawer`
- `@/components/pdf-drawer/hooks`
- `@/components/ui/button`
- `@/components/ui/card`
- `@/components/ui/form`
- `@/constants/chat`
- `@/hooks/logic-hooks`
- `@/hooks/user-setting-hooks`
- `@/utils/chat`
- `@hookform/resolvers/zod`
- `i18next`
- `lodash`
- `lucide-react`
- `react`
- `react-hook-form`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats/chat/chat-box`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/next-chats/chat/chat-box/` directory
- Potential test file: `test_multiple-chat-box.tsx`

## Keywords

../../hooks/use-create-conversation, ../../hooks/use-send-chat-message, ../../hooks/use-send-multiple-message, ../../utils, ../interface, ../use-add-box, @/components/large-model-form-field, @/components/llm-setting-items/next, @/components/message-input/next, @/components/message-item, @/components/pdf-drawer, @/components/pdf-drawer/hooks, @/components/ui/button, @/components/ui/card, @/components/ui/form, @/constants/chat, @/hooks/logic-hooks, @/hooks/user-setting-hooks, @/utils/chat, @hookform/resolvers/zod, AbortController, Assistant, Button, Card, CardContent, CardHeader, CardTitle, ChatCard, ChatCardProps, Form, FormSchema, HTMLDivElement, IMessage, LargeModelFormFieldWithoutFilter, ListCheck, LlmSettingSchema, MessageItem, MessageType, MultipleChatBox, MultipleChatBoxProps, NextMessageInput, PdfDrawer, Pick, Plus, ReturnType, Tooltip, TooltipContent, TooltipTrigger, Trash2, TypeScript...

---
*Generated by RAGFlow Repository Documentation Generator*
