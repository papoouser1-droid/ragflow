# Documentation: web/src/pages/agent/share/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/share/index.tsx`
- **Size**: 7436 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/share/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/share/index.tsx` is located in the `web/src/pages/agent/share` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to share.

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

- [parameter-dialog.tsx](parameter-dialog.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
