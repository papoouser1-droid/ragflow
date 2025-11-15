# File Documentation: web/src/pages/agent/hooks/use-cache-chat-log.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-cache-chat-log.ts`
- **Extension**: `.ts`
- **Lines**: 94
- **Characters**: 2,469
- **Size**: 2,469 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  IEventList,
  INodeEvent,
  MessageEventType,
} from '@/hooks/use-send-message';
import { useCallback, useEffect, useMemo, useState } from 'react';

export const ExcludeTypes = [
  MessageEventType.Message,
  MessageEventType.MessageEnd,
];

export function useCacheChatLog() {
  const [eventList, setEventList] = useState<IEventList>([]);
  const [messageIdPool, setMessageIdPool] = useState<
    Record<string, IEventList>
  >({});

  const [currentMessageId, setCurrentMessageId] = useState('');
  useEffect(() => {
    setMessageIdPool((prev) => ({ ...prev, [currentMessageId]: eventList }));
  }, [currentMessageId, eventList]);

  const filterEventListByMessageId = useCallback(
    (messageId: string) => {
      return messageIdPool[messageId]?.filter(
        (x) => x.message_id === messageId,
      );
    },
    [messageIdPool],
  );

  const filterEventListByEventType = useCallback(
    (eventType: string) => {
      return messageIdPool[currentMessageId]?.filter(
        (x) => x.event === eventType,
      );
    },
    [messageIdPool, currentMessageId],
  );

  const clearEventList = useCallback(() => {
    setEventList([]);
    setMessageIdPool({});
  }, []);

  const addEventList = useCallback((events: IEventList, message_id: string) => {
    setEventList((x) => {
      const list = [...x, ...events];
      setMessageIdPool((prev) => ({ ...prev, [message_id]: list }));
      return list;
    });
  }, []);

  const currentEventListWithoutMessage = useMemo(() => {
    const list = messageIdPool[currentMessageId]?.filter(
      (x) =>
        x.message_id === currentMessageId &&
        ExcludeTypes.every((y) => y !== x.event),
    );
    return list as INodeEvent[];
  }, [currentMessageId, messageIdPool]);

  const currentEventListWithoutMessageById = useCallback(
    (messageId: string) => {
      const list = messageIdPool[messageId]?.filter(
        (x) =>
          x.message_id === messageId &&
          ExcludeTypes.every((y) => y !== x.event),
      );
      return list as INodeEvent[];
    },
    [messageIdPool],
  );

  const currentTaskId = useMemo(() => {
    return eventList.at(-1)?.task_id;
  }, [eventList]);

  return {
    eventList,
    currentEventListWithoutMessage,
    currentEventListWithoutMessageById,
    setEventList,
    clearEventList,
    addEventList,
    filterEventListByEventType,
    filterEventListByMessageId,
    setCurrentMessageId,
    currentMessageId,
    currentTaskId,
  };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/hooks/use-cache-chat-log.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 94 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `ExcludeTypes`: Exported entity
- `useCacheChatLog`: Exported entity

### Functions (11)

- `useCacheChatLog()`: Function definition
- `filterEventListByMessageId()`: Function definition
- `filterEventListByEventType()`: Function definition
- `clearEventList()`: Function definition
- `addEventList()`: Function definition
- `list()`: Function definition
- `currentEventListWithoutMessage()`: Function definition
- `list()`: Function definition
- `currentEventListWithoutMessageById()`: Function definition
- `list()`: Function definition
- `currentTaskId()`: Function definition

### Imports (2)

- `import {`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 12 (12.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~82


## Dependencies and Imports

- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-cache-chat-log.ts`

## Keywords

ExcludeTypes, IEventList, INodeEvent, Message, MessageEnd, MessageEventType, Record, TypeScript, addEventList, clearEventList, currentEventListWithoutMessage, currentEventListWithoutMessageById, currentTaskId, filterEventListByEventType, filterEventListByMessageId, list, react, useCacheChatLog

---
*Generated by RAGFlow Repository Documentation Generator*
