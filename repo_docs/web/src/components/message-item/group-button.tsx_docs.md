# File Documentation: web/src/components/message-item/group-button.tsx

## File Metadata

- **Path**: `web/src/components/message-item/group-button.tsx`
- **Extension**: `.tsx`
- **Lines**: 146
- **Characters**: 3,965
- **Size**: 3,965 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { PromptIcon } from '@/assets/icon/next-icon';
import CopyToClipboard from '@/components/copy-to-clipboard';
import { useSetModalState } from '@/hooks/common-hooks';
import { IRemoveMessageById } from '@/hooks/logic-hooks';
import {
  DeleteOutlined,
  DislikeOutlined,
  LikeOutlined,
  PauseCircleOutlined,
  SoundOutlined,
  SyncOutlined,
} from '@ant-design/icons';
import { Radio, Tooltip } from 'antd';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
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
}

export const AssistantGroupButton = ({
  messageId,
  content,
  prompt,
  audioBinary,
  showLikeButton,
  showLoudspeaker = true,
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

  return (
    <>
      <Radio.Group size="small">
        <Radio.Button value="a">
          <CopyToClipboard text={content}></CopyToClipboard>
        </Radio.Button>
        {showLoudspeaker && (
          <Radio.Button value="b" onClick={handleRead}>
            <Tooltip title={t('chat.read')}>
              {isPlaying ? <PauseCircleOutlined /> : <SoundOutlined />}
            </Tooltip>
            <audio src="" ref={ref}></audio>
          </Radio.Button>
        )}
        {showLikeButton && (
          <>
            <Radio.Button value="c" onClick={handleLike}>
              <LikeOutlined />
            </Radio.Button>
            <Radio.Button value="d" onClick={showModal}>
              <DislikeOutlined />
            </Radio.Button>
          </>
        )}
        {prompt && (
          <Radio.Button value="e" onClick={showPromptModal}>
            <PromptIcon style={{ fontSize: '16px' }} />
          </Radio.Button>
        )}
      </Radio.Group>
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

This file is part of the RAGFlow repository located at `web/src/components/message-item/group-button.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 146 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `AssistantGroupButton`: Exported entity
- `UserGroupButton`: Exported entity

### Functions (3)

- `AssistantGroupButton()`: Function definition
- `handleLike()`: Function definition
- `UserGroupButton()`: Function definition

### Imports (11)

- `import { PromptIcon } from '@/assets/icon/next-icon';`
- `import CopyToClipboard from '@/components/copy-to-clipboard';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { IRemoveMessageById } from '@/hooks/logic-hooks';`
- `import {`
- `import { Radio, Tooltip } from 'antd';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import FeedbackModal from './feedback-modal';`
- `import { useRemoveMessage, useSendFeedback, useSpeech } from './hooks';`

## Code Structure Analysis

- Total lines: 146
- Blank lines: 8 (5.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~138


## Dependencies and Imports

- `@/assets/icon/next-icon`
- `@/components/copy-to-clipboard`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `antd`
- `react`
- `react-i18next`
- `./feedback-modal`
- `./hooks`
- `./prompt-modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/message-item`.

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

- Other files in `web/src/components/message-item/` directory
- Potential test file: `test_group-button.tsx`

## Keywords

./feedback-modal, ./hooks, ./prompt-modal, @/assets/icon/next-icon, @/components/copy-to-clipboard, @/hooks/common-hooks, @/hooks/logic-hooks, AssistantGroupButton, Button, CopyToClipboard, DeleteOutlined, DislikeOutlined, FeedbackModal, Group, IProps, IRemoveMessageById, LikeOutlined, Partial, PauseCircleOutlined, PromptIcon, PromptModal, Radio, SoundOutlined, SyncOutlined, Tooltip, TypeScript, UserGroupButton, UserGroupButtonProps, ant, antd, handleLike, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
