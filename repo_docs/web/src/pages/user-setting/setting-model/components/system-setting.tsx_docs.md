# File Documentation: web/src/pages/user-setting/setting-model/components/system-setting.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/components/system-setting.tsx`
- **Extension**: `.tsx`
- **Lines**: 197
- **Characters**: 5,626
- **Size**: 5,626 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  SelectWithSearch,
  SelectWithSearchFlagOptionType,
} from '@/components/originui/select-with-search';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { LlmModelType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import {
  ISystemModelSettingSavingParams,
  useComposeLlmOptionsByModelTypes,
} from '@/hooks/llm-hooks';
import { CircleQuestionMark } from 'lucide-react';
import { useCallback, useEffect, useMemo, useState } from 'react';
import { useFetchSystemModelSettingOnMount } from '../hooks';

interface IProps {
  loading: boolean;
  onOk: (
    payload: Omit<ISystemModelSettingSavingParams, 'tenant_id' | 'name'>,
  ) => void;
}

const SystemSetting = ({ onOk, loading }: IProps) => {
  const { systemSetting: initialValues, allOptions } =
    useFetchSystemModelSettingOnMount();
  const { t: tcommon } = useTranslate('common');
  const { t } = useTranslate('setting');

  const [formData, setFormData] = useState({
    llm_id: '',
    embd_id: '',
    img2txt_id: '',
    asr_id: '',
    rerank_id: '',
    tts_id: '',
  });

  const handleFieldChange = useCallback(
    (field: string, value: string) => {
      const updatedData = { ...formData, [field]: value || '' };
      setFormData(updatedData);
      console.log('updatedData', updatedData);
      onOk(updatedData);
    },
    [formData, onOk],
  );

  useEffect(() => {
    setFormData({
      llm_id: initialValues.llm_id ?? '',
      embd_id: initialValues.embd_id ?? '',
      img2txt_id: initialValues.img2txt_id ?? '',
      asr_id: initialValues.asr_id ?? '',
      rerank_id: initialValues.rerank_id ?? '',
      tts_id: initialValues.tts_id ?? '',
    });
  }, [initialValues]);

  const modelOptions = useComposeLlmOptionsByModelTypes([
    LlmModelType.Chat,
    LlmModelType.Image2text,
  ]);

  const llmList = useMemo(() => {
    return [
      {
        id: 'llm_id',
        label: t('chatModel'),
        isRequired: true,
        value: formData.llm_id,
        options: modelOptions as SelectWithSearchFlagOptionType[],
        tooltip: t('chatModelTip'),
      },
      {
        id: 'embd_id',
        label: t('embeddingModel'),
        value: formData.embd_id,
        options: allOptions[
          LlmModelType.Embedding
        ] as SelectWithSearchFlagOptionType[],
        tooltip: t('embeddingModelTip'),
      },
      {
        id: 'img2txt_id',
        label: t('img2txtModel'),
        value: formData.img2txt_id,
        options: allOptions[
          LlmModelType.Image2text
        ] as SelectWithSearchFlagOptionType[],
        tooltip: t('img2txtModelTip'),
      },
      {
        id: 'asr_id',
        label: t('sequence2txtModel'),
        value: formData.asr_id,
        options: allOptions[
          LlmModelType.Speech2text
        ] as SelectWithSearchFlagOptionType[],
        tooltip: t('sequence2txtModelTip'),
      },
      {
        id: 'rerank_id',
        label: t('rerankModel'),
        value: formData.rerank_id,
        options: allOptions[
          LlmModelType.Rerank
        ] as SelectWithSearchFlagOptionType[],
        tooltip: t('rerankModelTip'),
      },
      {
        id: 'tts_id',
        label: t('ttsModel'),
        value: formData.tts_id,
        options: allOptions[
          LlmModelType.TTS
        ] as SelectWithSearchFlagOptionType[],
        tooltip: t('ttsModelTip'),
      },
    ];
  }, [formData, modelOptions, t, allOptions]);

  const Items = ({
    label,
    value,
    options,
    tooltip,
    id,
    isRequired,
  }: {
    id: string;
    label: string;
    value: string;
    options: SelectWithSearchFlagOptionType[];
    tooltip?: string;
    isRequired?: boolean;
  }) => {
    return (
      <div className="flex gap-3">
        <label className="block text-sm font-medium text-text-secondary mb-1 w-1/4">
          {isRequired && <span className="text-red-500">*</span>}
          {label}
          {tooltip && (
            <Tooltip>
              <TooltipContent>{tooltip}</TooltipContent>
              <TooltipTrigger>
                <CircleQuestionMark
                  size={12}
                  className="ml-1 text-text-disabled text-xs"
                />
              </TooltipTrigger>
            </Tooltip>
          )}
        </label>
        <SelectWithSearch
          triggerClassName="w-3/4 flex items-center"
          allowClear={id !== 'llm_id'}
          value={value}
          options={options}
          onChange={(value) => handleFieldChange(id, value)}
          placeholder={t('selectModelPlaceholder')}
        />
      </div>
    );
  };

  return (
    <div className="rounded-lg w-full">
      <div className="flex flex-col py-4">
        <div className="text-2xl font-medium">{t('systemModelSettings')}</div>
        <div className="text-sm text-text-secondary">
          {t('systemModelDescription')}
        </div>
      </div>
      <div className="px-7 py-6 space-y-6 max-h-[70vh] overflow-y-auto border border-border-button rounded-lg">
        {llmList.map((item) => (
          <Items key={item.id} {...item} />
        ))}
      </div>
      {/* <div className="border-t px-6 py-4 flex justify-end">
          <Button
            onClick={hideModal}
            disabled={loading}
            className="px-4 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:opacity-50"
          >
            {t('common:cancel')}
          </Button>
        </div> */}
    </div>
  );
};

export default SystemSetting;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/components/system-setting.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 197 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `SystemSetting()`: Function definition
- `handleFieldChange()`: Function definition
- `updatedData()`: Function definition
- `llmList()`: Function definition
- `Items()`: Function definition

### Imports (8)

- `import {`
- `import {`
- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import { CircleQuestionMark } from 'lucide-react';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`
- `import { useFetchSystemModelSettingOnMount } from '../hooks';`

## Code Structure Analysis

- Total lines: 197
- Blank lines: 11 (5.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~186


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `lucide-react`
- `react`
- `../hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/components`.

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

- Other files in `web/src/pages/user-setting/setting-model/components/` directory
- Potential test file: `test_system-setting.tsx`

## Keywords

../hooks, @/constants/knowledge, @/hooks/common-hooks, Button, Chat, CircleQuestionMark, Embedding, IProps, ISystemModelSettingSavingParams, Image2text, Items, LlmModelType, Omit, Rerank, SelectWithSearch, SelectWithSearchFlagOptionType, Speech2text, SystemSetting, TTS, Tooltip, TooltipContent, TooltipTrigger, TypeScript, handleFieldChange, llmList, lucide-react, modelOptions, react, updatedData

---
*Generated by RAGFlow Repository Documentation Generator*
