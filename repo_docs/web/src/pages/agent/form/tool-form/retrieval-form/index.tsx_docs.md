# File Documentation: web/src/pages/agent/form/tool-form/retrieval-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tool-form/retrieval-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 68
- **Characters**: 2,578
- **Size**: 2,578 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Collapse } from '@/components/collapse';
import { CrossLanguageFormField } from '@/components/cross-language-form-field';
import { FormContainer } from '@/components/form-container';
import { KnowledgeBaseFormField } from '@/components/knowledge-base-item';
import { MetadataFilter } from '@/components/metadata-filter';
import { RerankFormFields } from '@/components/rerank';
import { SimilaritySliderFormField } from '@/components/similarity-slider';
import { TOCEnhanceFormField } from '@/components/toc-enhance-form-field';
import { TopNFormField } from '@/components/top-n-item';
import { Form } from '@/components/ui/form';
import { UseKnowledgeGraphFormField } from '@/components/use-knowledge-graph-item';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import { DescriptionField } from '../../components/description-field';
import { FormWrapper } from '../../components/form-wrapper';
import {
  EmptyResponseField,
  RetrievalPartialSchema,
} from '../../retrieval-form/next';
import { useValues } from '../use-values';
import { useWatchFormChange } from '../use-watch-change';

export const FormSchema = z.object({
  ...RetrievalPartialSchema,
  description: z.string().optional(),
});

const RetrievalForm = () => {
  const defaultValues = useValues();

  const form = useForm({
    defaultValues: defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <DescriptionField></DescriptionField>
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
      </FormWrapper>
    </Form>
  );
};

export default RetrievalForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/tool-form/retrieval-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 68 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FormSchema`: Exported entity

### Functions (1)

- `RetrievalForm()`: Function definition

### Imports (20)

- `import { Collapse } from '@/components/collapse';`
- `import { CrossLanguageFormField } from '@/components/cross-language-form-field';`
- `import { FormContainer } from '@/components/form-container';`
- `import { KnowledgeBaseFormField } from '@/components/knowledge-base-item';`
- `import { MetadataFilter } from '@/components/metadata-filter';`
- `import { RerankFormFields } from '@/components/rerank';`
- `import { SimilaritySliderFormField } from '@/components/similarity-slider';`
- `import { TOCEnhanceFormField } from '@/components/toc-enhance-form-field';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import { Form } from '@/components/ui/form';`

## Code Structure Analysis

- Total lines: 68
- Blank lines: 7 (10.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~61


## Dependencies and Imports

- `@/components/collapse`
- `@/components/cross-language-form-field`
- `@/components/form-container`
- `@/components/knowledge-base-item`
- `@/components/metadata-filter`
- `@/components/rerank`
- `@/components/similarity-slider`
- `@/components/toc-enhance-form-field`
- `@/components/top-n-item`
- `@/components/ui/form`
- `@/components/use-knowledge-graph-item`
- `@hookform/resolvers/zod`
- `i18next`
- `react-hook-form`
- `zod`
- `../../components/description-field`
- `../../components/form-wrapper`
- `../use-values`
- `../use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/tool-form/retrieval-form`.

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

- Other files in `web/src/pages/agent/form/tool-form/retrieval-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../components/description-field, ../../components/form-wrapper, ../use-values, ../use-watch-change, @/components/collapse, @/components/cross-language-form-field, @/components/form-container, @/components/knowledge-base-item, @/components/metadata-filter, @/components/rerank, @/components/similarity-slider, @/components/toc-enhance-form-field, @/components/top-n-item, @/components/ui/form, @/components/use-knowledge-graph-item, @hookform/resolvers/zod, Collapse, CrossLanguageFormField, DescriptionField, EmptyResponseField, Form, FormContainer, FormSchema, FormWrapper, KnowledgeBaseFormField, MetadataFilter, RerankFormFields, RetrievalForm, RetrievalPartialSchema, SimilaritySliderFormField, TOCEnhanceFormField, TopNFormField, TypeScript, UseKnowledgeGraphFormField, defaultValues, form, hookform, i18next, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
