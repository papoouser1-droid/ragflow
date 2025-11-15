# File Documentation: web/src/components/next-message-item/group-button.tsx

## File Metadata

- **Path**: `web/src/components/next-message-item/group-button.tsx`
- **Extension**: `.tsx`
- **Lines**: 167
- **Characters**: 4,649
- **Size**: 4,649 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { PromptIcon } from '@/assets/icon/next-icon';
import CopyToClipboard from '@/components/copy-to-clipboard';
import { useSetModalState } from '@/hooks/common-hooks';
import { IRemoveMessageById } from '@/hooks/logic-hooks';
import { AgentChatContext } from '@/pages/agent/context';
import {
  DeleteOutlined,
  DislikeOutlined,
  LikeOutlined,
  PauseCircleOutlined,
  SoundOutlined,
  SyncOutlined,
} from '@ant-design/icons';
import { Radio, Tooltip } from 'antd';
import { NotebookText } from 'lucide-react';
import { useCallback, useContext } from 'react';
import { useTranslation } from 'react-i18next';
import { ToggleGroup, ToggleGroupItem } from '../ui/toggle-group';
import FeedbackModal from './feedback-modal';
import { useRemoveMessage, useSendFeedback, useSpeech } from './hooks';
import PromptModal from './prompt-modal';

interface IProps {
  messageId: string;
  content: string;
  prompt?: string;
  showLikeButton: boolean;
  audioBinary?: string;
  showLoudspeaker?: boolean;
  showLog?: boolean;
}

export const AssistantGroupButton = ({
  messageId,
  content,
  prompt,
  audioBinary,
  showLikeButton,
  showLoudspeaker = true,
  showLog = true,
}: IProps) => {
  const { visible, hideModal, showModal, onFeedbackOk, loading } =
    useSendFeedback(messageId);
  const {
    visible: promptVisible,
    hideModal: hidePromptModal,
    showModal: showPromptModal,
  } = useSetModalState();
  const { t } = useTranslation();
  const { handleRead, ref, isPlaying } = useSpeech(content, audioBinary);

  const handleLike = useCallback(() => {
    onFeedbackOk({ thumbup: true });
  }, [onFeedbackOk]);

  const { showLogSheet } = useContext(AgentChatContext);

  const handleShowLogSheet = useCallback(() => {
    showLogSheet(messageId);
  }, [messageId, showLogSheet]);

  return (
    <>
      <ToggleGroup
        type={'single'}
        size="sm"
        variant="outline"
        className="space-x-1"
      >
        <ToggleGroupItem value="a">
          <CopyToClipboard text={content}></CopyToClipboard>
        </ToggleGroupItem>
        {showLoudspeaker && (
          <ToggleGroupItem value="b" onClick={handleRead}>
            <Tooltip title={t('chat.read')}>
              {isPlaying ? <PauseCircleOutlined /> : <SoundOutlined />}
            </Tooltip>
            <audio src="" ref={ref}></audio>
          </ToggleGroupItem>
        )}
        {showLikeButton && (
          <>
            <ToggleGroupItem value="c" onClick={handleLike}>
              <LikeOutlined />
            </ToggleGroupItem>
            <ToggleGroupItem value="d" onClick={showModal}>
              <DislikeOutlined />
            </ToggleGroupItem>
          </>
        )}
        {prompt && (
          <Radio.Button value="e" onClick={showPromptModal}>
            <PromptIcon style={{ fontSize: '16px' }} />
          </Radio.Button>
        )}
        {showLog && (
          <ToggleGroupItem value="f" onClick={handleShowLogSheet}>
            <NotebookText className="size-4" />
          </ToggleGroupItem>
        )}
      </ToggleGroup>
      {visible && (
        <FeedbackModal
          visible={visible}
          hideModal={hideModal}
          onOk={onFeedbackOk}
          loading={loading}
        ></FeedbackModal>
      )}
      {promptVisible && (
        <PromptModal
          visible={promptVisible}
          hideModal={hidePromptModal}
          prompt={prompt}
        ></PromptModal>
      )}
    </>
  );
};

interface UserGroupButtonProps extends Partial<IRemoveMessageById> {
  messageId: string;
  content: string;
  regenerateMessage?: () => void;
  sendLoading: boolean;
}

export const UserGroupButton = ({
  content,
  messageId,
  sendLoading,
  removeMessageById,
  regenerateMessage,
}: UserGroupButtonProps) => {
  const { onRemoveMessage, loading } = useRemoveMessage(
    messageId,
    removeMessageById,
  );
  const { t } = useTranslation();

  return (
    <Radio.Group size="small">
      <Radio.Button value="a">
        <CopyToClipboard text={content}></CopyToClipboard>
      </Radio.Button>
      {regenerateMessage && (
        <Radio.Button
          value="b"
          onClick={regenerateMessage}
          disabled={sendLoading}
        >
          <Tooltip title={t('chat.regenerate')}>
            <SyncOutlined spin={sendLoading} />
          </Tooltip>
        </Radio.Button>
      )}
      {removeMessageById && (
        <Radio.Button value="c" onClick={onRemoveMessage} disabled={loading}>
          <Tooltip title={t('common.delete')}>
            <DeleteOutlined spin={loading} />
          </Tooltip>
        </Radio.Button>
      )}
    </Radio.Group>
  );
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/next-message-item/group-button.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 167 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `AssistantGroupButton`: Exported entity
- `UserGroupButton`: Exported entity

### Functions (4)

- `AssistantGroupButton()`: Function definition
- `handleLike()`: Function definition
- `handleShowLogSheet()`: Function definition
- `UserGroupButton()`: Function definition

### Imports (14)

- `import { PromptIcon } from '@/assets/icon/next-icon';`
- `import CopyToClipboard from '@/components/copy-to-clipboard';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { IRemoveMessageById } from '@/hooks/logic-hooks';`
- `import { AgentChatContext } from '@/pages/agent/context';`
- `import {`
- `import { Radio, Tooltip } from 'antd';`
- `import { NotebookText } from 'lucide-react';`
- `import { useCallback, useContext } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 167
- Blank lines: 10 (6.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~157


## Dependencies and Imports

- `@/assets/icon/next-icon`
- `@/components/copy-to-clipboard`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `@/pages/agent/context`
- `antd`
- `lucide-react`
- `react`
- `react-i18next`
- `../ui/toggle-group`
- `./feedback-modal`
- `./hooks`
- `./prompt-modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/next-message-item`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/next-message-item/` directory
- Potential test file: `test_group-button.tsx`

## Keywords

../ui/toggle-group, ./feedback-modal, ./hooks, ./prompt-modal, @/assets/icon/next-icon, @/components/copy-to-clipboard, @/hooks/common-hooks, @/hooks/logic-hooks, @/pages/agent/context, AgentChatContext, AssistantGroupButton, Button, CopyToClipboard, DeleteOutlined, DislikeOutlined, FeedbackModal, Group, IProps, IRemoveMessageById, LikeOutlined, NotebookText, Partial, PauseCircleOutlined, PromptIcon, PromptModal, Radio, SoundOutlined, SyncOutlined, ToggleGroup, ToggleGroupItem, Tooltip, TypeScript, UserGroupButton, UserGroupButtonProps, ant, antd, handleLike, handleShowLogSheet, lucide-react, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
