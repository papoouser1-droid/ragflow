# File Documentation: web/src/pages/next-search/search-setting-aisummery-config.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/search-setting-aisummery-config.tsx`
- **Extension**: `.tsx`
- **Lines**: 237
- **Characters**: 6,941
- **Size**: 6,941 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

  // maxTokensEnabled: z.boolean(),

## Detailed Walkthrough

### Exports (2)

- `LlmSettingSchema`: Exported entity
- `LlmSettingFieldItems`: Exported entity

### Functions (5)

- `LlmSettingFieldItems()`: Function definition
- `handleChange()`: Function definition
- `parameterOptions()`: Function definition
- `getFieldWithPrefix()`: Function definition
- `checkParameterIsEquel()`: Function definition

### Imports (11)

- `import { SliderInputSwitchFormField } from '@/components/llm-setting-items/slider';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import {`
- `import {`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useComposeLlmOptionsByModelTypes } from '@/hooks/llm-hooks';`
- `import { camelCase, isEqual } from 'lodash';`
- `import { useCallback } from 'react';`
- `import { useFormContext } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 237
- Blank lines: 10 (4.2%)
- Comment lines: ~1 (0.4%)
- Code lines: ~226


## Dependencies and Imports

- `@/components/llm-setting-items/slider`
- `@/components/originui/select-with-search`
- `@/hooks/common-hooks`
- `@/hooks/llm-hooks`
- `lodash`
- `react`
- `react-hook-form`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-search`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/next-search/` directory
- Potential test file: `test_search-setting-aisummery-config.tsx`

## Keywords

@/components/llm-setting-items/slider, @/components/originui/select-with-search, @/hooks/common-hooks, @/hooks/llm-hooks, Chat, Custom, FormControl, FormField, FormItem, FormLabel, FormMessage, Image2text, LlmModelType, LlmSettingEnableSchema, LlmSettingFieldItems, LlmSettingFieldItemsProps, LlmSettingSchema, ModelVariableType, Object, Select, SelectContent, SelectItem, SelectTrigger, SelectValue, SelectWithSearch, SliderInputSwitchFormField, TypeScript, checkParameterIsEquel, element, enabledKeys, form, getFieldWithPrefix, handleChange, key, lodash, modelOptions, parameterOptions, parameterRealValue, parameterValue, react, react-hook-form, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
