# File Documentation: web/src/pages/user-setting/setting-model/hooks.ts

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 462
- **Characters**: 11,640
- **Size**: 11,640 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';
import {
  IApiKeySavingParams,
  ISystemModelSettingSavingParams,
  useAddLlm,
  useDeleteFactory,
  useDeleteLlm,
  useEnableLlm,
  useSaveApiKey,
  useSaveTenantInfo,
  useSelectLlmOptionsByModelType,
} from '@/hooks/llm-hooks';
import { useFetchTenantInfo } from '@/hooks/user-setting-hooks';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { getRealModelName } from '@/utils/llm-util';
import { useQueryClient } from '@tanstack/react-query';
import { useCallback, useState } from 'react';
import { ApiKeyPostBody } from '../interface';

type SavingParamsState = Omit<IApiKeySavingParams, 'api_key'>;

export const useSubmitApiKey = () => {
  const [savingParams, setSavingParams] = useState<SavingParamsState>(
    {} as SavingParamsState,
  );
  const [editMode, setEditMode] = useState(false);
  const { saveApiKey, loading } = useSaveApiKey();
  const {
    visible: apiKeyVisible,
    hideModal: hideApiKeyModal,
    showModal: showApiKeyModal,
  } = useSetModalState();
  const queryClient = useQueryClient();
  const onApiKeySavingOk = useCallback(
    async (postBody: ApiKeyPostBody) => {
      const ret = await saveApiKey({
        ...savingParams,
        ...postBody,
      });

      if (ret === 0) {
        queryClient.invalidateQueries({ queryKey: ['llmList'] });
        hideApiKeyModal();
        setEditMode(false);
      }
    },
    [hideApiKeyModal, saveApiKey, savingParams, queryClient],
  );

  const onShowApiKeyModal = useCallback(
    (savingParams: SavingParamsState, isEdit = false) => {
      setSavingParams(savingParams);
      setEditMode(isEdit);
      showApiKeyModal();
    },
    [showApiKeyModal, setSavingParams],
  );

  return {
    saveApiKeyLoading: loading,
    initialApiKey: '',
    llmFactory: savingParams.llm_factory,
    editMode,
    onApiKeySavingOk,
    apiKeyVisible,
    hideApiKeyModal,
    showApiKeyModal: onShowApiKeyModal,
  };
};

export const useSubmitSystemModelSetting = () => {
  const { data: systemSetting } = useFetchTenantInfo();
  const { saveTenantInfo: saveSystemModelSetting, loading } =
    useSaveTenantInfo();
  const {
    visible: systemSettingVisible,
    hideModal: hideSystemSettingModal,
    showModal: showSystemSettingModal,
  } = useSetModalState();

  const onSystemSettingSavingOk = useCallback(
    async (
      payload: Omit<ISystemModelSettingSavingParams, 'tenant_id' | 'name'>,
    ) => {
      const ret = await saveSystemModelSetting({
        tenant_id: systemSetting.tenant_id,
        name: systemSetting.name,
        ...payload,
      });

      if (ret === 0) {
        hideSystemSettingModal();
      }
    },
    [hideSystemSettingModal, saveSystemModelSetting, systemSetting],
  );

  return {
    saveSystemModelSettingLoading: loading,
    onSystemSettingSavingOk,
    systemSettingVisible,
    hideSystemSettingModal,
    showSystemSettingModal,
  };
};

export const useFetchSystemModelSettingOnMount = () => {
  const { data: systemSetting } = useFetchTenantInfo();
  const allOptions = useSelectLlmOptionsByModelType();

  return { systemSetting, allOptions };
};

export const useSubmitOllama = () => {
  const [selectedLlmFactory, setSelectedLlmFactory] = useState<string>('');
  const [editMode, setEditMode] = useState(false);
  const [initialValues, setInitialValues] = useState<
    Partial<IAddLlmRequestBody> | undefined
  >();
  const { addLlm, loading } = useAddLlm();
  const {
    visible: llmAddingVisible,
    hideModal: hideLlmAddingModal,
    showModal: showLlmAddingModal,
  } = useSetModalState();

  const onLlmAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const cleanedPayload = { ...payload };
      if (!cleanedPayload.api_key || cleanedPayload.api_key.trim() === '') {
        delete cleanedPayload.api_key;
      }

      const ret = await addLlm(cleanedPayload);
      if (ret === 0) {
        hideLlmAddingModal();
        setEditMode(false);
        setInitialValues(undefined);
      }
    },
    [hideLlmAddingModal, addLlm],
  );

  const handleShowLlmAddingModal = (
    llmFactory: string,
    isEdit = false,
    modelData?: any,
    detailedData?: any,
  ) => {
    setSelectedLlmFactory(llmFactory);
    setEditMode(isEdit);

    if (isEdit && detailedData) {
      const initialVals = {
        llm_name: getRealModelName(detailedData.name),
        model_type: detailedData.type,
        api_base: detailedData.api_base || '',
        max_tokens: detailedData.max_tokens || 8192,
        api_key: '',
      };
      setInitialValues(initialVals);
    } else {
      setInitialValues(undefined);
    }
    showLlmAddingModal();
  };

  return {
    llmAddingLoading: loading,
    editMode,
    initialValues,
    onLlmAddingOk,
    llmAddingVisible,
    hideLlmAddingModal,
    showLlmAddingModal: handleShowLlmAddingModal,
    selectedLlmFactory,
  };
};

export const useSubmitVolcEngine = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: volcAddingVisible,
    hideModal: hideVolcAddingModal,
    showModal: showVolcAddingModal,
  } = useSetModalState();

  const onVolcAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideVolcAddingModal();
      }
    },
    [hideVolcAddingModal, addLlm],
  );

  return {
    volcAddingLoading: loading,
    onVolcAddingOk,
    volcAddingVisible,
    hideVolcAddingModal,
    showVolcAddingModal,
  };
};

export const useSubmitHunyuan = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: HunyuanAddingVisible,
    hideModal: hideHunyuanAddingModal,
    showModal: showHunyuanAddingModal,
  } = useSetModalState();

  const onHunyuanAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideHunyuanAddingModal();
      }
    },
    [hideHunyuanAddingModal, addLlm],
  );

  return {
    HunyuanAddingLoading: loading,
    onHunyuanAddingOk,
    HunyuanAddingVisible,
    hideHunyuanAddingModal,
    showHunyuanAddingModal,
  };
};

export const useSubmitTencentCloud = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: TencentCloudAddingVisible,
    hideModal: hideTencentCloudAddingModal,
    showModal: showTencentCloudAddingModal,
  } = useSetModalState();

  const onTencentCloudAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideTencentCloudAddingModal();
      }
    },
    [hideTencentCloudAddingModal, addLlm],
  );

  return {
    TencentCloudAddingLoading: loading,
    onTencentCloudAddingOk,
    TencentCloudAddingVisible,
    hideTencentCloudAddingModal,
    showTencentCloudAddingModal,
  };
};

export const useSubmitSpark = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: SparkAddingVisible,
    hideModal: hideSparkAddingModal,
    showModal: showSparkAddingModal,
  } = useSetModalState();

  const onSparkAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideSparkAddingModal();
      }
    },
    [hideSparkAddingModal, addLlm],
  );

  return {
    SparkAddingLoading: loading,
    onSparkAddingOk,
    SparkAddingVisible,
    hideSparkAddingModal,
    showSparkAddingModal,
  };
};

export const useSubmityiyan = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: yiyanAddingVisible,
    hideModal: hideyiyanAddingModal,
    showModal: showyiyanAddingModal,
  } = useSetModalState();

  const onyiyanAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideyiyanAddingModal();
      }
    },
    [hideyiyanAddingModal, addLlm],
  );

  return {
    yiyanAddingLoading: loading,
    onyiyanAddingOk,
    yiyanAddingVisible,
    hideyiyanAddingModal,
    showyiyanAddingModal,
  };
};

export const useSubmitFishAudio = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: FishAudioAddingVisible,
    hideModal: hideFishAudioAddingModal,
    showModal: showFishAudioAddingModal,
  } = useSetModalState();

  const onFishAudioAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideFishAudioAddingModal();
      }
    },
    [hideFishAudioAddingModal, addLlm],
  );

  return {
    FishAudioAddingLoading: loading,
    onFishAudioAddingOk,
    FishAudioAddingVisible,
    hideFishAudioAddingModal,
    showFishAudioAddingModal,
  };
};

export const useSubmitGoogle = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: GoogleAddingVisible,
    hideModal: hideGoogleAddingModal,
    showModal: showGoogleAddingModal,
  } = useSetModalState();

  const onGoogleAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideGoogleAddingModal();
      }
    },
    [hideGoogleAddingModal, addLlm],
  );

  return {
    GoogleAddingLoading: loading,
    onGoogleAddingOk,
    GoogleAddingVisible,
    hideGoogleAddingModal,
    showGoogleAddingModal,
  };
};

export const useSubmitBedrock = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: bedrockAddingVisible,
    hideModal: hideBedrockAddingModal,
    showModal: showBedrockAddingModal,
  } = useSetModalState();

  const onBedrockAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideBedrockAddingModal();
      }
    },
    [hideBedrockAddingModal, addLlm],
  );

  return {
    bedrockAddingLoading: loading,
    onBedrockAddingOk,
    bedrockAddingVisible,
    hideBedrockAddingModal,
    showBedrockAddingModal,
  };
};

export const useSubmitAzure = () => {
  const { addLlm, loading } = useAddLlm();
  const {
    visible: AzureAddingVisible,
    hideModal: hideAzureAddingModal,
    showModal: showAzureAddingModal,
  } = useSetModalState();

  const onAzureAddingOk = useCallback(
    async (payload: IAddLlmRequestBody) => {
      const ret = await addLlm(payload);
      if (ret === 0) {
        hideAzureAddingModal();
      }
    },
    [hideAzureAddingModal, addLlm],
  );

  return {
    AzureAddingLoading: loading,
    onAzureAddingOk,
    AzureAddingVisible,
    hideAzureAddingModal,
    showAzureAddingModal,
  };
};

export const useHandleDeleteLlm = (llmFactory: string) => {
  const { deleteLlm } = useDeleteLlm();
  const showDeleteConfirm = useShowDeleteConfirm();

  const handleDeleteLlm = (name: string) => {
    showDeleteConfirm({
      onOk: async () => {
        deleteLlm({ llm_factory: llmFactory, llm_name: name });
      },
    });
  };

  return { handleDeleteLlm };
};

export const useHandleEnableLlm = (llmFactory: string) => {
  const { enableLlm } = useEnableLlm();

  const handleEnableLlm = (name: string, enable: boolean) => {
    enableLlm({ llm_factory: llmFactory, llm_name: name, enable });
  };

  return { handleEnableLlm };
};

export const useHandleDeleteFactory = (llmFactory: string) => {
  const { deleteFactory } = useDeleteFactory();
  const showDeleteConfirm = useShowDeleteConfirm();

  const handleDeleteFactory = () => {
    showDeleteConfirm({
      onOk: async () => {
        deleteFactory({ llm_factory: llmFactory });
      },
    });
  };

  return { handleDeleteFactory };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 462 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (16)

- `useSubmitApiKey`: Exported entity
- `useSubmitSystemModelSetting`: Exported entity
- `useFetchSystemModelSettingOnMount`: Exported entity
- `useSubmitOllama`: Exported entity
- `useSubmitVolcEngine`: Exported entity
- `useSubmitHunyuan`: Exported entity
- `useSubmitTencentCloud`: Exported entity
- `useSubmitSpark`: Exported entity
- `useSubmityiyan`: Exported entity
- `useSubmitFishAudio`: Exported entity
- `useSubmitGoogle`: Exported entity
- `useSubmitBedrock`: Exported entity
- `useSubmitAzure`: Exported entity
- `useHandleDeleteLlm`: Exported entity
- `useHandleEnableLlm`: Exported entity
- `useHandleDeleteFactory`: Exported entity

### Functions (32)

- `useSubmitApiKey()`: Function definition
- `onApiKeySavingOk()`: Function definition
- `useSubmitSystemModelSetting()`: Function definition
- `onSystemSettingSavingOk()`: Function definition
- `useFetchSystemModelSettingOnMount()`: Function definition
- `useSubmitOllama()`: Function definition
- `onLlmAddingOk()`: Function definition
- `handleShowLlmAddingModal()`: Function definition
- `useSubmitVolcEngine()`: Function definition
- `onVolcAddingOk()`: Function definition
- `useSubmitHunyuan()`: Function definition
- `onHunyuanAddingOk()`: Function definition
- `useSubmitTencentCloud()`: Function definition
- `onTencentCloudAddingOk()`: Function definition
- `useSubmitSpark()`: Function definition
- `onSparkAddingOk()`: Function definition
- `useSubmityiyan()`: Function definition
- `onyiyanAddingOk()`: Function definition
- `useSubmitFishAudio()`: Function definition
- `onFishAudioAddingOk()`: Function definition

### Imports (8)

- `import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';`
- `import {`
- `import { useFetchTenantInfo } from '@/hooks/user-setting-hooks';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { getRealModelName } from '@/utils/llm-util';`
- `import { useQueryClient } from '@tanstack/react-query';`
- `import { useCallback, useState } from 'react';`
- `import { ApiKeyPostBody } from '../interface';`

## Code Structure Analysis

- Total lines: 462
- Blank lines: 54 (11.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~408


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/hooks/user-setting-hooks`
- `@/interfaces/request/llm`
- `@/utils/llm-util`
- `@tanstack/react-query`
- `react`
- `../interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model`.

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

- Other files in `web/src/pages/user-setting/setting-model/` directory
- Potential test file: `test_hooks.ts`

## Keywords

../interface, @/hooks/common-hooks, @/hooks/user-setting-hooks, @/interfaces/request/llm, @/utils/llm-util, @tanstack/react-query, ApiKeyPostBody, AzureAddingLoading, AzureAddingVisible, FishAudioAddingLoading, FishAudioAddingVisible, GoogleAddingLoading, GoogleAddingVisible, HunyuanAddingLoading, HunyuanAddingVisible, IAddLlmRequestBody, IApiKeySavingParams, ISystemModelSettingSavingParams, Omit, Partial, SavingParamsState, SparkAddingLoading, SparkAddingVisible, TencentCloudAddingLoading, TencentCloudAddingVisible, TypeScript, allOptions, cleanedPayload, handleDeleteFactory, handleDeleteLlm, handleEnableLlm, handleShowLlmAddingModal, initialVals, onApiKeySavingOk, onAzureAddingOk, onBedrockAddingOk, onFishAudioAddingOk, onGoogleAddingOk, onHunyuanAddingOk, onLlmAddingOk, onShowApiKeyModal, onSparkAddingOk, onSystemSettingSavingOk, onTencentCloudAddingOk, onVolcAddingOk, onyiyanAddingOk, queryClient, react, ret, showDeleteConfirm...

---
*Generated by RAGFlow Repository Documentation Generator*
