# File Documentation: web/src/hooks/llm-hooks.tsx

## File Metadata

- **Path**: `web/src/hooks/llm-hooks.tsx`
- **Extension**: `.tsx`
- **Lines**: 414
- **Characters**: 11,813
- **Size**: 11,813 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { LlmIcon } from '@/components/svg-icon';
import message from '@/components/ui/message';
import { LlmModelType } from '@/constants/knowledge';
import { ResponseGetType } from '@/interfaces/database/base';
import {
  IFactory,
  IMyLlmValue,
  IThirdOAIModelCollection as IThirdAiModelCollection,
  IThirdOAIModel,
  IThirdOAIModelCollection,
} from '@/interfaces/database/llm';
import {
  IAddLlmRequestBody,
  IDeleteLlmRequestBody,
} from '@/interfaces/request/llm';
import userService from '@/services/user-service';
import { getLLMIconName, getRealModelName } from '@/utils/llm-util';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { DefaultOptionType } from 'antd/es/select';
import { useCallback, useMemo } from 'react';
import { useTranslation } from 'react-i18next';

export const useFetchLlmList = (
  modelType?: LlmModelType,
): IThirdAiModelCollection => {
  const { data } = useQuery({
    queryKey: ['llmList'],
    initialData: {},
    queryFn: async () => {
      const { data } = await userService.llm_list({ model_type: modelType });

      return data?.data ?? {};
    },
  });

  return data;
};

export const useSelectLlmOptions = () => {
  const llmInfo: IThirdOAIModelCollection = useFetchLlmList();

  const embeddingModelOptions = useMemo(() => {
    return Object.entries(llmInfo).map(([key, value]) => {
      return {
        label: key,
        options: value.map((x) => ({
          label: getRealModelName(x.llm_name),
          value: `${x.llm_name}@${x.fid}`,
          disabled: !x.available,
        })),
      };
    });
  }, [llmInfo]);

  return embeddingModelOptions;
};

function buildLlmOptionsWithIcon(x: IThirdOAIModel) {
  return {
    label: (
      <div className="flex items-center justify-center gap-6">
        <LlmIcon
          name={getLLMIconName(x.fid, x.llm_name)}
          width={26}
          height={26}
          size={'small'}
        />
        <span>{getRealModelName(x.llm_name)}</span>
      </div>
    ),
    value: `${x.llm_name}@${x.fid}`,
    disabled: !x.available,
    is_tools: x.is_tools,
  };
}

export const useSelectLlmOptionsByModelType = () => {
  const llmInfo: IThirdOAIModelCollection = useFetchLlmList();

  const groupImage2TextOptions = useCallback(() => {
    const modelType = LlmModelType.Image2text;
    const modelTag = modelType.toUpperCase();
    return Object.entries(llmInfo)
      .map(([key, value]) => {
        return {
          label: key,
          options: value
            .filter(
              (x) =>
                (x.model_type.includes(modelType) ||
                  (x.tags && x.tags.includes(modelTag))) &&
                x.available &&
                x.status === '1',
            )
            .map(buildLlmOptionsWithIcon),
        };
      })
      .filter((x) => x.options.length > 0);
  }, [llmInfo]);

  const groupOptionsByModelType = useCallback(
    (modelType: LlmModelType) => {
      return Object.entries(llmInfo)
        .filter(([, value]) =>
          modelType
            ? value.some((x) => x.model_type.includes(modelType))
            : true,
        )
        .map(([key, value]) => {
          return {
            label: key,
            options: value
              .filter(
                (x) =>
                  (modelType ? x.model_type.includes(modelType) : true) &&
                  x.available,
              )
              .map(buildLlmOptionsWithIcon),
          };
        })
        .filter((x) => x.options.length > 0);
    },
    [llmInfo],
  );

  return {
    [LlmModelType.Chat]: groupOptionsByModelType(LlmModelType.Chat),
    [LlmModelType.Embedding]: groupOptionsByModelType(LlmModelType.Embedding),
    [LlmModelType.Image2text]: groupImage2TextOptions(),
    [LlmModelType.Speech2text]: groupOptionsByModelType(
      LlmModelType.Speech2text,
    ),
    [LlmModelType.Rerank]: groupOptionsByModelType(LlmModelType.Rerank),
    [LlmModelType.TTS]: groupOptionsByModelType(LlmModelType.TTS),
  };
};

// Merge different types of models from the same manufacturer under one manufacturer
export const useComposeLlmOptionsByModelTypes = (
  modelTypes: LlmModelType[],
) => {
  const allOptions = useSelectLlmOptionsByModelType();
  return modelTypes.reduce<
    (DefaultOptionType & {
      options: {
        label: JSX.Element;
        value: string;
        disabled: boolean;
        is_tools: boolean;
      }[];
    })[]
  >((pre, cur) => {
    const options = allOptions[cur];
    options.forEach((x) => {
      const item = pre.find((y) => y.label === x.label);
      if (item) {
        x.options.forEach((y) => {
          // A model that is both an image2text and speech2text model
          if (!item.options.some((z) => z.value === y.value)) {
            item.options.push(y);
          }
        });
      } else {
        pre.push(x);
      }
    });

    return pre;
  }, []);
};

export const useFetchLlmFactoryList = (): ResponseGetType<IFactory[]> => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['factoryList'],
    initialData: [],
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.factories_list();

      return data?.data ?? [];
    },
  });

  return { data, loading };
};

export type LlmItem = { name: string; logo: string } & IMyLlmValue;

export const useFetchMyLlmList = (): ResponseGetType<
  Record<string, IMyLlmValue>
> => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['myLlmList'],
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.my_llm();

      return data?.data ?? {};
    },
  });

  return { data, loading };
};

export const useFetchMyLlmListDetailed = (): ResponseGetType<
  Record<string, any>
> => {
  const { data, isFetching: loading } = useQuery({
    queryKey: ['myLlmListDetailed'],
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.my_llm({ include_details: true });

      return data?.data ?? {};
    },
  });

  return { data, loading };
};

export const useSelectLlmList = () => {
  const { data: myLlmList, loading: myLlmListLoading } = useFetchMyLlmList();
  const { data: factoryList, loading: factoryListLoading } =
    useFetchLlmFactoryList();

  const nextMyLlmList: Array<LlmItem> = useMemo(() => {
    return Object.entries(myLlmList).map(([key, value]) => ({
      name: key,
      logo: factoryList.find((x) => x.name === key)?.logo ?? '',
      ...value,
      llm: value.llm.map((x) => ({ ...x, name: x.name })),
    }));
  }, [myLlmList, factoryList]);

  const nextFactoryList = useMemo(() => {
    const currentList = factoryList.filter((x) =>
      Object.keys(myLlmList).every((y) => y !== x.name),
    );
    return currentList;
    // return sortLLmFactoryListBySpecifiedOrder(currentList);
  }, [factoryList, myLlmList]);

  return {
    myLlmList: nextMyLlmList,
    factoryList: nextFactoryList,
    loading: myLlmListLoading || factoryListLoading,
  };
};

export interface IApiKeySavingParams {
  llm_factory: string;
  api_key: string;
  llm_name?: string;
  model_type?: string;
  base_url?: string;
}

export const useSaveApiKey = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['saveApiKey'],
    mutationFn: async (params: IApiKeySavingParams) => {
      const { data } = await userService.set_api_key(params);
      if (data.code === 0) {
        message.success(t('message.modified'));
        queryClient.invalidateQueries({ queryKey: ['myLlmList'] });
        queryClient.invalidateQueries({ queryKey: ['myLlmListDetailed'] });
        queryClient.invalidateQueries({ queryKey: ['factoryList'] });
      }
      return data.code;
    },
  });

  return { data, loading, saveApiKey: mutateAsync };
};

export interface ISystemModelSettingSavingParams {
  tenant_id: string;
  name?: string;
  asr_id: string;
  embd_id: string;
  img2txt_id: string;
  llm_id: string;
}

export const useSaveTenantInfo = () => {
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['saveTenantInfo'],
    mutationFn: async (params: ISystemModelSettingSavingParams) => {
      const { data } = await userService.set_tenant_info(params);
      if (data.code === 0) {
        message.success(t('message.modified'));
      }
      return data.code;
    },
  });

  return { data, loading, saveTenantInfo: mutateAsync };
};

export const useAddLlm = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['addLlm'],
    mutationFn: async (params: IAddLlmRequestBody) => {
      const { data } = await userService.add_llm(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['myLlmList'] });
        queryClient.invalidateQueries({ queryKey: ['myLlmListDetailed'] });
        queryClient.invalidateQueries({ queryKey: ['factoryList'] });
        message.success(t('message.modified'));
      }
      return data.code;
    },
  });

  return { data, loading, addLlm: mutateAsync };
};

export const useDeleteLlm = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteLlm'],
    mutationFn: async (params: IDeleteLlmRequestBody) => {
      const { data } = await userService.delete_llm(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['myLlmList'] });
        queryClient.invalidateQueries({ queryKey: ['myLlmListDetailed'] });
        queryClient.invalidateQueries({ queryKey: ['factoryList'] });
        message.success(t('message.deleted'));
      }
      return data.code;
    },
  });

  return { data, loading, deleteLlm: mutateAsync };
};

export const useEnableLlm = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['enableLlm'],
    mutationFn: async (params: IDeleteLlmRequestBody & { enable: boolean }) => {
      const reqParam: IDeleteLlmRequestBody & {
        enable?: boolean;
        status?: 1 | 0;
      } = { ...params, status: params.enable ? 1 : 0 };
      delete reqParam.enable;
      const { data } = await userService.enable_llm(reqParam);
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['myLlmList'] });
        queryClient.invalidateQueries({ queryKey: ['myLlmListDetailed'] });
        queryClient.invalidateQueries({ queryKey: ['factoryList'] });
        message.success(t('message.modified'));
      }
      return data.code;
    },
  });

  return { data, loading, enableLlm: mutateAsync };
};

export const useDeleteFactory = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteFactory'],
    mutationFn: async (params: IDeleteLlmRequestBody) => {
      const { data } = await userService.deleteFactory(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['myLlmList'] });
        queryClient.invalidateQueries({ queryKey: ['myLlmListDetailed'] });
        queryClient.invalidateQueries({ queryKey: ['factoryList'] });
        queryClient.invalidateQueries({ queryKey: ['llmList'] });
        message.success(t('message.deleted'));
      }
      return data.code;
    },
  });

  return { data, loading, deleteFactory: mutateAsync };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/llm-hooks.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 414 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (14)

- `useFetchLlmList`: Exported entity
- `useSelectLlmOptions`: Exported entity
- `useSelectLlmOptionsByModelType`: Exported entity
- `useComposeLlmOptionsByModelTypes`: Exported entity
- `useFetchLlmFactoryList`: Exported entity
- `useFetchMyLlmList`: Exported entity
- `useFetchMyLlmListDetailed`: Exported entity
- `useSelectLlmList`: Exported entity
- `useSaveApiKey`: Exported entity
- `useSaveTenantInfo`: Exported entity
- `useAddLlm`: Exported entity
- `useDeleteLlm`: Exported entity
- `useEnableLlm`: Exported entity
- `useDeleteFactory`: Exported entity

### Functions (24)

- `useFetchLlmList()`: Function definition
- `useSelectLlmOptions()`: Function definition
- `embeddingModelOptions()`: Function definition
- `buildLlmOptionsWithIcon()`: Function definition
- `useSelectLlmOptionsByModelType()`: Function definition
- `groupImage2TextOptions()`: Function definition
- `modelTag()`: Function definition
- `groupOptionsByModelType()`: Function definition
- `useComposeLlmOptionsByModelTypes()`: Function definition
- `allOptions()`: Function definition
- `options()`: Function definition
- `item()`: Function definition
- `useFetchLlmFactoryList()`: Function definition
- `useFetchMyLlmList()`: Function definition
- `useFetchMyLlmListDetailed()`: Function definition
- `useSelectLlmList()`: Function definition
- `nextFactoryList()`: Function definition
- `currentList()`: Function definition
- `useSaveApiKey()`: Function definition
- `useSaveTenantInfo()`: Function definition

### Imports (12)

- `import { LlmIcon } from '@/components/svg-icon';`
- `import message from '@/components/ui/message';`
- `import { LlmModelType } from '@/constants/knowledge';`
- `import { ResponseGetType } from '@/interfaces/database/base';`
- `import {`
- `import {`
- `import userService from '@/services/user-service';`
- `import { getLLMIconName, getRealModelName } from '@/utils/llm-util';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { DefaultOptionType } from 'antd/es/select';`

## Code Structure Analysis

- Total lines: 414
- Blank lines: 42 (10.1%)
- Comment lines: ~3 (0.7%)
- Code lines: ~369


## Dependencies and Imports

- `@/components/svg-icon`
- `@/components/ui/message`
- `@/constants/knowledge`
- `@/interfaces/database/base`
- `@/services/user-service`
- `@/utils/llm-util`
- `@tanstack/react-query`
- `antd/es/select`
- `react`
- `react-i18next`

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
- Potential test file: `test_llm-hooks.tsx`

## Keywords

@/components/svg-icon, @/components/ui/message, @/constants/knowledge, @/interfaces/database/base, @/services/user-service, @/utils/llm-util, @tanstack/react-query, Array, Chat, DefaultOptionType, Element, Embedding, IAddLlmRequestBody, IApiKeySavingParams, IDeleteLlmRequestBody, IFactory, IMyLlmValue, ISystemModelSettingSavingParams, IThirdAiModelCollection, IThirdOAIModel, IThirdOAIModelCollection, Image2text, JSX, LlmIcon, LlmItem, LlmModelType, Merge, Object, Record, Rerank, ResponseGetType, Speech2text, TTS, TypeScript, allOptions, antd/es/select, buildLlmOptionsWithIcon, currentList, embeddingModelOptions, groupImage2TextOptions, groupOptionsByModelType, item, llmInfo, modelTag, modelType, nextFactoryList, nextMyLlmList, options, queryClient, react...

---
*Generated by RAGFlow Repository Documentation Generator*
