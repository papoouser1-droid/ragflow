# Documentation: web/src/components/rerank.tsx

## File Metadata

- **Path**: `web/src/components/rerank.tsx`
- **Size**: 3531 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/rerank.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/rerank.tsx` is located in the `web/src/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [auto-keywords-form-field.tsx](auto-keywords-form-field.tsx_docs.md)
- [auto-keywords-item.tsx](auto-keywords-item.tsx_docs.md)
- [avatar-upload.tsx](avatar-upload.tsx_docs.md)
- [bulk-operate-bar.tsx](bulk-operate-bar.tsx_docs.md)
- [card-container.tsx](card-container.tsx_docs.md)
- [collapse.tsx](collapse.tsx_docs.md)
- [confirm-delete-dialog.tsx](confirm-delete-dialog.tsx_docs.md)
- [copy-to-clipboard.tsx](copy-to-clipboard.tsx_docs.md)
- [cross-language-form-field.tsx](cross-language-form-field.tsx_docs.md)
- [cross-language-item.tsx](cross-language-item.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
