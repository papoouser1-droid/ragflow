# Documentation: web/src/pages/agent/form/message-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/message-form/index.tsx`
- **Size**: 4428 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/message-form/index.tsx`.

## Original Source Code

```tsx
import { FormContainer } from '@/components/form-container';
import { BlockButton, Button } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { RAGFlowSelect } from '@/components/ui/select';
import { zodResolver } from '@hookform/resolvers/zod';
import { X } from 'lucide-react';
import { memo } from 'react';
import { useFieldArray, useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { ExportFileType } from '../../constant';
import { INextOperatorForm } from '../../interface';
import { FormWrapper } from '../components/form-wrapper';
import { PromptEditor } from '../components/prompt-editor';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

function MessageForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const values = useValues(node);

  const FormSchema = z.object({
    content: z
      .array(
        z.object({
          value: z.string(),
        }),
      )
      .optional(),
    output_format: z.string().optional(),
  });

  const form = useForm({
    defaultValues: {
      ...values,
      output_format: values.output_format,
    },
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  const { fields, append, remove } = useFieldArray({
    name: 'content',
    control: form.control,
  });

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <FormItem>
            <FormLabel tooltip={t('flow.downloadFileTypeTip')}>
              {t('flow.downloadFileType')}
            </FormLabel>
            <FormField
              control={form.control}
              name={`output_format`}
              render={({ field }) => (
                <FormItem className="flex-1">
                  <FormControl>
                    <RAGFlowSelect
                      options={Object.keys(ExportFileType).map(
                        (key: string) => {
                          return {
                            value:
                              ExportFileType[
                                key as keyof typeof ExportFileType
                              ],
                            label: key,
                          };
                        },
                      )}
                      {...field}
                      onValueChange={field.onChange}
                      placeholder={t('flow.messagePlaceholder')}
                    ></RAGFlowSelect>
                  </FormControl>
                </FormItem>
              )}
            />
          </FormItem>
        </FormContainer>
        <FormContainer>
          <FormItem>
            <FormLabel tooltip={t('flow.msgTip')}>{t('flow.msg')}</FormLabel>
            <div className="space-y-4">
              {fields.map((field, index) => (
                <div key={field.id} className="flex items-start gap-2">
                  <FormField
                    control={form.control}
                    name={`content.${index}.value`}
                    render={({ field }) => (
                      <FormItem className="flex-1">
                        <FormControl>
                          <PromptEditor
                            {...field}
                            placeholder={t('flow.messagePlaceholder')}
                          ></PromptEditor>
                        </FormControl>
                      </FormItem>
                    )}
                  />
                  {fields.length > 1 && (
                    <Button
                      type="button"
                      variant={'ghost'}
                      onClick={() => remove(index)}
                    >
                      <X />
                    </Button>
                  )}
                </div>
              ))}

              <BlockButton
                type="button"
                onClick={() => append({ value: '' })} // "" will cause the inability to add, refer to: https://github.com/orgs/react-hook-form/discussions/8485#discussioncomment-2961861
              >
                {t('flow.addMessage')}
              </BlockButton>
            </div>
            <FormMessage />
          </FormItem>
        </FormContainer>
      </FormWrapper>
    </Form>
  );
}

export default memo(MessageForm);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/message-form/index.tsx` is located in the `web/src/pages/agent/form/message-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to message-form.

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

- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
