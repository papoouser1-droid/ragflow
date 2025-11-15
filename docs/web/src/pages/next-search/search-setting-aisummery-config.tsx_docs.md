# Documentation: web/src/pages/next-search/search-setting-aisummery-config.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/search-setting-aisummery-config.tsx`
- **Size**: 6941 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/next-search/search-setting-aisummery-config.tsx`.

## Original Source Code

```tsx
import { SliderInputSwitchFormField } from '@/components/llm-setting-items/slider';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import {
  LlmModelType,
  ModelVariableType,
  settledModelVariableMap,
} from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useComposeLlmOptionsByModelTypes } from '@/hooks/llm-hooks';
import { camelCase, isEqual } from 'lodash';
import { useCallback } from 'react';
import { useFormContext } from 'react-hook-form';
import { z } from 'zod';

interface LlmSettingFieldItemsProps {
  prefix?: string;
  options?: any[];
}
const LlmSettingEnableSchema = {
  temperatureEnabled: z.boolean(),
  topPEnabled: z.boolean(),
  presencePenaltyEnabled: z.boolean(),
  frequencyPenaltyEnabled: z.boolean(),
};
export const LlmSettingSchema = {
  llm_id: z.string(),
  parameter: z.string().optional(),
  temperature: z.coerce.number().optional(),
  top_p: z.coerce.number().optional(),
  presence_penalty: z.coerce.number().optional(),
  frequency_penalty: z.coerce.number().optional(),
  ...LlmSettingEnableSchema,
  // maxTokensEnabled: z.boolean(),
};

export function LlmSettingFieldItems({
  prefix,
  options,
}: LlmSettingFieldItemsProps) {
  const form = useFormContext();
  const { t } = useTranslate('chat');

  const modelOptions = useComposeLlmOptionsByModelTypes([
    LlmModelType.Chat,
    LlmModelType.Image2text,
  ]);

  const handleChange = useCallback(
    (parameter: string) => {
      const values =
        settledModelVariableMap[
          parameter as keyof typeof settledModelVariableMap
        ];
      const enabledKeys = Object.keys(LlmSettingEnableSchema);

      for (const key in values) {
        if (Object.prototype.hasOwnProperty.call(values, key)) {
          const element = values[key as keyof typeof values];
          form.setValue(`${prefix}.${key}`, element);
        }
      }
      if (enabledKeys && enabledKeys.length) {
        for (const key of enabledKeys) {
          form.setValue(`${prefix}.${key}`, true);
        }
      }
    },
    [form, prefix],
  );

  const parameterOptions = Object.values(ModelVariableType).map((x) => ({
    label: t(camelCase(x)),
    value: x,
  })) as unknown as { label: string; value: ModelVariableType | 'Custom' }[];
  parameterOptions.push({
    label: t(camelCase('Custom')),
    value: 'Custom',
  });

  const getFieldWithPrefix = useCallback(
    (name: string) => {
      return prefix ? `${prefix}.${name}` : name;
    },
    [prefix],
  );

  const checkParameterIsEquel = () => {
    const [
      parameter,
      topPValue,
      frequencyPenaltyValue,
      temperatureValue,
      presencePenaltyValue,
    ] = form.getValues([
      getFieldWithPrefix('parameter'),
      getFieldWithPrefix('temperature'),
      getFieldWithPrefix('top_p'),
      getFieldWithPrefix('frequency_penalty'),
      getFieldWithPrefix('presence_penalty'),
    ]);
    if (parameter && parameter !== 'Custom') {
      const parameterValue =
        settledModelVariableMap[parameter as keyof typeof ModelVariableType];
      const parameterRealValue = {
        top_p: topPValue,
        temperature: temperatureValue,
        frequency_penalty: frequencyPenaltyValue,
        presence_penalty: presencePenaltyValue,
      };
      if (!isEqual(parameterValue, parameterRealValue)) {
        form.setValue(getFieldWithPrefix('parameter'), 'Custom');
      }
    }
  };

  return (
    <div className="space-y-5">
      <FormField
        control={form.control}
        name={getFieldWithPrefix('llm_id')}
        render={({ field }) => (
          <FormItem>
            <FormLabel>
              <span className="text-destructive mr-1"> *</span>
              {t('model')}
            </FormLabel>
            <FormControl>
              <SelectWithSearch
                options={options || modelOptions}
                triggerClassName="!bg-bg-input"
                {...field}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name={getFieldWithPrefix('parameter')}
        render={({ field }) => (
          <FormItem className="flex justify-between gap-4 items-center">
            <FormLabel>{t('freedom')}</FormLabel>
            <FormControl>
              <div className="w-28">
                <Select
                  {...field}
                  onValueChange={(val) => {
                    handleChange(val);
                    field.onChange(val);
                  }}
                >
                  <SelectTrigger>
                    <SelectValue />
                  </SelectTrigger>
                  <SelectContent>
                    {parameterOptions.map((x) => (
                      <SelectItem value={x.value} key={x.value}>
                        {x.label}
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <SliderInputSwitchFormField
        name={getFieldWithPrefix('temperature')}
        checkName={getFieldWithPrefix('temperatureEnabled')}
        label="temperature"
        max={1}
        min={0}
        step={0.01}
        onChange={() => {
          checkParameterIsEquel();
        }}
      ></SliderInputSwitchFormField>
      <SliderInputSwitchFormField
        name={getFieldWithPrefix('top_p')}
        checkName={getFieldWithPrefix('topPEnabled')}
        label="topP"
        max={1}
        step={0.01}
        min={0}
        onChange={() => {
          checkParameterIsEquel();
        }}
      ></SliderInputSwitchFormField>
      <SliderInputSwitchFormField
        name={getFieldWithPrefix('presence_penalty')}
        checkName={getFieldWithPrefix('presencePenaltyEnabled')}
        label="presencePenalty"
        max={1}
        step={0.01}
        min={0}
        onChange={() => {
          checkParameterIsEquel();
        }}
      ></SliderInputSwitchFormField>
      <SliderInputSwitchFormField
        name={getFieldWithPrefix('frequency_penalty')}
        checkName={getFieldWithPrefix('frequencyPenaltyEnabled')}
        label="frequencyPenalty"
        max={1}
        step={0.01}
        min={0}
        onChange={() => {
          checkParameterIsEquel();
        }}
      ></SliderInputSwitchFormField>
      {/* <SliderInputSwitchFormField
        name={getFieldWithPrefix('max_tokens')}
        checkName="maxTokensEnabled"
        label="maxTokens"
        max={128000}
      ></SliderInputSwitchFormField> */}
    </div>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/next-search/search-setting-aisummery-config.tsx` is located in the `web/src/pages/next-search` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to next-search.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [embed-app-modal.tsx](embed-app-modal.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [mindmap-drawer.tsx](mindmap-drawer.tsx_docs.md)
- [search-home.tsx](search-home.tsx_docs.md)
- [search-setting.tsx](search-setting.tsx_docs.md)
- [search-view.tsx](search-view.tsx_docs.md)
- [searching.tsx](searching.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
