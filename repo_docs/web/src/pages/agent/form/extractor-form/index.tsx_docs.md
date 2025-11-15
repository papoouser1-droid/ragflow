# File Documentation: web/src/pages/agent/form/extractor-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/extractor-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 108
- **Characters**: 3,637
- **Size**: 3,637 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import { LargeModelFormField } from '@/components/large-model-form-field';
import { LlmSettingSchema } from '@/components/llm-setting-items/next';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Form } from '@/components/ui/form';
import { PromptEditor } from '@/pages/agent/form/components/prompt-editor';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  ContextGeneratorFieldName,
  initialExtractorValues,
} from '../../constant/pipeline';
import { useBuildNodeOutputOptions } from '../../hooks/use-build-options';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { useSwitchPrompt } from './use-switch-prompt';

export const FormSchema = z.object({
  field_name: z.string(),
  sys_prompt: z.string(),
  prompts: z.string().optional(),
  ...LlmSettingSchema,
});

export type ExtractorFormSchemaType = z.infer<typeof FormSchema>;

const outputList = buildOutputList(initialExtractorValues.outputs);

const ExtractorForm = ({ node }: INextOperatorForm) => {
  const defaultValues = useFormValues(initialExtractorValues, node);
  const { t } = useTranslation();

  const form = useForm<ExtractorFormSchemaType>({
    defaultValues,
    resolver: zodResolver(FormSchema),
    // mode: 'onChange',
  });

  const promptOptions = useBuildNodeOutputOptions(node?.id);

  const options = buildOptions(ContextGeneratorFieldName, t, 'flow');

  const {
    handleFieldNameChange,
    confirmSwitch,
    hideModal,
    visible,
    cancelSwitch,
  } = useSwitchPrompt(form);

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <LargeModelFormField></LargeModelFormField>
        <RAGFlowFormItem label={t('flow.fieldName')} name="field_name">
          {(field) => (
            <SelectWithSearch
              onChange={(value) => {
                field.onChange(value);
                handleFieldNameChange(value);
              }}
              value={field.value}
              placeholder={t('dataFlowPlaceholder')}
              options={options}
            ></SelectWithSearch>
          )}
        </RAGFlowFormItem>
        <RAGFlowFormItem label={t('flow.systemPrompt')} name="sys_prompt">
          <PromptEditor
            placeholder={t('flow.messagePlaceholder')}
            showToolbar={true}
            baseOptions={promptOptions}
          ></PromptEditor>
        </RAGFlowFormItem>
        <RAGFlowFormItem label={t('flow.userPrompt')} name="prompts">
          <PromptEditor
            showToolbar={true}
            baseOptions={promptOptions}
          ></PromptEditor>
        </RAGFlowFormItem>
        <Output list={outputList}></Output>
      </FormWrapper>
      {visible && (
        <ConfirmDeleteDialog
          title={t('flow.switchPromptMessage')}
          open
          onOpenChange={hideModal}
          onOk={confirmSwitch}
          onCancel={cancelSwitch}
        ></ConfirmDeleteDialog>
      )}
    </Form>
  );
};

export default memo(ExtractorForm);

```

## High-Level Overview

    // mode: 'onChange',

## Detailed Walkthrough

### Exports (1)

- `FormSchema`: Exported entity

### Functions (1)

- `ExtractorForm()`: Function definition

### Imports (22)

- `import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';`
- `import { LargeModelFormField } from '@/components/large-model-form-field';`
- `import { LlmSettingSchema } from '@/components/llm-setting-items/next';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Form } from '@/components/ui/form';`
- `import { PromptEditor } from '@/pages/agent/form/components/prompt-editor';`
- `import { buildOptions } from '@/utils/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`

## Code Structure Analysis

- Total lines: 108
- Blank lines: 12 (11.1%)
- Comment lines: ~1 (0.9%)
- Code lines: ~95


## Dependencies and Imports

- `@/components/confirm-delete-dialog`
- `@/components/large-model-form-field`
- `@/components/llm-setting-items/next`
- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/form`
- `@/pages/agent/form/components/prompt-editor`
- `@/utils/form`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../hooks/use-build-options`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/extractor-form`.

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

- Other files in `web/src/pages/agent/form/extractor-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks/use-build-options, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ./use-switch-prompt, @/components/confirm-delete-dialog, @/components/large-model-form-field, @/components/llm-setting-items/next, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/form, @/pages/agent/form/components/prompt-editor, @/utils/form, @hookform/resolvers/zod, ConfirmDeleteDialog, ContextGeneratorFieldName, ExtractorForm, ExtractorFormSchemaType, Form, FormSchema, FormWrapper, INextOperatorForm, LargeModelFormField, LlmSettingSchema, Output, PromptEditor, RAGFlowFormItem, SelectWithSearch, TypeScript, defaultValues, form, hookform, options, outputList, promptOptions, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
