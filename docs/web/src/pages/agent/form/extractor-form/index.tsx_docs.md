# Documentation: web/src/pages/agent/form/extractor-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/extractor-form/index.tsx`
- **Size**: 3637 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/extractor-form/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/extractor-form/index.tsx` is located in the `web/src/pages/agent/form/extractor-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to extractor-form.

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

- [use-switch-prompt.ts](use-switch-prompt.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
