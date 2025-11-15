# File Documentation: web/src/pages/dataset/testing/testing-form.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/testing/testing-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 123
- **Characters**: 3,425
- **Size**: 3,425 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```tsx
'use client';

import { zodResolver } from '@hookform/resolvers/zod';
import { useForm, useWatch } from 'react-hook-form';
import { z } from 'zod';

import { CrossLanguageFormField } from '@/components/cross-language-form-field';
import { FormContainer } from '@/components/form-container';
import {
  initialTopKValue,
  RerankFormFields,
  topKSchema,
} from '@/components/rerank';
import {
  initialSimilarityThresholdValue,
  initialVectorSimilarityWeightValue,
  SimilaritySliderFormField,
  similarityThresholdSchema,
  vectorSimilarityWeightSchema,
} from '@/components/similarity-slider';
import { ButtonLoading } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { Textarea } from '@/components/ui/textarea';
import { UseKnowledgeGraphFormField } from '@/components/use-knowledge-graph-item';
import { useTestRetrieval } from '@/hooks/use-knowledge-request';
import { trim } from 'lodash';
import { Send } from 'lucide-react';
import { useEffect } from 'react';
import { useTranslation } from 'react-i18next';

type TestingFormProps = Pick<
  ReturnType<typeof useTestRetrieval>,
  'loading' | 'refetch' | 'setValues'
>;

export default function TestingForm({
  loading,
  refetch,
  setValues,
}: TestingFormProps) {
  const { t } = useTranslation();

  const formSchema = z.object({
    question: z.string().min(1, {
      message: t('knowledgeDetails.testTextPlaceholder'),
    }),
    ...similarityThresholdSchema,
    ...vectorSimilarityWeightSchema,
    ...topKSchema,
    use_kg: z.boolean().optional(),
  });

  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      ...initialSimilarityThresholdValue,
      ...initialVectorSimilarityWeightValue,
      ...initialTopKValue,
      use_kg: false,
    },
  });

  const question = form.watch('question');

  const values = useWatch({ control: form.control });

  useEffect(() => {
    setValues(values as Required<z.infer<typeof formSchema>>);
  }, [setValues, values]);

  function onSubmit() {
    refetch();
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        <FormContainer className="p-10">
          <SimilaritySliderFormField
            isTooltipShown={true}
          ></SimilaritySliderFormField>
          <RerankFormFields></RerankFormFields>
          <UseKnowledgeGraphFormField name="use_kg"></UseKnowledgeGraphFormField>
          <CrossLanguageFormField
            name={'cross_languages'}
          ></CrossLanguageFormField>
        </FormContainer>
        <FormField
          control={form.control}
          name="question"
          render={({ field }) => (
            <FormItem>
              {/* <FormLabel>{t('knowledgeDetails.testText')}</FormLabel> */}
              <FormControl>
                <Textarea {...field}></Textarea>
              </FormControl>

              <FormMessage />
            </FormItem>
          )}
        />
        <div className="flex justify-end">
          <ButtonLoading
            type="submit"
            disabled={!!!trim(question)}
            loading={loading}
          >
            {/* {!loading && <CirclePlay />} */}
            {t('knowledgeDetails.testingLabel')}
            <Send />
          </ButtonLoading>
        </div>
      </form>
    </Form>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/testing/testing-form.tsx`.

Based on the file structure and naming, it appears to be a testing - contains unit tests, integration tests, or test utilities.

The file contains approximately 123 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `TestingForm`: Exported entity

### Functions (3)

- `TestingForm()`: Function definition
- `values()`: Function definition
- `onSubmit()`: Function definition

### Imports (16)

- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useForm, useWatch } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { CrossLanguageFormField } from '@/components/cross-language-form-field';`
- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import {`
- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import { Textarea } from '@/components/ui/textarea';`

## Code Structure Analysis

- Total lines: 123
- Blank lines: 13 (10.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~110


## Dependencies and Imports

- `@hookform/resolvers/zod`
- `react-hook-form`
- `zod`
- `@/components/cross-language-form-field`
- `@/components/form-container`
- `@/components/ui/button`
- `@/components/ui/textarea`
- `@/components/use-knowledge-graph-item`
- `@/hooks/use-knowledge-request`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/testing`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `web/src/pages/dataset/testing/` directory

## Keywords

@/components/cross-language-form-field, @/components/form-container, @/components/ui/button, @/components/ui/textarea, @/components/use-knowledge-graph-item, @/hooks/use-knowledge-request, @hookform/resolvers/zod, ButtonLoading, CirclePlay, CrossLanguageFormField, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, Pick, Required, RerankFormFields, ReturnType, Send, SimilaritySliderFormField, TestingForm, TestingFormProps, Textarea, TypeScript, UseKnowledgeGraphFormField, form, formSchema, hookform, lodash, lucide-react, onSubmit, question, react, react-hook-form, react-i18next, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
