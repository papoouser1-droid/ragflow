# File Documentation: web/src/components/message-item/hooks.ts

## File Metadata

- **Path**: `web/src/components/message-item/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 117
- **Characters**: 3,174
- **Size**: 3,174 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useDeleteMessage, useFeedback } from '@/hooks/chat-hooks';
import { useSetModalState } from '@/hooks/common-hooks';
import { IRemoveMessageById, useSpeechWithSse } from '@/hooks/logic-hooks';
import { IFeedbackRequestBody } from '@/interfaces/request/chat';
import { hexStringToUint8Array } from '@/utils/common-util';
import { SpeechPlayer } from 'openai-speech-stream-player';
import { useCallback, useEffect, useRef, useState } from 'react';

export const useSendFeedback = (messageId: string) => {
  const { visible, hideModal, showModal } = useSetModalState();
  const { feedback, loading } = useFeedback();

  const onFeedbackOk = useCallback(
    async (params: IFeedbackRequestBody) => {
      const ret = await feedback({
        ...params,
        messageId: messageId,
      });

      if (ret === 0) {
        hideModal();
      }
    },
    [feedback, hideModal, messageId],
  );

  return {
    loading,
    onFeedbackOk,
    visible,
    hideModal,
    showModal,
  };
};

export const useRemoveMessage = (
  messageId: string,
  removeMessageById?: IRemoveMessageById['removeMessageById'],
) => {
  const { deleteMessage, loading } = useDeleteMessage();

  const onRemoveMessage = useCallback(async () => {
    if (messageId) {
      const code = await deleteMessage(messageId);
      if (code === 0) {
        removeMessageById?.(messageId);
      }
    }
  }, [deleteMessage, messageId, removeMessageById]);

  return { onRemoveMessage, loading };
};

export const useSpeech = (content: string, audioBinary?: string) => {
  const ref = useRef<HTMLAudioElement>(null);
  const { read } = useSpeechWithSse();
  const player = useRef<SpeechPlayer>();
  const [isPlaying, setIsPlaying] = useState<boolean>(false);

  const initialize = useCallback(async () => {
    player.current = new SpeechPlayer({
      audio: ref.current!,
      onPlaying: () => {
        setIsPlaying(true);
      },
      onPause: () => {
        setIsPlaying(false);
      },
      onChunkEnd: () => {},
      mimeType: MediaSource.isTypeSupported('audio/mpeg')
        ? 'audio/mpeg'
        : 'audio/mp4; codecs="mp4a.40.2"', // https://stackoverflow.com/questions/64079424/cannot-replay-mp3-in-firefox-using-mediasource-even-though-it-works-in-chrome
    });
    await player.current.init();
  }, []);

  const pause = useCallback(() => {
    player.current?.pause();
  }, []);

  const speech = useCallback(async () => {
    const response = await read({ text: content });
    if (response) {
      player?.current?.feedWithResponse(response);
    }
  }, [read, content]);

  const handleRead = useCallback(async () => {
    if (isPlaying) {
      setIsPlaying(false);
      pause();
    } else {
      setIsPlaying(true);
      speech();
    }
  }, [setIsPlaying, speech, isPlaying, pause]);

  useEffect(() => {
    if (audioBinary) {
      const units = hexStringToUint8Array(audioBinary);
      if (units) {
        try {
          player.current?.feed(units);
        } catch (error) {
          console.warn(error);
        }
      }
    }
  }, [audioBinary]);

  useEffect(() => {
    initialize();
  }, [initialize]);

  return { ref, handleRead, isPlaying };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/message-item/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 117 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useSendFeedback`: Exported entity
- `useRemoveMessage`: Exported entity
- `useSpeech`: Exported entity

### Functions (10)

- `useSendFeedback()`: Function definition
- `onFeedbackOk()`: Function definition
- `useRemoveMessage()`: Function definition
- `onRemoveMessage()`: Function definition
- `useSpeech()`: Function definition
- `initialize()`: Function definition
- `pause()`: Function definition
- `speech()`: Function definition
- `handleRead()`: Function definition
- `units()`: Function definition

### Imports (7)

- `import { useDeleteMessage, useFeedback } from '@/hooks/chat-hooks';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { IRemoveMessageById, useSpeechWithSse } from '@/hooks/logic-hooks';`
- `import { IFeedbackRequestBody } from '@/interfaces/request/chat';`
- `import { hexStringToUint8Array } from '@/utils/common-util';`
- `import { SpeechPlayer } from 'openai-speech-stream-player';`
- `import { useCallback, useEffect, useRef, useState } from 'react';`

## Code Structure Analysis

- Total lines: 117
- Blank lines: 16 (13.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~101


## Dependencies and Imports

- `@/hooks/chat-hooks`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `@/interfaces/request/chat`
- `@/utils/common-util`
- `openai-speech-stream-player`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/message-item`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/message-item/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/hooks/chat-hooks, @/hooks/common-hooks, @/hooks/logic-hooks, @/interfaces/request/chat, @/utils/common-util, HTMLAudioElement, IFeedbackRequestBody, IRemoveMessageById, MediaSource, SpeechPlayer, TypeScript, code, handleRead, initialize, onFeedbackOk, onRemoveMessage, openai-speech-stream-player, pause, player, react, ref, response, ret, speech, units, useRemoveMessage, useSendFeedback, useSpeech

---
*Generated by RAGFlow Repository Documentation Generator*
