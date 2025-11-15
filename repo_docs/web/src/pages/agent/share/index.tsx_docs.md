# File Documentation: web/src/pages/agent/share/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/share/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 226
- **Characters**: 7,436
- **Size**: 7,436 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { EmbedContainer } from '@/components/embed-container';
import { FileUploadProps } from '@/components/file-upload';
import { NextMessageInput } from '@/components/message-input/next';
import MessageItem from '@/components/next-message-item';
import PdfDrawer from '@/components/pdf-drawer';
import { useClickDrawer } from '@/components/pdf-drawer/hooks';
import { MessageType } from '@/constants/chat';
import { useUploadCanvasFileWithProgress } from '@/hooks/use-agent-request';
import { cn } from '@/lib/utils';
import i18n from '@/locales/config';
import DebugContent from '@/pages/agent/debug-content';
import { useCacheChatLog } from '@/pages/agent/hooks/use-cache-chat-log';
import { useAwaitCompentData } from '@/pages/agent/hooks/use-chat-logic';
import { useSendButtonDisabled } from '@/pages/chat/hooks';
import { buildMessageUuidWithRole } from '@/utils/chat';
import { isEmpty } from 'lodash';
import React, { forwardRef, useCallback } from 'react';
import {
  useGetSharedChatSearchParams,
  useSendNextSharedMessage,
} from '../hooks/use-send-shared-message';
import { ParameterDialog } from './parameter-dialog';

const ChatContainer = () => {
  const {
    sharedId: conversationId,
    locale,
    visibleAvatar,
  } = useGetSharedChatSearchParams();
  const { visible, hideModal, documentId, selectedChunk, clickDocumentButton } =
    useClickDrawer();

  const { uploadCanvasFile, loading } =
    useUploadCanvasFileWithProgress(conversationId);
  const {
    addEventList,
    setCurrentMessageId,
    currentEventListWithoutMessageById,
    clearEventList,
  } = useCacheChatLog();

  const {
    handlePressEnter,
    handleInputChange,
    value,
    sendLoading,
    scrollRef,
    messageContainerRef,
    derivedMessages,
    hasError,
    inputsData,
    isTaskMode,
    stopOutputMessage,
    findReferenceByMessageId,
    appendUploadResponseList,
    parameterDialogVisible,
    showParameterDialog,
    sendFormMessage,
    addNewestOneAnswer,
    ok,
    resetSession,
  } = useSendNextSharedMessage(addEventList);

  const { buildInputList, handleOk, isWaitting } = useAwaitCompentData({
    derivedMessages,
    sendFormMessage,
    canvasId: conversationId as string,
  });
  const sendDisabled = useSendButtonDisabled(value);

  const showBeginParameterDialog = useCallback(() => {
    if (inputsData && inputsData.inputs && !isEmpty(inputsData.inputs)) {
      showParameterDialog();
    }
  }, [inputsData, showParameterDialog]);

  const handleUploadFile: NonNullable<FileUploadProps['onUpload']> =
    useCallback(
      async (files, options) => {
        const ret = await uploadCanvasFile({ files, options });
        appendUploadResponseList(ret.data, files);
      },
      [appendUploadResponseList, uploadCanvasFile],
    );

  React.useEffect(() => {
    if (locale && i18n.language !== locale) {
      i18n.changeLanguage(locale);
    }
  }, [locale, visibleAvatar]);

  React.useEffect(() => {
    if (!isTaskMode && inputsData.prologue) {
      addNewestOneAnswer({
        answer: inputsData.prologue,
      });
    }
  }, [inputsData.prologue, addNewestOneAnswer, isTaskMode]);

  React.useEffect(() => {
    showBeginParameterDialog();
  }, [showBeginParameterDialog]);

  const handleInputsModalOk = (params: any[]) => {
    ok(params);
  };
  const handleReset = () => {
    resetSession();
    clearEventList();
    showBeginParameterDialog();
  };
  if (!conversationId) {
    return <div>empty</div>;
  }

  return (
    <>
      <EmbedContainer
        title={inputsData.title}
        avatar={inputsData.avatar}
        handleReset={handleReset}
      >
        <div className="flex flex-1 flex-col p-2.5  h-[90vh] m-3">
          <div
            className={cn(
              'flex flex-1 flex-col overflow-auto scrollbar-auto m-auto w-5/6',
            )}
            ref={messageContainerRef}
          >
            <div>
              {derivedMessages?.map((message, i) => {
                return (
                  <MessageItem
                    visibleAvatar={visibleAvatar}
                    conversationId={conversationId}
                    currentEventListWithoutMessageById={
                      currentEventListWithoutMessageById
                    }
                    setCurrentMessageId={setCurrentMessageId}
                    key={buildMessageUuidWithRole(message)}
                    item={message}
                    nickname="You"
                    reference={findReferenceByMessageId(message.id)}
                    loading={
                      message.role === MessageType.Assistant &&
                      sendLoading &&
                      derivedMessages?.length - 1 === i
                    }
                    isShare={true}
                    avatarDialog={inputsData.avatar}
                    agentName={inputsData.title}
                    index={i}
                    clickDocumentButton={clickDocumentButton}
                    showLikeButton={false}
                    showLoudspeaker={false}
                    showLog={false}
                    sendLoading={sendLoading}
                  >
                    {message.role === MessageType.Assistant &&
                      derivedMessages.length - 1 === i && (
                        <DebugContent
                          parameters={buildInputList(message)}
                          message={message}
                          ok={handleOk(message)}
                          isNext={false}
                          btnText={'Submit'}
                        ></DebugContent>
                      )}
                    {message.role === MessageType.Assistant &&
                      derivedMessages.length - 1 !== i && (
                        <div>
                          <div>{message?.data?.tips}</div>

                          <div>
                            {buildInputList(message)?.map((item) => item.value)}
                          </div>
                        </div>
                      )}
                  </MessageItem>
                );
              })}
            </div>
            <div ref={scrollRef} />
          </div>
          {isTaskMode || (
            <div className="flex w-full justify-center mb-8">
              <div className="w-5/6">
                <NextMessageInput
                  isShared
                  value={value}
                  disabled={hasError || isWaitting}
                  sendDisabled={sendDisabled || isWaitting}
                  conversationId={conversationId}
                  onInputChange={handleInputChange}
                  onPressEnter={handlePressEnter}
                  sendLoading={sendLoading}
                  stopOutputMessage={stopOutputMessage}
                  onUpload={handleUploadFile}
                  isUploading={loading || isWaitting}
                ></NextMessageInput>
              </div>
            </div>
          )}
        </div>
      </EmbedContainer>
      {visible && (
        <PdfDrawer
          visible={visible}
          hideModal={hideModal}
          documentId={documentId}
          chunk={selectedChunk}
        ></PdfDrawer>
      )}
      {parameterDialogVisible && (
        <ParameterDialog
          // hideModal={hideParameterDialog}
          ok={handleInputsModalOk}
          data={inputsData.inputs}
        ></ParameterDialog>
      )}
    </>
  );
};

export default forwardRef(ChatContainer);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/share/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 226 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `ChatContainer()`: Function definition
- `showBeginParameterDialog()`: Function definition
- `ret()`: Function definition
- `handleInputsModalOk()`: Function definition
- `handleReset()`: Function definition

### Imports (19)

- `import { EmbedContainer } from '@/components/embed-container';`
- `import { FileUploadProps } from '@/components/file-upload';`
- `import { NextMessageInput } from '@/components/message-input/next';`
- `import MessageItem from '@/components/next-message-item';`
- `import PdfDrawer from '@/components/pdf-drawer';`
- `import { useClickDrawer } from '@/components/pdf-drawer/hooks';`
- `import { MessageType } from '@/constants/chat';`
- `import { useUploadCanvasFileWithProgress } from '@/hooks/use-agent-request';`
- `import { cn } from '@/lib/utils';`
- `import i18n from '@/locales/config';`

## Code Structure Analysis

- Total lines: 226
- Blank lines: 14 (6.2%)
- Comment lines: ~1 (0.4%)
- Code lines: ~211


## Dependencies and Imports

- `@/components/embed-container`
- `@/components/file-upload`
- `@/components/message-input/next`
- `@/components/next-message-item`
- `@/components/pdf-drawer`
- `@/components/pdf-drawer/hooks`
- `@/constants/chat`
- `@/hooks/use-agent-request`
- `@/lib/utils`
- `@/locales/config`
- `@/pages/agent/debug-content`
- `@/pages/agent/hooks/use-cache-chat-log`
- `@/pages/agent/hooks/use-chat-logic`
- `@/pages/chat/hooks`
- `@/utils/chat`
- `lodash`
- `react`
- `./parameter-dialog`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/share`.

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

- Other files in `web/src/pages/agent/share/` directory
- Potential test file: `test_index.tsx`

## Keywords

./parameter-dialog, @/components/embed-container, @/components/file-upload, @/components/message-input/next, @/components/next-message-item, @/components/pdf-drawer, @/components/pdf-drawer/hooks, @/constants/chat, @/hooks/use-agent-request, @/lib/utils, @/locales/config, @/pages/agent/debug-content, @/pages/agent/hooks/use-cache-chat-log, @/pages/agent/hooks/use-chat-logic, @/pages/chat/hooks, @/utils/chat, Assistant, ChatContainer, DebugContent, EmbedContainer, FileUploadProps, MessageItem, MessageType, NextMessageInput, NonNullable, ParameterDialog, PdfDrawer, React, Submit, TypeScript, You, handleInputsModalOk, handleReset, handleUploadFile, lodash, react, ret, sendDisabled, showBeginParameterDialog

---
*Generated by RAGFlow Repository Documentation Generator*
