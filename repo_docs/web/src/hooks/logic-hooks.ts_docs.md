# File Documentation: web/src/hooks/logic-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/logic-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 749
- **Characters**: 19,836
- **Size**: 19,836 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { Authorization } from '@/constants/authorization';
import { MessageType } from '@/constants/chat';
import { LanguageTranslationMap } from '@/constants/common';
import { ResponseType } from '@/interfaces/database/base';
import { IAnswer, Message } from '@/interfaces/database/chat';
import { IKnowledgeFile } from '@/interfaces/database/knowledge';
import { IClientConversation, IMessage } from '@/pages/chat/interface';
import api from '@/utils/api';
import { getAuthorization } from '@/utils/authorization-util';
import { buildMessageUuid } from '@/utils/chat';
import { PaginationProps, message } from 'antd';
import { FormInstance } from 'antd/lib';
import axios from 'axios';
import { EventSourceParserStream } from 'eventsource-parser/stream';
import { has, isEmpty, omit } from 'lodash';
import {
  ChangeEventHandler,
  useCallback,
  useEffect,
  useMemo,
  useRef,
  useState,
} from 'react';
import { useTranslation } from 'react-i18next';
import { v4 as uuid } from 'uuid';
import { useTranslate } from './common-hooks';
import { useSetPaginationParams } from './route-hook';
import { useFetchTenantInfo, useSaveSetting } from './user-setting-hooks';

export function usePrevious<T>(value: T) {
  const ref = useRef<T>();
  useEffect(() => {
    ref.current = value;
  }, [value]);
  return ref.current;
}

export const useSetSelectedRecord = <T = IKnowledgeFile>() => {
  const [currentRecord, setCurrentRecord] = useState<T>({} as T);

  const setRecord = (record: T) => {
    setCurrentRecord(record);
  };

  return { currentRecord, setRecord };
};

export const useChangeLanguage = () => {
  const { i18n } = useTranslation();
  const { saveSetting } = useSaveSetting();

  const changeLanguage = (lng: string) => {
    i18n.changeLanguage(
      LanguageTranslationMap[lng as keyof typeof LanguageTranslationMap],
    );
    saveSetting({ language: lng });
  };

  return changeLanguage;
};

export const useGetPaginationWithRouter = () => {
  const { t } = useTranslate('common');
  const {
    setPaginationParams,
    page,
    size: pageSize,
  } = useSetPaginationParams();

  const onPageChange: PaginationProps['onChange'] = useCallback(
    (pageNumber: number, pageSize: number) => {
      setPaginationParams(pageNumber, pageSize);
    },
    [setPaginationParams],
  );

  const setCurrentPagination = useCallback(
    (pagination: { page: number; pageSize?: number }) => {
      if (pagination.pageSize !== pageSize) {
        pagination.page = 1; // Reset to first page if pageSize changes
      }
      setPaginationParams(pagination.page, pagination.pageSize);
    },
    [setPaginationParams, pageSize],
  );

  const pagination: PaginationProps = useMemo(() => {
    return {
      showQuickJumper: true,
      total: 0,
      showSizeChanger: true,
      current: page,
      pageSize: pageSize,
      pageSizeOptions: [1, 2, 10, 20, 50, 100],
      onChange: onPageChange,
      showTotal: (total) => `${t('total')} ${total}`,
    };
  }, [t, onPageChange, page, pageSize]);

  return {
    pagination,
    setPagination: setCurrentPagination,
  };
};

export const useHandleSearchChange = () => {
  const [searchString, setSearchString] = useState('');
  const { setPagination } = useGetPaginationWithRouter();
  const handleInputChange = useCallback(
    (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
      const value = e.target.value;
      setSearchString(value);
      setPagination({ page: 1 });
    },
    [setPagination],
  );

  return { handleInputChange, searchString };
};

export const useGetPagination = () => {
  const [pagination, setPagination] = useState({ page: 1, pageSize: 10 });
  const { t } = useTranslate('common');

  const onPageChange: PaginationProps['onChange'] = useCallback(
    (pageNumber: number, pageSize: number) => {
      setPagination({ page: pageNumber, pageSize });
    },
    [],
  );

  const currentPagination: PaginationProps = useMemo(() => {
    return {
      showQuickJumper: true,
      total: 0,
      showSizeChanger: true,
      current: pagination.page,
      pageSize: pagination.pageSize,
      pageSizeOptions: [1, 2, 10, 20, 50, 100],
      onChange: onPageChange,
      showTotal: (total) => `${t('total')} ${total}`,
    };
  }, [t, onPageChange, pagination]);

  return {
    pagination: currentPagination,
  };
};

export interface AppConf {
  appName: string;
}

export const useFetchAppConf = () => {
  const [appConf, setAppConf] = useState<AppConf>({} as AppConf);
  const fetchAppConf = useCallback(async () => {
    const ret = await axios.get('/conf.json');

    setAppConf(ret.data);
  }, []);

  useEffect(() => {
    fetchAppConf();
  }, [fetchAppConf]);

  return appConf;
};

function useSetDoneRecord() {
  const [doneRecord, setDoneRecord] = useState<Record<string, boolean>>({});

  const clearDoneRecord = useCallback(() => {
    setDoneRecord({});
  }, []);

  const setDoneRecordById = useCallback((id: string, val: boolean) => {
    setDoneRecord((prev) => ({ ...prev, [id]: val }));
  }, []);

  const allDone = useMemo(() => {
    return Object.values(doneRecord).every((val) => val);
  }, [doneRecord]);

  useEffect(() => {
    if (!isEmpty(doneRecord) && allDone) {
      clearDoneRecord();
    }
  }, [allDone, clearDoneRecord, doneRecord]);

  return {
    doneRecord,
    setDoneRecord,
    setDoneRecordById,
    clearDoneRecord,
    allDone,
  };
}

export const useSendMessageWithSse = (
  url: string = api.completeConversation,
) => {
  const [answer, setAnswer] = useState<IAnswer>({} as IAnswer);
  const [done, setDone] = useState(true);
  const { doneRecord, clearDoneRecord, setDoneRecordById, allDone } =
    useSetDoneRecord();
  const timer = useRef<any>();
  const sseRef = useRef<AbortController>();

  const initializeSseRef = useCallback(() => {
    sseRef.current = new AbortController();
  }, []);

  const resetAnswer = useCallback(() => {
    if (timer.current) {
      clearTimeout(timer.current);
    }
    timer.current = setTimeout(() => {
      setAnswer({} as IAnswer);
      clearTimeout(timer.current);
    }, 1000);
  }, []);

  const setDoneValue = useCallback(
    (body: any, value: boolean) => {
      if (has(body, 'chatBoxId')) {
        setDoneRecordById(body.chatBoxId, value);
      } else {
        setDone(value);
      }
    },
    [setDoneRecordById],
  );

  const send = useCallback(
    async (
      body: any,
      controller?: AbortController,
    ): Promise<{ response: Response; data: ResponseType } | undefined> => {
      initializeSseRef();
      try {
        setDoneValue(body, false);
        const response = await fetch(url, {
          method: 'POST',
          headers: {
            [Authorization]: getAuthorization(),
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(omit(body, 'chatBoxId')),
          signal: controller?.signal || sseRef.current?.signal,
        });

        const res = response.clone().json();

        const reader = response?.body
          ?.pipeThrough(new TextDecoderStream())
          .pipeThrough(new EventSourceParserStream())
          .getReader();

        while (true) {
          try {
            const x = await reader?.read();
            if (x) {
              const { done, value } = x;
              if (done) {
                resetAnswer();
                break;
              }
              try {
                const val = JSON.parse(value?.data || '');
                const d = val?.data;
                if (typeof d !== 'boolean') {
                  setAnswer({
                    ...d,
                    conversationId: body?.conversation_id,
                    chatBoxId: body.chatBoxId,
                  });
                }
              } catch (e) {
                // Swallow parse errors silently
              }
            }
          } catch (e) {
            if (e instanceof DOMException && e.name === 'AbortError') {
              console.log('Request was aborted by user or logic.');
              break;
            }
          }
        }
        setDoneValue(body, true);
        resetAnswer();
        return { data: await res, response };
      } catch (e) {
        setDoneValue(body, true);

        resetAnswer();
        // Swallow fetch errors silently
      }
    },
    [initializeSseRef, setDoneValue, url, resetAnswer],
  );

  const stopOutputMessage = useCallback(() => {
    sseRef.current?.abort();
  }, []);

  return {
    send,
    answer,
    done,
    doneRecord,
    allDone,
    setDone,
    resetAnswer,
    stopOutputMessage,
    clearDoneRecord,
  };
};

export const useSpeechWithSse = (url: string = api.tts) => {
  const read = useCallback(
    async (body: any) => {
      const response = await fetch(url, {
        method: 'POST',
        headers: {
          [Authorization]: getAuthorization(),
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(body),
      });
      try {
        const res = await response.clone().json();
        if (res?.code !== 0) {
          message.error(res?.message);
        }
      } catch (error) {
        // Swallow errors silently
      }
      return response;
    },
    [url],
  );

  return { read };
};

//#region chat hooks

export const useScrollToBottom = (
  messages?: unknown,
  containerRef?: React.RefObject<HTMLDivElement>,
) => {
  const ref = useRef<HTMLDivElement>(null);
  const [isAtBottom, setIsAtBottom] = useState(true);
  const isAtBottomRef = useRef(true);

  useEffect(() => {
    isAtBottomRef.current = isAtBottom;
  }, [isAtBottom]);

  const checkIfUserAtBottom = useCallback(() => {
    if (!containerRef?.current) return true;
    const { scrollTop, scrollHeight, clientHeight } = containerRef.current;
    return Math.abs(scrollTop + clientHeight - scrollHeight) < 25;
  }, [containerRef]);

  useEffect(() => {
    if (!containerRef?.current) return;
    const container = containerRef.current;

    const handleScroll = () => {
      setIsAtBottom(checkIfUserAtBottom());
    };

    container.addEventListener('scroll', handleScroll);
    handleScroll();
    return () => container.removeEventListener('scroll', handleScroll);
  }, [containerRef, checkIfUserAtBottom]);

  // Imperative scroll function
  const scrollToBottom = useCallback(() => {
    if (containerRef?.current) {
      const container = containerRef.current;
      container.scrollTo({
        top: container.scrollHeight - container.clientHeight,
        behavior: 'smooth',
      });
    }
  }, [containerRef]);

  useEffect(() => {
    if (!messages) return;
    if (!containerRef?.current) return;
    requestAnimationFrame(() => {
      setTimeout(() => {
        if (isAtBottomRef.current) {
          scrollToBottom();
        }
      }, 100);
    });
  }, [messages, containerRef, scrollToBottom]);

  return { scrollRef: ref, isAtBottom, scrollToBottom };
};

export const useHandleMessageInputChange = () => {
  const [value, setValue] = useState('');

  const handleInputChange: ChangeEventHandler<HTMLTextAreaElement> = (e) => {
    const value = e.target.value;
    const nextValue = value.replaceAll('\\n', '\n').replaceAll('\\t', '\t');
    setValue(nextValue);
  };

  return {
    handleInputChange,
    value,
    setValue,
  };
};

export const useSelectDerivedMessages = () => {
  const [derivedMessages, setDerivedMessages] = useState<IMessage[]>([]);

  const messageContainerRef = useRef<HTMLDivElement>(null);

  const { scrollRef, scrollToBottom } = useScrollToBottom(
    derivedMessages,
    messageContainerRef,
  );

  const addNewestQuestion = useCallback(
    (message: Message, answer: string = '') => {
      setDerivedMessages((pre) => {
        return [
          ...pre,
          {
            ...message,
            id: buildMessageUuid(message), // The message id is generated on the front end,
            // and the message id returned by the back end is the same as the question id,
            //  so that the pair of messages can be deleted together when deleting the message
          },
          {
            role: MessageType.Assistant,
            content: answer,
            id: buildMessageUuid({ ...message, role: MessageType.Assistant }),
          },
        ];
      });
    },
    [],
  );

  const addNewestOneQuestion = useCallback((message: Message) => {
    setDerivedMessages((pre) => {
      return [
        ...pre,
        {
          ...message,
          id: buildMessageUuid(message), // The message id is generated on the front end,
          // and the message id returned by the back end is the same as the question id,
          //  so that the pair of messages can be deleted together when deleting the message
        },
      ];
    });
  }, []);

  // Add the streaming message to the last item in the message list
  const addNewestAnswer = useCallback((answer: IAnswer) => {
    setDerivedMessages((pre) => {
      return [
        ...(pre?.slice(0, -1) ?? []),
        {
          role: MessageType.Assistant,
          content: answer.answer,
          reference: answer.reference,
          id: buildMessageUuid({
            id: answer.id,
            role: MessageType.Assistant,
          }),
          prompt: answer.prompt,
          audio_binary: answer.audio_binary,
          ...omit(answer, 'reference'),
        },
      ];
    });
  }, []);

  // Add the streaming message to the last item in the message list
  const addNewestOneAnswer = useCallback((answer: IAnswer) => {
    setDerivedMessages((pre) => {
      const idx = pre.findIndex((x) => x.id === answer.id);

      if (idx !== -1) {
        return pre.map((x) => {
          if (x.id === answer.id) {
            return { ...x, ...answer, content: answer.answer };
          }
          return x;
        });
      }

      return [
        ...(pre ?? []),
        {
          role: MessageType.Assistant,
          content: answer.answer,
          reference: answer.reference,
          id: buildMessageUuid({
            id: answer.id,
            role: MessageType.Assistant,
          }),
          prompt: answer.prompt,
          audio_binary: answer.audio_binary,
          ...omit(answer, 'reference'),
        },
      ];
    });
  }, []);

  const removeLatestMessage = useCallback(() => {
    setDerivedMessages((pre) => {
      const nextMessages = pre?.slice(0, -2) ?? [];
      return nextMessages;
    });
  }, []);

  const removeMessageById = useCallback(
    (messageId: string) => {
      setDerivedMessages((pre) => {
        const nextMessages = pre?.filter((x) => x.id !== messageId) ?? [];
        return nextMessages;
      });
    },
    [setDerivedMessages],
  );

  const removeMessagesAfterCurrentMessage = useCallback(
    (messageId: string) => {
      setDerivedMessages((pre) => {
        const index = pre.findIndex((x) => x.id === messageId);
        if (index !== -1) {
          let nextMessages = pre.slice(0, index + 2) ?? [];
          const latestMessage = nextMessages.at(-1);
          nextMessages = latestMessage
            ? [
                ...nextMessages.slice(0, -1),
                {
                  ...latestMessage,
                  content: '',
                  reference: undefined,
                  prompt: undefined,
                },
              ]
            : nextMessages;
          return nextMessages;
        }
        return pre;
      });
    },
    [setDerivedMessages],
  );

  const removeAllMessages = useCallback(() => {
    setDerivedMessages([]);
  }, [setDerivedMessages]);

  const removeAllMessagesExceptFirst = useCallback(() => {
    setDerivedMessages((list) => {
      if (list.length <= 1) {
        return list;
      }
      return list.slice(0, 1);
    });
  }, [setDerivedMessages]);

  return {
    scrollRef,
    messageContainerRef,
    derivedMessages,
    setDerivedMessages,
    addNewestQuestion,
    addNewestAnswer,
    removeLatestMessage,
    removeMessageById,
    addNewestOneQuestion,
    addNewestOneAnswer,
    removeMessagesAfterCurrentMessage,
    removeAllMessages,
    scrollToBottom,
    removeAllMessagesExceptFirst,
  };
};

export interface IRemoveMessageById {
  removeMessageById(messageId: string): void;
}

export const useRemoveMessagesAfterCurrentMessage = (
  setCurrentConversation: (
    callback: (state: IClientConversation) => IClientConversation,
  ) => void,
) => {
  const removeMessagesAfterCurrentMessage = useCallback(
    (messageId: string) => {
      setCurrentConversation((pre) => {
        const index = pre.message?.findIndex((x) => x.id === messageId);
        if (index !== -1) {
          let nextMessages = pre.message?.slice(0, index + 2) ?? [];
          const latestMessage = nextMessages.at(-1);
          nextMessages = latestMessage
            ? [
                ...nextMessages.slice(0, -1),
                {
                  ...latestMessage,
                  content: '',
                  reference: undefined,
                  prompt: undefined,
                },
              ]
            : nextMessages;
          return {
            ...pre,
            message: nextMessages,
          };
        }
        return pre;
      });
    },
    [setCurrentConversation],
  );

  return { removeMessagesAfterCurrentMessage };
};

export interface IRegenerateMessage {
  regenerateMessage?: (message: Message) => void;
}

export const useRegenerateMessage = ({
  removeMessagesAfterCurrentMessage,
  sendMessage,
  messages,
}: {
  removeMessagesAfterCurrentMessage(messageId: string): void;
  sendMessage({
    message,
  }: {
    message: Message;
    messages?: Message[];
  }): void | Promise<any>;
  messages: Message[];
}) => {
  const regenerateMessage = useCallback(
    async (message: Message) => {
      if (message.id) {
        removeMessagesAfterCurrentMessage(message.id);
        const index = messages.findIndex((x) => x.id === message.id);
        let nextMessages;
        if (index !== -1) {
          nextMessages = messages.slice(0, index);
        }
        sendMessage({
          message: { ...message, id: uuid() },
          messages: nextMessages,
        });
      }
    },
    [removeMessagesAfterCurrentMessage, sendMessage, messages],
  );

  return { regenerateMessage };
};

// #endregion

/**
 *
 * @param defaultId
 * used to switch between different items, similar to radio
 * @returns
 */
export const useSelectItem = (defaultId?: string) => {
  const [selectedId, setSelectedId] = useState('');

  const handleItemClick = useCallback(
    (id: string) => () => {
      setSelectedId(id);
    },
    [],
  );

  useEffect(() => {
    if (defaultId) {
      setSelectedId(defaultId);
    }
  }, [defaultId]);

  return { selectedId, handleItemClick };
};

export const useFetchModelId = () => {
  const { data: tenantInfo } = useFetchTenantInfo(true);

  return tenantInfo?.llm_id ?? '';
};

const ChunkTokenNumMap = {
  naive: 128,
  knowledge_graph: 8192,
};

export const useHandleChunkMethodSelectChange = (form: FormInstance) => {
  // const form = Form.useFormInstance();
  const handleChange = useCallback(
    (value: string) => {
      if (value in ChunkTokenNumMap) {
        form.setFieldValue(
          ['parser_config', 'chunk_token_num'],
          ChunkTokenNumMap[value as keyof typeof ChunkTokenNumMap],
        );
      }
    },
    [form],
  );

  return handleChange;
};

// reset form fields when modal is form, closed
export const useResetFormOnCloseModal = ({
  form,
  visible,
}: {
  form: FormInstance;
  visible?: boolean;
}) => {
  const prevOpenRef = useRef<boolean>();
  useEffect(() => {
    prevOpenRef.current = visible;
  }, [visible]);
  const prevOpen = prevOpenRef.current;

  useEffect(() => {
    if (!visible && prevOpen) {
      form.resetFields();
    }
  }, [form, prevOpen, visible]);
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/logic-hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 749 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (18)

- `usePrevious`: Exported entity
- `useSetSelectedRecord`: Exported entity
- `useChangeLanguage`: Exported entity
- `useGetPaginationWithRouter`: Exported entity
- `useHandleSearchChange`: Exported entity
- `useGetPagination`: Exported entity
- `useFetchAppConf`: Exported entity
- `useSendMessageWithSse`: Exported entity
- `useSpeechWithSse`: Exported entity
- `useScrollToBottom`: Exported entity
- `useHandleMessageInputChange`: Exported entity
- `useSelectDerivedMessages`: Exported entity
- `useRemoveMessagesAfterCurrentMessage`: Exported entity
- `useRegenerateMessage`: Exported entity
- `useSelectItem`: Exported entity
- `useFetchModelId`: Exported entity
- `useHandleChunkMethodSelectChange`: Exported entity
- `useResetFormOnCloseModal`: Exported entity

### Functions (58)

- `usePrevious()`: Function definition
- `ref()`: Function definition
- `setRecord()`: Function definition
- `useChangeLanguage()`: Function definition
- `changeLanguage()`: Function definition
- `useGetPaginationWithRouter()`: Function definition
- `setCurrentPagination()`: Function definition
- `useHandleSearchChange()`: Function definition
- `handleInputChange()`: Function definition
- `useGetPagination()`: Function definition
- `useFetchAppConf()`: Function definition
- `fetchAppConf()`: Function definition
- `ret()`: Function definition
- `useSetDoneRecord()`: Function definition
- `clearDoneRecord()`: Function definition
- `setDoneRecordById()`: Function definition
- `allDone()`: Function definition
- `useSendMessageWithSse()`: Function definition
- `initializeSseRef()`: Function definition
- `resetAnswer()`: Function definition

### Imports (21)

- `import { Authorization } from '@/constants/authorization';`
- `import { MessageType } from '@/constants/chat';`
- `import { LanguageTranslationMap } from '@/constants/common';`
- `import { ResponseType } from '@/interfaces/database/base';`
- `import { IAnswer, Message } from '@/interfaces/database/chat';`
- `import { IKnowledgeFile } from '@/interfaces/database/knowledge';`
- `import { IClientConversation, IMessage } from '@/pages/chat/interface';`
- `import api from '@/utils/api';`
- `import { getAuthorization } from '@/utils/authorization-util';`
- `import { buildMessageUuid } from '@/utils/chat';`

## Code Structure Analysis

- Total lines: 749
- Blank lines: 89 (11.9%)
- Comment lines: ~20 (2.7%)
- Code lines: ~640


## Dependencies and Imports

- `@/constants/authorization`
- `@/constants/chat`
- `@/constants/common`
- `@/interfaces/database/base`
- `@/interfaces/database/chat`
- `@/interfaces/database/knowledge`
- `@/pages/chat/interface`
- `@/utils/api`
- `@/utils/authorization-util`
- `@/utils/chat`
- `antd`
- `antd/lib`
- `axios`
- `eventsource-parser/stream`
- `lodash`
- `react-i18next`
- `uuid`
- `./common-hooks`
- `./route-hook`
- `./user-setting-hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/hooks/` directory
- Potential test file: `test_logic-hooks.ts`

## Keywords

./common-hooks, ./route-hook, ./user-setting-hooks, @/constants/authorization, @/constants/chat, @/constants/common, @/interfaces/database/base, @/interfaces/database/chat, @/interfaces/database/knowledge, @/pages/chat/interface, @/utils/api, @/utils/authorization-util, @/utils/chat, AbortController, AbortError, Add, AppConf, Assistant, Authorization, ChangeEvent, ChangeEventHandler, ChunkTokenNumMap, Content, DOMException, EventSourceParserStream, Form, FormInstance, HTMLDivElement, HTMLInputElement, HTMLTextAreaElement, IAnswer, IClientConversation, IKnowledgeFile, IMessage, IRegenerateMessage, IRemoveMessageById, Imperative, JSON, LanguageTranslationMap, Math, Message, MessageType, Object, POST, PaginationProps, Promise, React, Record, RefObject, Request...

---
*Generated by RAGFlow Repository Documentation Generator*
