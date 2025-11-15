# File Documentation: web/src/pages/chat/share/large.tsx

## File Metadata

- **Path**: `web/src/pages/chat/share/large.tsx`
- **Extension**: `.tsx`
- **Lines**: 125
- **Characters**: 3,793
- **Size**: 3,793 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import MessageInput from '@/components/message-input';
import MessageItem from '@/components/message-item';
import { useClickDrawer } from '@/components/pdf-drawer/hooks';
import { MessageType, SharedFrom } from '@/constants/chat';
import { useSendButtonDisabled } from '@/pages/chat/hooks';
import { Flex, Spin } from 'antd';
import React, { forwardRef, useMemo } from 'react';
import {
  useGetSharedChatSearchParams,
  useSendSharedMessage,
} from '../shared-hooks';
import { buildMessageItemReference } from '../utils';

import PdfDrawer from '@/components/pdf-drawer';
import { useFetchNextConversationSSE } from '@/hooks/chat-hooks';
import { useFetchFlowSSE } from '@/hooks/flow-hooks';
import i18n from '@/locales/config';
import { buildMessageUuidWithRole } from '@/utils/chat';
import styles from './index.less';

const ChatContainer = () => {
  const {
    sharedId: conversationId,
    from,
    locale,
    visibleAvatar,
  } = useGetSharedChatSearchParams();
  const { visible, hideModal, documentId, selectedChunk, clickDocumentButton } =
    useClickDrawer();

  const {
    handlePressEnter,
    handleInputChange,
    value,
    sendLoading,
    loading,
    ref,
    derivedMessages,
    hasError,
    stopOutputMessage,
  } = useSendSharedMessage();
  const sendDisabled = useSendButtonDisabled(value);

  const useFetchAvatar = useMemo(() => {
    return from === SharedFrom.Agent
      ? useFetchFlowSSE
      : useFetchNextConversationSSE;
  }, [from]);
  React.useEffect(() => {
    if (locale && i18n.language !== locale) {
      i18n.changeLanguage(locale);
    }
  }, [locale, visibleAvatar]);
  const { data: avatarData } = useFetchAvatar();

  if (!conversationId) {
    return <div>empty</div>;
  }

  return (
    <>
      <Flex flex={1} className={styles.chatContainer} vertical>
        <Flex flex={1} vertical className={styles.messageContainer}>
          <div>
            <Spin spinning={loading}>
              {derivedMessages?.map((message, i) => {
                return (
                  <MessageItem
                    visibleAvatar={visibleAvatar}
                    key={buildMessageUuidWithRole(message)}
                    avatarDialog={avatarData?.avatar}
                    item={message}
                    nickname="You"
                    reference={buildMessageItemReference(
                      {
                        message: derivedMessages,
                        reference: [],
                      },
                      message,
                    )}
                    loading={
                      message.role === MessageType.Assistant &&
                      sendLoading &&
                      derivedMessages?.length - 1 === i
                    }
                    index={i}
                    clickDocumentButton={clickDocumentButton}
                    showLikeButton={false}
                    showLoudspeaker={false}
                  ></MessageItem>
                );
              })}
            </Spin>
          </div>
          <div ref={ref} />
        </Flex>

        <MessageInput
          isShared
          value={value}
          disabled={hasError}
          sendDisabled={sendDisabled}
          conversationId={conversationId}
          onInputChange={handleInputChange}
          onPressEnter={handlePressEnter}
          sendLoading={sendLoading}
          uploadMethod="external_upload_and_parse"
          showUploadIcon={false}
          stopOutputMessage={stopOutputMessage}
        ></MessageInput>
      </Flex>
      {visible && (
        <PdfDrawer
          visible={visible}
          hideModal={hideModal}
          documentId={documentId}
          chunk={selectedChunk}
        ></PdfDrawer>
      )}
    </>
  );
};

export default forwardRef(ChatContainer);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chat/share/large.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 125 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `ChatContainer()`: Function definition
- `useFetchAvatar()`: Function definition

### Imports (15)

- `import MessageInput from '@/components/message-input';`
- `import MessageItem from '@/components/message-item';`
- `import { useClickDrawer } from '@/components/pdf-drawer/hooks';`
- `import { MessageType, SharedFrom } from '@/constants/chat';`
- `import { useSendButtonDisabled } from '@/pages/chat/hooks';`
- `import { Flex, Spin } from 'antd';`
- `import React, { forwardRef, useMemo } from 'react';`
- `import {`
- `import { buildMessageItemReference } from '../utils';`
- `import PdfDrawer from '@/components/pdf-drawer';`

## Code Structure Analysis

- Total lines: 125
- Blank lines: 9 (7.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~116


## Dependencies and Imports

- `@/components/message-input`
- `@/components/message-item`
- `@/components/pdf-drawer/hooks`
- `@/constants/chat`
- `@/pages/chat/hooks`
- `antd`
- `react`
- `../utils`
- `@/components/pdf-drawer`
- `@/hooks/chat-hooks`
- `@/hooks/flow-hooks`
- `@/locales/config`
- `@/utils/chat`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/share`.

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

- Other files in `web/src/pages/chat/share/` directory
- Potential test file: `test_large.tsx`

## Keywords

../utils, ./index.less, @/components/message-input, @/components/message-item, @/components/pdf-drawer, @/components/pdf-drawer/hooks, @/constants/chat, @/hooks/chat-hooks, @/hooks/flow-hooks, @/locales/config, @/pages/chat/hooks, @/utils/chat, Agent, Assistant, ChatContainer, Flex, MessageInput, MessageItem, MessageType, PdfDrawer, React, SharedFrom, Spin, TypeScript, You, antd, react, sendDisabled, useFetchAvatar

---
*Generated by RAGFlow Repository Documentation Generator*
