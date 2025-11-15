# File Documentation: web/src/pages/agent/form/message-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/message-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 140
- **Characters**: 4,428
- **Size**: 4,428 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/message-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 140 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `MessageForm()`: Function definition

### Imports (16)

- `import { FormContainer } from '@/components/form-container';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { X } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useFieldArray, useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 140
- Blank lines: 10 (7.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~130


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/button`
- `@/components/ui/select`
- `@hookform/resolvers/zod`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../constant`
- `../../interface`
- `../components/form-wrapper`
- `../components/prompt-editor`
- `./use-values`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/message-form`.

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

- Other files in `web/src/pages/agent/form/message-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../interface, ../components/form-wrapper, ../components/prompt-editor, ./use-values, ./use-watch-change, @/components/form-container, @/components/ui/button, @/components/ui/select, @hookform/resolvers/zod, BlockButton, Button, ExportFileType, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, MessageForm, Object, PromptEditor, RAGFlowSelect, TypeScript, form, hookform, lucide-react, react, react-hook-form, react-i18next, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
