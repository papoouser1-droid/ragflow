# File Documentation: web/src/pages/agent/chat/use-send-agent-message.ts

## File Metadata

- **Path**: `web/src/pages/agent/chat/use-send-agent-message.ts`
- **Extension**: `.ts`
- **Lines**: 460
- **Characters**: 11,503
- **Size**: 11,509 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import sonnerMessage from '@/components/ui/message';
import { MessageType } from '@/constants/chat';
import {
  useHandleMessageInputChange,
  useSelectDerivedMessages,
} from '@/hooks/logic-hooks';
import {
  IAttachment,
  IEventList,
  IInputEvent,
  IMessageEndData,
  IMessageEndEvent,
  IMessageEvent,
  MessageEventType,
  useSendMessageBySSE,
} from '@/hooks/use-send-message';
import { Message } from '@/interfaces/database/chat';
import i18n from '@/locales/config';
import api from '@/utils/api';
import { get } from 'lodash';
import trim from 'lodash/trim';
import {
  useCallback,
  useContext,
  useEffect,
  useMemo,
  useRef,
  useState,
} from 'react';
import { useParams } from 'umi';
import { v4 as uuid } from 'uuid';
import { BeginId } from '../constant';
import { AgentChatLogContext } from '../context';
import { transferInputsArrayToObject } from '../form/begin-form/use-watch-change';
import {
  useIsTaskMode,
  useSelectBeginNodeDataInputs,
} from '../hooks/use-get-begin-query';
import { useStopMessage } from '../hooks/use-stop-message';
import { BeginQuery } from '../interface';
import useGraphStore from '../store';
import { receiveMessageError } from '../utils';

export function findMessageFromList(eventList: IEventList) {
  const messageEventList = eventList.filter(
    (x) => x.event === MessageEventType.Message,
  ) as IMessageEvent[];

  let nextContent = '';

  let startIndex = -1;
  let endIndex = -1;

  messageEventList.forEach((x, idx) => {
    const { data } = x;
    const { content, start_to_think, end_to_think } = data;
    if (start_to_think === true) {
      nextContent += '<think>' + content;
      startIndex = idx;
      return;
    }

    if (end_to_think === true) {
      endIndex = idx;
      nextContent += content + '</think>';
      return;
    }

    nextContent += content;
  });

  const currentIdx = messageEventList.length - 1;

  // Make sure that after start_to_think === true and before end_to_think === true, add a </think> tag at the end.
  if (startIndex >= 0 && startIndex <= currentIdx && endIndex === -1) {
    nextContent += '</think>';
  }

  const workflowFinished = eventList.find(
    (x) => x.event === MessageEventType.WorkflowFinished,
  ) as IMessageEvent;
  return {
    id: eventList[0]?.message_id,
    content: nextContent,
    attachment: workflowFinished?.data?.outputs?.attachment || {},
  };
}

export function findInputFromList(eventList: IEventList) {
  const inputEvent = eventList.find(
    (x) => x.event === MessageEventType.UserInputs,
  ) as IInputEvent;

  if (!inputEvent) {
    return {};
  }

  return {
    id: inputEvent?.message_id,
    data: inputEvent?.data,
  };
}

export function getLatestError(eventList: IEventList) {
  return get(eventList.at(-1), 'data.outputs._ERROR');
}

export const useGetBeginNodePrologue = () => {
  const getNode = useGraphStore((state) => state.getNode);
  const formData = get(getNode(BeginId), 'data.form', {});

  return useMemo(() => {
    if (formData?.enablePrologue) {
      return formData?.prologue;
    }
  }, [formData?.enablePrologue, formData?.prologue]);
};

export function useFindMessageReference(answerList: IEventList) {
  const [messageEndEventList, setMessageEndEventList] = useState<
    IMessageEndEvent[]
  >([]);

  const findReferenceByMessageId = useCallback(
    (messageId: string) => {
      const event = messageEndEventList.find(
        (item) => item.message_id === messageId,
      );
      if (event) {
        return (event?.data as IMessageEndData)?.reference;
      }
    },
    [messageEndEventList],
  );

  useEffect(() => {
    const messageEndEvent = answerList.find(
      (x) => x.event === MessageEventType.MessageEnd,
    );
    if (messageEndEvent) {
      setMessageEndEventList((list) => {
        const nextList = [...list];
        if (
          nextList.every((x) => x.message_id !== messageEndEvent.message_id)
        ) {
          nextList.push(messageEndEvent as IMessageEndEvent);
        }
        return nextList;
      });
    }
  }, [answerList]);

  return { findReferenceByMessageId };
}

interface UploadResponseDataType {
  created_at: number;
  created_by: string;
  extension: string;
  id: string;
  mime_type: string;
  name: string;
  preview_url: null;
  size: number;
}

export function useSetUploadResponseData() {
  const [uploadResponseList, setUploadResponseList] = useState<
    UploadResponseDataType[]
  >([]);
  const [fileList, setFileList] = useState<File[]>([]);

  const append = useCallback((data: UploadResponseDataType, files: File[]) => {
    setUploadResponseList((prev) => [...prev, data]);
    setFileList((pre) => [...pre, ...files]);
  }, []);

  const clear = useCallback(() => {
    setUploadResponseList([]);
    setFileList([]);
  }, []);

  return {
    uploadResponseList,
    fileList,
    setUploadResponseList,
    appendUploadResponseList: append,
    clearUploadResponseList: clear,
  };
}

export const buildRequestBody = (value: string = '') => {
  const id = uuid();
  const msgBody = {
    id,
    content: value.trim(),
    role: MessageType.User,
  };

  return msgBody;
};

export const useSendAgentMessage = ({
  url,
  addEventList,
  beginParams,
  isShared,
  refetch,
  isTaskMode: isTask,
}: {
  url?: string;
  addEventList?: (data: IEventList, messageId: string) => void;
  beginParams?: any[];
  isShared?: boolean;
  refetch?: () => void;
  isTaskMode?: boolean;
}) => {
  const { id: agentId } = useParams();
  const { handleInputChange, value, setValue } = useHandleMessageInputChange();
  const inputs = useSelectBeginNodeDataInputs();
  const [sessionId, setSessionId] = useState<string | null>(null);
  const { send, answerList, done, stopOutputMessage, resetAnswerList } =
    useSendMessageBySSE(url || api.runCanvas);
  const messageId = useMemo(() => {
    return answerList[0]?.message_id;
  }, [answerList]);

  const isTaskMode = useIsTaskMode(isTask);

  const { findReferenceByMessageId } = useFindMessageReference(answerList);
  const prologue = useGetBeginNodePrologue();
  const {
    derivedMessages,
    scrollRef,
    messageContainerRef,
    removeLatestMessage,
    removeMessageById,
    addNewestOneQuestion,
    addNewestOneAnswer,
    removeAllMessages,
    removeAllMessagesExceptFirst,
    scrollToBottom,
  } = useSelectDerivedMessages();
  const { addEventList: addEventListFun } = useContext(AgentChatLogContext);
  const {
    appendUploadResponseList,
    clearUploadResponseList,
    uploadResponseList,
    fileList,
  } = useSetUploadResponseData();

  const { stopMessage } = useStopMessage();

  const stopConversation = useCallback(() => {
    const taskId = answerList.at(0)?.task_id;
    stopOutputMessage();
    stopMessage(taskId);
  }, [answerList, stopMessage, stopOutputMessage]);

  const sendMessage = useCallback(
    async ({
      message,
      beginInputs,
    }: {
      message: Message;
      messages?: Message[];
      beginInputs?: BeginQuery[];
    }) => {
      const params: Record<string, unknown> = {
        id: agentId,
      };

      params.running_hint_text = i18n.t('flow.runningHintText', {
        defaultValue: 'is running...🕞',
      });
      if (typeof message.content === 'string') {
        const query = inputs;

        params.query = message.content;
        // params.message_id = message.id;
        params.inputs = transferInputsArrayToObject(
          beginInputs || beginParams || query,
        ); // begin operator inputs

        params.files = uploadResponseList;

        params.session_id = sessionId;
      }

      try {
        const res = await send(params);

        clearUploadResponseList();

        if (receiveMessageError(res)) {
          sonnerMessage.error(res?.data?.message);

          // cancel loading
          setValue(message.content);
          removeLatestMessage();
        } else {
          refetch?.(); // pull the message list after sending the message successfully
        }
      } catch (error) {
        console.log('🚀 ~ useSendAgentMessage ~ error:', error);
      }
    },
    [
      agentId,
      inputs,
      beginParams,
      uploadResponseList,
      sessionId,
      send,
      clearUploadResponseList,
      setValue,
      removeLatestMessage,
      refetch,
    ],
  );

  const sendFormMessage = useCallback(
    async (body: { id?: string; inputs: Record<string, BeginQuery> }) => {
      addNewestOneQuestion({
        content: Object.entries(body.inputs)
          .map(([key, val]) => `${key}: ${val.value}`)
          .join('<br/>'),
        role: MessageType.User,
      });
      await send({ ...body, session_id: sessionId });
      refetch?.();
    },
    [addNewestOneQuestion, refetch, send, sessionId],
  );

  // reset session
  const resetSession = useCallback(() => {
    stopConversation();
    resetAnswerList();
    setSessionId(null);
    if (isTaskMode) {
      removeAllMessages();
    } else {
      removeAllMessagesExceptFirst();
    }
  }, [
    stopConversation,
    resetAnswerList,
    isTaskMode,
    removeAllMessages,
    removeAllMessagesExceptFirst,
  ]);

  const handlePressEnter = useCallback(() => {
    if (trim(value) === '') return;
    const msgBody = buildRequestBody(value);
    if (done) {
      setValue('');
      sendMessage({
        message: msgBody,
      });
    }
    addNewestOneQuestion({ ...msgBody, files: fileList });
    setTimeout(() => {
      scrollToBottom();
    }, 100);
  }, [
    value,
    done,
    addNewestOneQuestion,
    fileList,
    setValue,
    sendMessage,
    scrollToBottom,
  ]);

  const sendedTaskMessage = useRef<boolean>(false);

  const sendMessageInTaskMode = useCallback(() => {
    if (isShared || !isTaskMode || sendedTaskMessage.current) {
      return;
    }
    const msgBody = buildRequestBody('');

    sendMessage({
      message: msgBody,
    });
    sendedTaskMessage.current = true;
  }, [isShared, isTaskMode, sendMessage]);

  useEffect(() => {
    sendMessageInTaskMode();
  }, [sendMessageInTaskMode]);

  useEffect(() => {
    const { content, id, attachment } = findMessageFromList(answerList);
    const inputAnswer = findInputFromList(answerList);
    const answer = content || getLatestError(answerList);
    if (answerList.length > 0) {
      addNewestOneAnswer({
        answer: answer ?? '',
        attachment: attachment as IAttachment,
        id: id,
        ...inputAnswer,
      });
    }
  }, [answerList, addNewestOneAnswer]);

  useEffect(() => {
    if (isTaskMode) {
      return;
    }
    if (prologue) {
      addNewestOneAnswer({
        answer: prologue,
      });
    }
  }, [
    addNewestOneAnswer,
    agentId,
    isTaskMode,
    prologue,
    send,
    sendFormMessage,
  ]);

  useEffect(() => {
    if (typeof addEventList === 'function') {
      addEventList(answerList, messageId);
    } else if (typeof addEventListFun === 'function') {
      addEventListFun(answerList, messageId);
    }
  }, [addEventList, answerList, addEventListFun, messageId]);

  useEffect(() => {
    if (answerList[0]?.session_id) {
      setSessionId(answerList[0]?.session_id);
    }
  }, [answerList]);

  return {
    value,
    sendLoading: !done,
    derivedMessages,
    scrollRef,
    messageContainerRef,
    handlePressEnter,
    handleInputChange,
    removeMessageById,
    stopOutputMessage: stopConversation,
    send,
    sendFormMessage,
    resetSession,
    findReferenceByMessageId,
    appendUploadResponseList,
    addNewestOneAnswer,
    sendMessage,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/chat/use-send-agent-message.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 460 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (8)

- `findMessageFromList`: Exported entity
- `findInputFromList`: Exported entity
- `getLatestError`: Exported entity
- `useGetBeginNodePrologue`: Exported entity
- `useFindMessageReference`: Exported entity
- `useSetUploadResponseData`: Exported entity
- `buildRequestBody`: Exported entity
- `useSendAgentMessage`: Exported entity

### Functions (28)

- `findMessageFromList()`: Function definition
- `messageEventList()`: Function definition
- `workflowFinished()`: Function definition
- `findInputFromList()`: Function definition
- `inputEvent()`: Function definition
- `getLatestError()`: Function definition
- `useGetBeginNodePrologue()`: Function definition
- `getNode()`: Function definition
- `formData()`: Function definition
- `useFindMessageReference()`: Function definition
- `findReferenceByMessageId()`: Function definition
- `event()`: Function definition
- `messageEndEvent()`: Function definition
- `nextList()`: Function definition
- `useSetUploadResponseData()`: Function definition
- `append()`: Function definition
- `clear()`: Function definition
- `buildRequestBody()`: Function definition
- `useSendAgentMessage()`: Function definition
- `messageId()`: Function definition

### Imports (20)

- `import sonnerMessage from '@/components/ui/message';`
- `import { MessageType } from '@/constants/chat';`
- `import {`
- `import {`
- `import { Message } from '@/interfaces/database/chat';`
- `import i18n from '@/locales/config';`
- `import api from '@/utils/api';`
- `import { get } from 'lodash';`
- `import trim from 'lodash/trim';`
- `import {`

## Code Structure Analysis

- Total lines: 460
- Blank lines: 53 (11.5%)
- Comment lines: ~4 (0.9%)
- Code lines: ~403


## Dependencies and Imports

- `@/components/ui/message`
- `@/constants/chat`
- `@/interfaces/database/chat`
- `@/locales/config`
- `@/utils/api`
- `lodash`
- `lodash/trim`
- `umi`
- `uuid`
- `../constant`
- `../context`
- `../form/begin-form/use-watch-change`
- `../hooks/use-stop-message`
- `../interface`
- `../store`
- `../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/chat`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
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

- Other files in `web/src/pages/agent/chat/` directory
- Potential test file: `test_use-send-agent-message.ts`

## Keywords

../constant, ../context, ../form/begin-form/use-watch-change, ../hooks/use-stop-message, ../interface, ../store, ../utils, @/components/ui/message, @/constants/chat, @/interfaces/database/chat, @/locales/config, @/utils/api, AgentChatLogContext, BeginId, BeginQuery, File, IAttachment, IEventList, IInputEvent, IMessageEndData, IMessageEndEvent, IMessageEvent, Make, Message, MessageEnd, MessageEventType, MessageType, Object, Record, TypeScript, UploadResponseDataType, User, UserInputs, WorkflowFinished, answer, append, buildRequestBody, clear, currentIdx, endIndex, event, findInputFromList, findMessageFromList, findReferenceByMessageId, formData, getLatestError, getNode, handlePressEnter, id, inputAnswer...

---
*Generated by RAGFlow Repository Documentation Generator*
