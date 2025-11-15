# File Documentation: web/src/pages/agent/form/retrieval-form/next.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/retrieval-form/next.tsx`
- **Extension**: `.tsx`
- **Lines**: 140
- **Characters**: 4,468
- **Size**: 4,468 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Collapse } from '@/components/collapse';
import { CrossLanguageFormField } from '@/components/cross-language-form-field';
import { FormContainer } from '@/components/form-container';
import { KnowledgeBaseFormField } from '@/components/knowledge-base-item';
import {
  MetadataFilter,
  MetadataFilterSchema,
} from '@/components/metadata-filter';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { RerankFormFields } from '@/components/rerank';
import { SimilaritySliderFormField } from '@/components/similarity-slider';
import { TOCEnhanceFormField } from '@/components/toc-enhance-form-field';
import { TopNFormField } from '@/components/top-n-item';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Textarea } from '@/components/ui/textarea';
import { UseKnowledgeGraphFormField } from '@/components/use-knowledge-graph-item';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { initialRetrievalValues } from '../../constant';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { useValues } from './use-values';

export const RetrievalPartialSchema = {
  similarity_threshold: z.coerce.number(),
  keywords_similarity_weight: z.coerce.number(),
  top_n: z.coerce.number(),
  top_k: z.coerce.number(),
  kb_ids: z.array(z.string()),
  rerank_id: z.string(),
  empty_response: z.string(),
  cross_languages: z.array(z.string()),
  use_kg: z.boolean(),
  toc_enhance: z.boolean(),
  ...MetadataFilterSchema,
};

export const FormSchema = z.object({
  query: z.string().optional(),
  ...RetrievalPartialSchema,
});

export function EmptyResponseField() {
  const { t } = useTranslation();
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name="empty_response"
      render={({ field }) => (
        <FormItem>
          <FormLabel tooltip={t('chat.emptyResponseTip')}>
            {t('chat.emptyResponse')}
          </FormLabel>
          <FormControl>
            <Textarea
              placeholder={t('common.namePlaceholder')}
              {...field}
              autoComplete="off"
              rows={4}
            />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}

function RetrievalForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const outputList = useMemo(() => {
    return [
      {
        title: 'formalized_content',
        type: initialRetrievalValues.outputs.formalized_content.type,
      },
      {
        title: 'json',
        type: initialRetrievalValues.outputs.json.type,
      },
    ];
  }, []);

  const defaultValues = useValues(node);

  const form = useForm({
    defaultValues: defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <RAGFlowFormItem name="query" label={t('flow.query')}>
            <PromptEditor></PromptEditor>
          </RAGFlowFormItem>
          <KnowledgeBaseFormField showVariable></KnowledgeBaseFormField>
        </FormContainer>
        <Collapse title={<div>{t('flow.advancedSettings')}</div>}>
          <FormContainer>
            <SimilaritySliderFormField
              vectorSimilarityWeightName="keywords_similarity_weight"
              isTooltipShown
            ></SimilaritySliderFormField>
            <TopNFormField></TopNFormField>
            <RerankFormFields></RerankFormFields>
            <MetadataFilter canReference></MetadataFilter>
            <EmptyResponseField></EmptyResponseField>
            <CrossLanguageFormField name="cross_languages"></CrossLanguageFormField>
            <UseKnowledgeGraphFormField name="use_kg"></UseKnowledgeGraphFormField>
            <TOCEnhanceFormField name="toc_enhance"></TOCEnhanceFormField>
          </FormContainer>
        </Collapse>
        <Output list={outputList}></Output>
      </FormWrapper>
    </Form>
  );
}

export default memo(RetrievalForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/retrieval-form/next.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 140 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `RetrievalPartialSchema`: Exported entity
- `FormSchema`: Exported entity
- `EmptyResponseField`: Exported entity

### Functions (3)

- `EmptyResponseField()`: Function definition
- `RetrievalForm()`: Function definition
- `outputList()`: Function definition

### Imports (25)

- `import { Collapse } from '@/components/collapse';`
- `import { CrossLanguageFormField } from '@/components/cross-language-form-field';`
- `import { FormContainer } from '@/components/form-container';`
- `import { KnowledgeBaseFormField } from '@/components/knowledge-base-item';`
- `import {`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { RerankFormFields } from '@/components/rerank';`
- `import { SimilaritySliderFormField } from '@/components/similarity-slider';`
- `import { TOCEnhanceFormField } from '@/components/toc-enhance-form-field';`
- `import { TopNFormField } from '@/components/top-n-item';`

## Code Structure Analysis

- Total lines: 140
- Blank lines: 12 (8.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~128


## Dependencies and Imports

- `@/components/collapse`
- `@/components/cross-language-form-field`
- `@/components/form-container`
- `@/components/knowledge-base-item`
- `@/components/ragflow-form`
- `@/components/rerank`
- `@/components/similarity-slider`
- `@/components/toc-enhance-form-field`
- `@/components/top-n-item`
- `@/components/ui/textarea`
- `@/components/use-knowledge-graph-item`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../constant`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../components/form-wrapper`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/retrieval-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/retrieval-form/` directory
- Potential test file: `test_next.tsx`

## Keywords

../../constant, ../../hooks/use-watch-form-change, ../../interface, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ./use-values, @/components/collapse, @/components/cross-language-form-field, @/components/form-container, @/components/knowledge-base-item, @/components/ragflow-form, @/components/rerank, @/components/similarity-slider, @/components/toc-enhance-form-field, @/components/top-n-item, @/components/ui/textarea, @/components/use-knowledge-graph-item, @hookform/resolvers/zod, Collapse, CrossLanguageFormField, EmptyResponseField, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, KnowledgeBaseFormField, MetadataFilter, MetadataFilterSchema, Output, PromptEditor, RAGFlowFormItem, RerankFormFields, RetrievalForm, RetrievalPartialSchema, SimilaritySliderFormField, TOCEnhanceFormField, Textarea, TopNFormField, TypeScript, UseKnowledgeGraphFormField, defaultValues, form, hookform...

---
*Generated by RAGFlow Repository Documentation Generator*
