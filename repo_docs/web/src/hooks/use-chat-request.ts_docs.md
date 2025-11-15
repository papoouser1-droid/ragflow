# File Documentation: web/src/hooks/use-chat-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-chat-request.ts`
- **Extension**: `.ts`
- **Lines**: 533
- **Characters**: 14,160
- **Size**: 14,160 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { FileUploadProps } from '@/components/file-upload';
import message from '@/components/ui/message';
import { ChatSearchParams } from '@/constants/chat';
import {
  IConversation,
  IDialog,
  IExternalChatInfo,
} from '@/interfaces/database/chat';
import { IAskRequestBody } from '@/interfaces/request/chat';
import { IClientConversation } from '@/pages/next-chats/chat/interface';
import { useGetSharedChatSearchParams } from '@/pages/next-chats/hooks/use-send-shared-message';
import { isConversationIdExist } from '@/pages/next-chats/utils';
import chatService from '@/services/next-chat-service';
import { buildMessageListWithUuid, getConversationId } from '@/utils/chat';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { has } from 'lodash';
import { useCallback, useMemo, useRef } from 'react';
import { useTranslation } from 'react-i18next';
import { useParams, useSearchParams } from 'umi';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import { useHandleSearchStrChange } from './logic-hooks/use-change-search';

export const enum ChatApiAction {
  FetchDialogList = 'fetchDialogList',
  RemoveDialog = 'removeDialog',
  SetDialog = 'setDialog',
  FetchDialog = 'fetchDialog',
  FetchConversationList = 'fetchConversationList',
  FetchConversation = 'fetchConversation',
  UpdateConversation = 'updateConversation',
  RemoveConversation = 'removeConversation',
  DeleteMessage = 'deleteMessage',
  FetchMindMap = 'fetchMindMap',
  FetchRelatedQuestions = 'fetchRelatedQuestions',
  UploadAndParse = 'upload_and_parse',
  FetchExternalChatInfo = 'fetchExternalChatInfo',
}

export const useGetChatSearchParams = () => {
  const [currentQueryParameters] = useSearchParams();

  return {
    dialogId: currentQueryParameters.get(ChatSearchParams.DialogId) || '',
    conversationId:
      currentQueryParameters.get(ChatSearchParams.ConversationId) || '',
    isNew: currentQueryParameters.get(ChatSearchParams.isNew) || '',
  };
};

export const useClickDialogCard = () => {
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  const [_, setSearchParams] = useSearchParams();

  const newQueryParameters: URLSearchParams = useMemo(() => {
    return new URLSearchParams();
  }, []);

  const handleClickDialog = useCallback(
    (dialogId: string) => {
      newQueryParameters.set(ChatSearchParams.DialogId, dialogId);
      // newQueryParameters.set(
      //   ChatSearchParams.ConversationId,
      //   EmptyConversationId,
      // );
      setSearchParams(newQueryParameters);
    },
    [newQueryParameters, setSearchParams],
  );

  return { handleClickDialog };
};

export const useFetchDialogList = () => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<{ dialogs: IDialog[]; total: number }>({
    queryKey: [
      ChatApiAction.FetchDialogList,
      {
        debouncedSearchString,
        ...pagination,
      },
    ],
    initialData: { dialogs: [], total: 0 },
    gcTime: 0,
    refetchOnWindowFocus: false,
    queryFn: async () => {
      const { data } = await chatService.listDialog(
        {
          params: {
            keywords: debouncedSearchString,
            page_size: pagination.pageSize,
            page: pagination.current,
          },
          data: {},
        },
        true,
      );

      return data?.data ?? { dialogs: [], total: 0 };
    },
  });

  const onInputChange: React.ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      handleInputChange(e);
    },
    [handleInputChange],
  );

  return {
    data,
    loading,
    refetch,
    searchString,
    handleInputChange: onInputChange,
    pagination: { ...pagination, total: data?.total },
    setPagination,
  };
};

export const useRemoveDialog = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.RemoveDialog],
    mutationFn: async (dialogIds: string[]) => {
      const { data } = await chatService.removeDialog({ dialogIds });
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['fetchDialogList'] });

        message.success(t('message.deleted'));
      }
      return data.code;
    },
  });

  return { data, loading, removeDialog: mutateAsync };
};

export const useSetDialog = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.SetDialog],
    mutationFn: async (params: Partial<IDialog>) => {
      const { data } = await chatService.setDialog(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({
          exact: false,
          queryKey: [ChatApiAction.FetchDialogList],
        });

        queryClient.invalidateQueries({
          queryKey: [ChatApiAction.FetchDialog],
        });

        message.success(
          t(`message.${params.dialog_id ? 'modified' : 'created'}`),
        );
      }
      return data?.code;
    },
  });

  return { data, loading, setDialog: mutateAsync };
};

export const useFetchDialog = () => {
  const { id } = useParams();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IDialog>({
    queryKey: [ChatApiAction.FetchDialog, id],
    gcTime: 0,
    initialData: {} as IDialog,
    enabled: !!id,
    refetchOnWindowFocus: false,
    queryFn: async () => {
      const { data } = await chatService.getDialog(
        { params: { dialogId: id } },
        true,
      );

      return data?.data ?? ({} as IDialog);
    },
  });

  return { data, loading, refetch };
};

//#region Conversation

export const useClickConversationCard = () => {
  const [currentQueryParameters, setSearchParams] = useSearchParams();
  const newQueryParameters: URLSearchParams = useMemo(
    () => new URLSearchParams(currentQueryParameters.toString()),
    [currentQueryParameters],
  );

  const handleClickConversation = useCallback(
    (conversationId: string, isNew: string) => {
      newQueryParameters.set(ChatSearchParams.ConversationId, conversationId);
      newQueryParameters.set(ChatSearchParams.isNew, isNew);
      setSearchParams(newQueryParameters);
    },
    [setSearchParams, newQueryParameters],
  );

  return { handleClickConversation };
};

export const useFetchConversationList = () => {
  const { id } = useParams();
  const { handleClickConversation } = useClickConversationCard();

  const { searchString, handleInputChange } = useHandleSearchStrChange();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IConversation[]>({
    queryKey: [ChatApiAction.FetchConversationList, id],
    initialData: [],
    gcTime: 0,
    refetchOnWindowFocus: false,
    enabled: !!id,
    select(data) {
      return searchString
        ? data.filter((x) => x.name.includes(searchString))
        : data;
    },
    queryFn: async () => {
      const { data } = await chatService.listConversation(
        { params: { dialog_id: id } },
        true,
      );
      if (data.code === 0) {
        if (data.data.length > 0) {
          handleClickConversation(data.data[0].id, '');
        } else {
          handleClickConversation('', '');
        }
      }
      return data?.data;
    },
  });

  return { data, loading, refetch, searchString, handleInputChange };
};

export const useFetchConversation = () => {
  const { isNew, conversationId } = useGetChatSearchParams();
  const { sharedId } = useGetSharedChatSearchParams();
  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IClientConversation>({
    queryKey: [ChatApiAction.FetchConversation, conversationId],
    initialData: {} as IClientConversation,
    // enabled: isConversationIdExist(conversationId),
    gcTime: 0,
    refetchOnWindowFocus: false,
    queryFn: async () => {
      if (
        isNew !== 'true' &&
        isConversationIdExist(sharedId || conversationId)
      ) {
        const { data } = await chatService.getConversation(
          {
            params: {
              conversationId: conversationId || sharedId,
            },
          },
          true,
        );

        const conversation = data?.data ?? {};

        const messageList = buildMessageListWithUuid(conversation?.message);

        return { ...conversation, message: messageList };
      }
      return { message: [] };
    },
  });

  return { data, loading, refetch };
};

export const useUpdateConversation = () => {
  const { t } = useTranslation();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.UpdateConversation],
    mutationFn: async (params: Record<string, any>) => {
      const { data } = await chatService.setConversation({
        ...params,
        conversation_id: params.conversation_id
          ? params.conversation_id
          : getConversationId(),
      });
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [ChatApiAction.FetchConversationList],
        });
        message.success(t(`message.modified`));
      }
      return data;
    },
  });

  return { data, loading, updateConversation: mutateAsync };
};

export const useRemoveConversation = () => {
  const queryClient = useQueryClient();
  const { dialogId } = useGetChatSearchParams();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.RemoveConversation],
    mutationFn: async (conversationIds: string[]) => {
      const { data } = await chatService.removeConversation({
        conversationIds,
        dialogId,
      });
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [ChatApiAction.FetchConversationList],
        });
      }
      return data.code;
    },
  });

  return { data, loading, removeConversation: mutateAsync };
};

export const useDeleteMessage = () => {
  const { conversationId } = useGetChatSearchParams();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.DeleteMessage],
    mutationFn: async (messageId: string) => {
      const { data } = await chatService.deleteMessage({
        messageId,
        conversationId,
      });

      if (data.code === 0) {
        message.success(t(`message.deleted`));
      }

      return data.code;
    },
  });

  return { data, loading, deleteMessage: mutateAsync };
};

type UploadParameters = Parameters<NonNullable<FileUploadProps['onUpload']>>;

type X = {
  file: UploadParameters[0][0];
  options: UploadParameters[1];
  conversationId?: string;
};

export function useUploadAndParseFile() {
  const { conversationId: id } = useGetChatSearchParams();
  const { t } = useTranslation();
  const controller = useRef(new AbortController());

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.UploadAndParse],
    mutationFn: async ({
      file,
      options: { onProgress, onSuccess, onError },
      conversationId,
    }: X) => {
      try {
        const formData = new FormData();
        formData.append('file', file);
        formData.append('conversation_id', conversationId || id);

        const { data } = await chatService.uploadAndParse(
          {
            signal: controller.current.signal,
            data: formData,
            onUploadProgress: ({ progress }) => {
              onProgress(file, (progress || 0) * 100 - 1);
            },
          },
          true,
        );

        onProgress(file, 100);

        if (data.code === 0) {
          onSuccess(file);
          message.success(t(`message.uploaded`));
        } else {
          onError(file, new Error(data.message));
        }

        return data;
      } catch (error) {
        onError(file, error as Error);
      }
    },
  });

  const cancel = useCallback(() => {
    controller.current.abort();
    controller.current = new AbortController();
  }, [controller]);

  return { data, loading, uploadAndParseFile: mutateAsync, cancel };
}

export const useFetchExternalChatInfo = () => {
  const { sharedId: id } = useGetSharedChatSearchParams();

  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IExternalChatInfo>({
    queryKey: [ChatApiAction.FetchExternalChatInfo, id],
    gcTime: 0,
    initialData: {} as IExternalChatInfo,
    enabled: !!id,
    refetchOnWindowFocus: false,
    queryFn: async () => {
      const { data } = await chatService.fetchExternalChatInfo(id!);

      return data?.data;
    },
  });

  return { data, loading, refetch };
};

//#endregion

//#region search page

export const useFetchMindMap = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.FetchMindMap],
    gcTime: 0,
    mutationFn: async (params: IAskRequestBody) => {
      try {
        const ret = await chatService.getMindMap(params);
        return ret?.data?.data ?? {};
      } catch (error: any) {
        if (has(error, 'message')) {
          message.error(error.message);
        }

        return [];
      }
    },
  });

  return { data, loading, fetchMindMap: mutateAsync };
};

export const useFetchRelatedQuestions = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [ChatApiAction.FetchRelatedQuestions],
    gcTime: 0,
    mutationFn: async (question: string): Promise<string[]> => {
      const { data } = await chatService.getRelatedQuestions({ question });

      return data?.data ?? [];
    },
  });

  return { data, loading, fetchRelatedQuestions: mutateAsync };
};
//#endregion

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-chat-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 533 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (17)

- `enum`: Exported entity
- `useGetChatSearchParams`: Exported entity
- `useClickDialogCard`: Exported entity
- `useFetchDialogList`: Exported entity
- `useRemoveDialog`: Exported entity
- `useSetDialog`: Exported entity
- `useFetchDialog`: Exported entity
- `useClickConversationCard`: Exported entity
- `useFetchConversationList`: Exported entity
- `useFetchConversation`: Exported entity
- `useUpdateConversation`: Exported entity
- `useRemoveConversation`: Exported entity
- `useDeleteMessage`: Exported entity
- `useUploadAndParseFile`: Exported entity
- `useFetchExternalChatInfo`: Exported entity
- `useFetchMindMap`: Exported entity
- `useFetchRelatedQuestions`: Exported entity

### Functions (19)

- `useGetChatSearchParams()`: Function definition
- `useClickDialogCard()`: Function definition
- `handleClickDialog()`: Function definition
- `useFetchDialogList()`: Function definition
- `useRemoveDialog()`: Function definition
- `useSetDialog()`: Function definition
- `useFetchDialog()`: Function definition
- `useClickConversationCard()`: Function definition
- `handleClickConversation()`: Function definition
- `useFetchConversationList()`: Function definition
- `useFetchConversation()`: Function definition
- `useUpdateConversation()`: Function definition
- `useRemoveConversation()`: Function definition
- `useDeleteMessage()`: Function definition
- `useUploadAndParseFile()`: Function definition
- `cancel()`: Function definition
- `useFetchExternalChatInfo()`: Function definition
- `useFetchMindMap()`: Function definition
- `useFetchRelatedQuestions()`: Function definition

### Imports (18)

- `import { FileUploadProps } from '@/components/file-upload';`
- `import message from '@/components/ui/message';`
- `import { ChatSearchParams } from '@/constants/chat';`
- `import {`
- `import { IAskRequestBody } from '@/interfaces/request/chat';`
- `import { IClientConversation } from '@/pages/next-chats/chat/interface';`
- `import { useGetSharedChatSearchParams } from '@/pages/next-chats/hooks/use-send-shared-message';`
- `import { isConversationIdExist } from '@/pages/next-chats/utils';`
- `import chatService from '@/services/next-chat-service';`
- `import { buildMessageListWithUuid, getConversationId } from '@/utils/chat';`

## Code Structure Analysis

- Total lines: 533
- Blank lines: 71 (13.3%)
- Comment lines: ~10 (1.9%)
- Code lines: ~452


## Dependencies and Imports

- `@/components/file-upload`
- `@/components/ui/message`
- `@/constants/chat`
- `@/interfaces/request/chat`
- `@/pages/next-chats/chat/interface`
- `@/pages/next-chats/hooks/use-send-shared-message`
- `@/pages/next-chats/utils`
- `@/services/next-chat-service`
- `@/utils/chat`
- `@tanstack/react-query`
- `ahooks`
- `lodash`
- `react`
- `react-i18next`
- `umi`
- `./logic-hooks/use-change-search`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

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

- Other files in `web/src/hooks/` directory
- Potential test file: `test_use-chat-request.ts`

## Keywords

./logic-hooks/use-change-search, @/components/file-upload, @/components/ui/message, @/constants/chat, @/interfaces/request/chat, @/pages/next-chats/chat/interface, @/pages/next-chats/hooks/use-send-shared-message, @/pages/next-chats/utils, @/services/next-chat-service, @/utils/chat, @tanstack/react-query, AbortController, ChangeEventHandler, ChatApiAction, ChatSearchParams, Conversation, ConversationId, DeleteMessage, DialogId, EmptyConversationId, Error, FetchConversation, FetchConversationList, FetchDialog, FetchDialogList, FetchExternalChatInfo, FetchMindMap, FetchRelatedQuestions, FileUploadProps, FormData, HTMLInputElement, IAskRequestBody, IClientConversation, IConversation, IDialog, IExternalChatInfo, NonNullable, Parameters, Partial, Promise, React, Record, RemoveConversation, RemoveDialog, SetDialog, TypeScript, URLSearchParams, UpdateConversation, UploadAndParse, UploadParameters...

---
*Generated by RAGFlow Repository Documentation Generator*
