# File Documentation: web/src/components/rerank.tsx

## File Metadata

- **Path**: `web/src/components/rerank.tsx`
- **Extension**: `.tsx`
- **Lines**: 150
- **Characters**: 3,531
- **Size**: 3,531 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { LlmModelType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';
import { Select as AntSelect, Form, message, Slider } from 'antd';
import { useCallback } from 'react';
import { useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { SelectWithSearch } from './originui/select-with-search';
import { SliderInputFormField } from './slider-input-form-field';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from './ui/form';

type FieldType = {
  rerank_id?: string;
  top_k?: number;
};

export const RerankItem = () => {
  const { t } = useTranslate('knowledgeDetails');
  const allOptions = useSelectLlmOptionsByModelType();
  const [messageApi, contextHolder] = message.useMessage();

  const handleChange = useCallback(
    (val: string) => {
      if (val) {
        messageApi.open({
          type: 'warning',
          content: t('reRankModelWaring'),
        });
      }
    },
    [messageApi, t],
  );

  return (
    <>
      {contextHolder}
      <Form.Item
        label={t('rerankModel')}
        name={'rerank_id'}
        tooltip={t('rerankTip')}
      >
        <AntSelect
          options={allOptions[LlmModelType.Rerank]}
          allowClear
          placeholder={t('rerankPlaceholder')}
          onChange={handleChange}
        />
      </Form.Item>
    </>
  );
};

export const topKSchema = {
  top_k: z.number().optional(),
};

export const initialTopKValue = {
  top_k: 1024,
};

const Rerank = () => {
  const { t } = useTranslate('knowledgeDetails');

  return (
    <>
      <RerankItem></RerankItem>
      <Form.Item noStyle dependencies={['rerank_id']}>
        {({ getFieldValue }) => {
          const rerankId = getFieldValue('rerank_id');
          return (
            rerankId && (
              <Form.Item<FieldType>
                label={t('topK')}
                name={'top_k'}
                initialValue={1024}
                tooltip={t('topKTip')}
              >
                <Slider max={2048} min={1} />
              </Form.Item>
            )
          );
        }}
      </Form.Item>
    </>
  );
};

export default Rerank;

const RerankId = 'rerank_id';

function RerankFormField() {
  const form = useFormContext();
  const { t } = useTranslate('knowledgeDetails');
  const allOptions = useSelectLlmOptionsByModelType();
  const options = allOptions[LlmModelType.Rerank];

  return (
    <FormField
      control={form.control}
      name={RerankId}
      render={({ field }) => (
        <FormItem>
          <FormLabel tooltip={t('rerankTip')}>{t('rerankModel')}</FormLabel>
          <FormControl>
            <SelectWithSearch
              allowClear
              {...field}
              options={options}
            ></SelectWithSearch>
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}

export const rerankFormSchema = {
  [RerankId]: z.string().optional(),
  top_k: z.coerce.number().optional(),
};

export function RerankFormFields() {
  const { watch } = useFormContext();
  const { t } = useTranslate('knowledgeDetails');
  const rerankId = watch(RerankId);

  return (
    <>
      <RerankFormField></RerankFormField>
      {rerankId && (
        <SliderInputFormField
          name={'top_k'}
          label={t('topK')}
          max={2048}
          min={1}
          tooltip={t('topKTip')}
        ></SliderInputFormField>
      )}
    </>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/rerank.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 150 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `RerankItem`: Exported entity
- `topKSchema`: Exported entity
- `initialTopKValue`: Exported entity
- `rerankFormSchema`: Exported entity
- `RerankFormFields`: Exported entity

### Functions (5)

- `RerankItem()`: Function definition
- `handleChange()`: Function definition
- `Rerank()`: Function definition
- `RerankFormField()`: Function definition
- `RerankFormFields()`: Function definition

### Imports (10)

- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';`
- `import { Select as AntSelect, Form, message, Slider } from 'antd';`
- `import { useCallback } from 'react';`
- `import { useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { SelectWithSearch } from './originui/select-with-search';`
- `import { SliderInputFormField } from './slider-input-form-field';`
- `import {`

## Code Structure Analysis

- Total lines: 150
- Blank lines: 16 (10.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~134


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/hooks/llm-hooks`
- `antd`
- `react`
- `react-hook-form`
- `zod`
- `./originui/select-with-search`
- `./slider-input-form-field`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/` directory
- Potential test file: `test_rerank.tsx`

## Keywords

./originui/select-with-search, ./slider-input-form-field, @/constants/knowledge, @/hooks/common-hooks, @/hooks/llm-hooks, AntSelect, FieldType, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, Item, LlmModelType, Rerank, RerankFormField, RerankFormFields, RerankId, RerankItem, Select, SelectWithSearch, Slider, SliderInputFormField, TypeScript, allOptions, antd, form, handleChange, initialTopKValue, options, react, react-hook-form, rerankFormSchema, rerankId, topKSchema, zod

---
*Generated by RAGFlow Repository Documentation Generator*
