# Documentation: web/src/components/next-message-item/index.tsx

## File Metadata

- **Path**: `web/src/components/next-message-item/index.tsx`
- **Size**: 9939 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/next-message-item/index.tsx`.

## Original Source Code

```tsx
import { ReactComponent as AssistantIcon } from '@/assets/svg/assistant.svg';
import { MessageType } from '@/constants/chat';
import { IReferenceChunk, IReferenceObject } from '@/interfaces/database/chat';
import classNames from 'classnames';
import {
  PropsWithChildren,
  memo,
  useCallback,
  useContext,
  useEffect,
  useMemo,
  useState,
} from 'react';

import { IRegenerateMessage, IRemoveMessageById } from '@/hooks/logic-hooks';
import { INodeEvent, MessageEventType } from '@/hooks/use-send-message';
import { cn } from '@/lib/utils';
import { AgentChatContext } from '@/pages/agent/context';
import { WorkFlowTimeline } from '@/pages/agent/log-sheet/workflow-timeline';
import { IMessage } from '@/pages/chat/interface';
import { downloadFile } from '@/services/file-manager-service';
import { downloadFileFromBlob } from '@/utils/file-util';
import { isEmpty } from 'lodash';
import { Atom, ChevronDown, ChevronUp, Download } from 'lucide-react';
import MarkdownContent from '../next-markdown-content';
import { RAGFlowAvatar } from '../ragflow-avatar';
import { useTheme } from '../theme-provider';
import { Button } from '../ui/button';
import { AssistantGroupButton, UserGroupButton } from './group-button';
import styles from './index.less';
import { ReferenceDocumentList } from './reference-document-list';
import { UploadedMessageFiles } from './uploaded-message-files';

interface IProps
  extends Partial<IRemoveMessageById>,
    IRegenerateMessage,
    PropsWithChildren {
  item: IMessage;
  conversationId?: string;
  currentEventListWithoutMessageById?: (messageId: string) => INodeEvent[];
  setCurrentMessageId?: (messageId: string) => void;
  reference?: IReferenceObject;
  loading?: boolean;
  sendLoading?: boolean;
  visibleAvatar?: boolean;
  nickname?: string;
  avatar?: string;
  avatarDialog?: string | null;
  agentName?: string;
  clickDocumentButton?: (documentId: string, chunk: IReferenceChunk) => void;
  index: number;
  showLikeButton?: boolean;
  showLoudspeaker?: boolean;
  showLog?: boolean;
  isShare?: boolean;
}

function MessageItem({
  item,
  conversationId,
  currentEventListWithoutMessageById,
  setCurrentMessageId,
  reference,
  loading = false,
  avatar,
  avatarDialog,
  agentName,
  sendLoading = false,
  clickDocumentButton,
  removeMessageById,
  regenerateMessage,
  showLikeButton = true,
  showLoudspeaker = true,
  visibleAvatar = true,
  children,
  showLog,
  isShare,
}: IProps) {
  const { theme } = useTheme();
  const isAssistant = item.role === MessageType.Assistant;
  const isUser = item.role === MessageType.User;
  const [showThinking, setShowThinking] = useState(false);
  const { setLastSendLoadingFunc } = useContext(AgentChatContext);

  useEffect(() => {
    if (typeof setLastSendLoadingFunc === 'function') {
      setLastSendLoadingFunc(loading, item.id);
    }
  }, [loading, setLastSendLoadingFunc, item.id]);

  const referenceDocuments = useMemo(() => {
    const docs = reference?.doc_aggs ?? {};

    return Object.values(docs);
  }, [reference?.doc_aggs]);

  const handleRegenerateMessage = useCallback(() => {
    regenerateMessage?.(item);
  }, [regenerateMessage, item]);

  useEffect(() => {
    if (typeof setCurrentMessageId === 'function') {
      setCurrentMessageId(item.id);
    }
  }, [item.id, setCurrentMessageId]);

  const startedNodeList = useCallback(
    (item: IMessage) => {
      const finish = currentEventListWithoutMessageById?.(item.id)?.some(
        (item) => item.event === MessageEventType.WorkflowFinished,
      );
      return !finish && loading;
    },
    [currentEventListWithoutMessageById, loading],
  );
  return (
    <div
      className={classNames(styles.messageItem, {
        [styles.messageItemLeft]: item.role === MessageType.Assistant,
        [styles.messageItemRight]: item.role === MessageType.User,
      })}
    >
      <section
        className={classNames(styles.messageItemSection, {
          [styles.messageItemSectionLeft]: item.role === MessageType.Assistant,
          [styles.messageItemSectionRight]: item.role === MessageType.User,
        })}
      >
        <div
          className={classNames(styles.messageItemContent, {
            [styles.messageItemContentReverse]: item.role === MessageType.User,
          })}
        >
          {visibleAvatar &&
            (item.role === MessageType.User ? (
              <RAGFlowAvatar avatar={avatar ?? '/logo.svg'} />
            ) : avatarDialog || agentName ? (
              <RAGFlowAvatar
                avatar={avatarDialog as string}
                name={agentName}
                isPerson
              />
            ) : (
              <AssistantIcon />
            ))}
          <section className="flex-col gap-2 flex-1">
            <div className="flex justify-between items-center">
              {isShare && isAssistant && (
                <Button
                  variant={'transparent'}
                  onClick={() => setShowThinking((think) => !think)}
                >
                  <div className="flex items-center gap-1">
                    <div className="">
                      <Atom
                        className={startedNodeList(item) ? 'animate-spin' : ''}
                      />
                    </div>
                    Thinking
                    {showThinking ? <ChevronUp /> : <ChevronDown />}
                  </div>
                </Button>
              )}
              <div className="space-x-1">
                {isAssistant ? (
                  <>
                    {isShare && !sendLoading && !isEmpty(item.content) && (
                      <AssistantGroupButton
                        messageId={item.id}
                        content={item.content}
                        prompt={item.prompt}
                        showLikeButton={showLikeButton}
                        audioBinary={item.audio_binary}
                        showLoudspeaker={showLoudspeaker}
                        showLog={showLog}
                      ></AssistantGroupButton>
                    )}
                    {!isShare && (
                      <AssistantGroupButton
                        messageId={item.id}
                        content={item.content}
                        prompt={item.prompt}
                        showLikeButton={showLikeButton}
                        audioBinary={item.audio_binary}
                        showLoudspeaker={showLoudspeaker}
                        showLog={showLog}
                      ></AssistantGroupButton>
                    )}
                  </>
                ) : (
                  <UserGroupButton
                    content={item.content}
                    messageId={item.id}
                    removeMessageById={removeMessageById}
                    regenerateMessage={
                      regenerateMessage && handleRegenerateMessage
                    }
                    sendLoading={sendLoading}
                  ></UserGroupButton>
                )}
              </div>
            </div>

            {isAssistant &&
              currentEventListWithoutMessageById &&
              showThinking && (
                <div className="mt-4 mb-4">
                  <WorkFlowTimeline
                    currentEventListWithoutMessage={currentEventListWithoutMessageById(
                      item.id,
                    )}
                    isShare={isShare}
                    currentMessageId={item.id}
                    canvasId={conversationId}
                    sendLoading={loading}
                  />
                </div>
              )}
            <div
              className={cn({
                [theme === 'dark'
                  ? styles.messageTextDark
                  : styles.messageText]: isAssistant,
                [styles.messageUserText]: !isAssistant,
                'bg-bg-card': !isAssistant,
              })}
            >
              {item.data ? (
                children
              ) : sendLoading && isEmpty(item.content) ? (
                <>{!isShare && 'running...'}</>
              ) : (
                <MarkdownContent
                  loading={loading}
                  content={item.content}
                  reference={reference}
                  clickDocumentButton={clickDocumentButton}
                ></MarkdownContent>
              )}
            </div>
            {isAssistant && referenceDocuments.length > 0 && (
              <ReferenceDocumentList
                list={referenceDocuments}
              ></ReferenceDocumentList>
            )}

            {isUser && (
              <UploadedMessageFiles files={item.files}></UploadedMessageFiles>
            )}
            {isAssistant && item.attachment && item.attachment.doc_id && (
              <div className="w-full flex items-center justify-end">
                <Button
                  variant="link"
                  className="p-1 m-0 h-auto text-text-sub-title-invert"
                  onClick={async () => {
                    if (item.attachment?.doc_id) {
                      try {
                        const response = await downloadFile({
                          docId: item.attachment.doc_id,
                          ext: item.attachment.format,
                        });
                        const blob = new Blob([response.data], {
                          type: response.data.type,
                        });
                        downloadFileFromBlob(blob, item.attachment.file_name);
                      } catch (error) {
                        console.error('Download failed:', error);
                      }
                    }
                  }}
                >
                  <Download size={16} />
                </Button>
              </div>
            )}
          </section>
        </div>
      </section>
    </div>
  );
}

export default memo(MessageItem);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/next-message-item/index.tsx` is located in the `web/src/components/next-message-item` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to next-message-item.

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

- [feedback-modal.tsx](feedback-modal.tsx_docs.md)
- [group-button.tsx](group-button.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [prompt-modal.tsx](prompt-modal.tsx_docs.md)
- [reference-document-list.tsx](reference-document-list.tsx_docs.md)
- [uploaded-message-files.tsx](uploaded-message-files.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
