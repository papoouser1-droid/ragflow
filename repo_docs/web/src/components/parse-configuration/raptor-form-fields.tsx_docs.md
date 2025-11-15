# File Documentation: web/src/components/parse-configuration/raptor-form-fields.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/raptor-form-fields.tsx`
- **Extension**: `.tsx`
- **Lines**: 247
- **Characters**: 8,115
- **Size**: 8,115 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormLayout } from '@/constants/form';
import { DocumentParserType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import {
  GenerateLogButton,
  GenerateType,
  IGenerateLogButtonProps,
} from '@/pages/dataset/dataset/generate-button/generate';
import random from 'lodash/random';
import { Shuffle } from 'lucide-react';
import { useCallback } from 'react';
import { useFormContext, useWatch } from 'react-hook-form';
import { SliderInputFormField } from '../slider-input-form-field';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '../ui/form';
import { ExpandedInput } from '../ui/input';
import { Radio } from '../ui/radio';
import { Textarea } from '../ui/textarea';

export const excludedParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.One,
  DocumentParserType.Picture,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Qa,
  DocumentParserType.Tag,
];

export const showRaptorParseConfiguration = (
  parserId: DocumentParserType | undefined,
) => {
  return !excludedParseMethods.some((x) => x === parserId);
};

export const excludedTagParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const showTagItems = (parserId: DocumentParserType) => {
  return !excludedTagParseMethods.includes(parserId);
};

const UseRaptorField = 'parser_config.raptor.use_raptor';
const RandomSeedField = 'parser_config.raptor.random_seed';
const MaxTokenField = 'parser_config.raptor.max_token';
const ThresholdField = 'parser_config.raptor.threshold';
const MaxCluster = 'parser_config.raptor.max_cluster';
const Prompt = 'parser_config.raptor.prompt';

// The three types "table", "resume" and "one" do not display this configuration.

const RaptorFormFields = ({
  data,
  onDelete,
}: {
  data: IGenerateLogButtonProps;
  onDelete: () => void;
}) => {
  const form = useFormContext();
  const { t } = useTranslate('knowledgeConfiguration');
  const useRaptor = useWatch({ name: UseRaptorField });

  const handleGenerate = useCallback(() => {
    form.setValue(RandomSeedField, random(10000));
  }, [form]);

  return (
    <>
      <FormField
        control={form.control}
        name={UseRaptorField}
        render={({ field }) => {
          return (
            <FormItem
              defaultChecked={false}
              className="items-center space-y-0 "
            >
              <div className="flex items-center gap-1">
                <FormLabel
                  tooltip={t('useRaptorTip')}
                  className="text-sm  w-1/4 whitespace-break-spaces"
                >
                  <div className="w-auto xl:w-20 2xl:w-24 3xl:w-28 4xl:w-auto ">
                    {t('useRaptor')}
                  </div>
                </FormLabel>
                <div className="w-3/4">
                  <FormControl>
                    <GenerateLogButton
                      {...data}
                      onDelete={onDelete}
                      className="w-full text-text-secondary"
                      status={1}
                      type={GenerateType.Raptor}
                    />
                  </FormControl>
                </div>
              </div>
              <div className="flex pt-1">
                <div className="w-1/4"></div>
                <FormMessage />
              </div>
            </FormItem>
          );
        }}
      />
      <FormField
        control={form.control}
        name={'parser_config.raptor.scope'}
        render={({ field }) => {
          return (
            <FormItem className=" items-center space-y-0 ">
              <div className="flex items-start">
                <FormLabel
                  tooltip={t('generationScopeTip')}
                  className="text-sm  whitespace-nowrap w-1/4"
                >
                  {t('generationScope')}
                </FormLabel>
                <div className="w-3/4">
                  <FormControl>
                    <Radio.Group {...field} disabled={!!data?.finish_at}>
                      <div className={'flex gap-4 w-full text-text-secondary '}>
                        <Radio value="dataset">{t('scopeDataset')}</Radio>
                        <Radio value="file">{t('scopeSingleFile')}</Radio>
                      </div>
                    </Radio.Group>
                  </FormControl>
                </div>
              </div>
              <div className="flex pt-1">
                <div className="w-1/4"></div>
                <FormMessage />
              </div>
            </FormItem>
          );
        }}
      />
      {useRaptor && (
        <div className="space-y-3">
          <FormField
            control={form.control}
            name={'parser_config.raptor.prompt'}
            render={({ field }) => {
              return (
                <FormItem className=" items-center space-y-0 ">
                  <div className="flex items-start">
                    <FormLabel
                      tooltip={t('promptTip')}
                      className="text-sm  whitespace-nowrap w-1/4"
                    >
                      {t('prompt')}
                    </FormLabel>
                    <div className="w-3/4">
                      <FormControl>
                        <Textarea
                          {...field}
                          rows={8}
                          onChange={(e) => {
                            field.onChange(e?.target?.value);
                          }}
                        />
                      </FormControl>
                    </div>
                  </div>
                  <div className="flex pt-1">
                    <div className="w-1/4"></div>
                    <FormMessage />
                  </div>
                </FormItem>
              );
            }}
          />
          <SliderInputFormField
            name={'parser_config.raptor.max_token'}
            label={t('maxToken')}
            tooltip={t('maxTokenTip')}
            max={2048}
            min={0}
            layout={FormLayout.Horizontal}
          ></SliderInputFormField>
          <SliderInputFormField
            name={'parser_config.raptor.threshold'}
            label={t('threshold')}
            tooltip={t('thresholdTip')}
            step={0.01}
            max={1}
            min={0}
            layout={FormLayout.Horizontal}
          ></SliderInputFormField>
          <SliderInputFormField
            name={'parser_config.raptor.max_cluster'}
            label={t('maxCluster')}
            tooltip={t('maxClusterTip')}
            max={1024}
            min={1}
            layout={FormLayout.Horizontal}
          ></SliderInputFormField>
          <FormField
            control={form.control}
            name={'parser_config.raptor.random_seed'}
            render={({ field }) => (
              <FormItem className=" items-center space-y-0 ">
                <div className="flex items-center">
                  <FormLabel className="text-sm  whitespace-wrap w-1/4">
                    {t('randomSeed')}
                  </FormLabel>
                  <div className="w-3/4">
                    <FormControl defaultValue={0}>
                      <ExpandedInput
                        {...field}
                        className="w-full"
                        defaultValue={0}
                        type="number"
                        suffix={
                          <Shuffle
                            className="size-3.5 cursor-pointer"
                            onClick={handleGenerate}
                          />
                        }
                      />
                    </FormControl>
                  </div>
                </div>
                <div className="flex pt-1">
                  <div className="w-1/4"></div>
                  <FormMessage />
                </div>
              </FormItem>
            )}
          />
        </div>
      )}
    </>
  );
};

export default RaptorFormFields;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/parse-configuration/raptor-form-fields.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 247 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `excludedParseMethods`: Exported entity
- `showRaptorParseConfiguration`: Exported entity
- `excludedTagParseMethods`: Exported entity
- `showTagItems`: Exported entity

### Functions (4)

- `showRaptorParseConfiguration()`: Function definition
- `showTagItems()`: Function definition
- `RaptorFormFields()`: Function definition
- `handleGenerate()`: Function definition

### Imports (13)

- `import { FormLayout } from '@/constants/form';`
- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import random from 'lodash/random';`
- `import { Shuffle } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useFormContext, useWatch } from 'react-hook-form';`
- `import { SliderInputFormField } from '../slider-input-form-field';`
- `import {`

## Code Structure Analysis

- Total lines: 247
- Blank lines: 11 (4.5%)
- Comment lines: ~1 (0.4%)
- Code lines: ~235


## Dependencies and Imports

- `@/constants/form`
- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `lodash/random`
- `lucide-react`
- `react`
- `react-hook-form`
- `../slider-input-form-field`
- `../ui/input`
- `../ui/radio`
- `../ui/textarea`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/parse-configuration`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/parse-configuration/` directory
- Potential test file: `test_raptor-form-fields.tsx`

## Keywords

../slider-input-form-field, ../ui/input, ../ui/radio, ../ui/textarea, @/constants/form, @/constants/knowledge, @/hooks/common-hooks, DocumentParserType, ExpandedInput, FormControl, FormField, FormItem, FormLabel, FormLayout, FormMessage, GenerateLogButton, GenerateType, Group, Horizontal, IGenerateLogButtonProps, KnowledgeGraph, MaxCluster, MaxTokenField, One, Picture, Prompt, Radio, RandomSeedField, Raptor, RaptorFormFields, Resume, Shuffle, SliderInputFormField, Table, Tag, Textarea, The, ThresholdField, TypeScript, UseRaptorField, excludedParseMethods, excludedTagParseMethods, form, handleGenerate, lodash/random, lucide-react, react, react-hook-form, showRaptorParseConfiguration, showTagItems...

---
*Generated by RAGFlow Repository Documentation Generator*
