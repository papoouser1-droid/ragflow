# File Documentation: web/src/pages/agent/form/email-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/email-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 162
- **Characters**: 4,100
- **Size**: 4,100 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormContainer } from '@/components/form-container';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { ReactNode } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialEmailValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';

interface InputFormFieldProps {
  name: string;
  label: ReactNode;
  type?: string;
}

function InputFormField({ name, label, type }: InputFormFieldProps) {
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={name}
      render={({ field }) => (
        <FormItem>
          <FormLabel>{label}</FormLabel>
          <FormControl>
            <Input {...field} type={type}></Input>
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}

function PromptFormField({ name, label }: InputFormFieldProps) {
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={name}
      render={({ field }) => (
        <FormItem>
          <FormLabel>{label}</FormLabel>
          <FormControl>
            <PromptEditor
              {...field}
              showToolbar={false}
              multiLine={false}
            ></PromptEditor>
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}
export function EmailFormWidgets() {
  const { t } = useTranslate('flow');

  return (
    <>
      <InputFormField
        name="smtp_server"
        label={t('smtpServer')}
      ></InputFormField>
      <InputFormField
        name="smtp_port"
        label={t('smtpPort')}
        type="number"
      ></InputFormField>
      <InputFormField name="email" label={t('senderEmail')}></InputFormField>
      <InputFormField
        name="password"
        label={t('authCode')}
        type="password"
      ></InputFormField>
      <InputFormField
        name="sender_name"
        label={t('senderName')}
      ></InputFormField>
    </>
  );
}

export const EmailFormPartialSchema = {
  smtp_server: z.string(),
  smtp_port: z.number(),
  email: z.string(),
  password: z.string(),
  sender_name: z.string(),
};

const FormSchema = z.object({
  to_email: z.string(),
  cc_email: z.string(),
  content: z.string(),
  subject: z.string(),
  ...EmailFormPartialSchema,
});

const outputList = buildOutputList(initialEmailValues.outputs);

const EmailForm = ({ node }: INextOperatorForm) => {
  const { t } = useTranslate('flow');
  const defaultValues = useFormValues(initialEmailValues, node);

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <PromptFormField
            name="to_email"
            label={t('toEmail')}
          ></PromptFormField>
          <PromptFormField
            name="cc_email"
            label={t('ccEmail')}
          ></PromptFormField>
          <PromptFormField
            name="content"
            label={t('content')}
          ></PromptFormField>
          <PromptFormField
            name="subject"
            label={t('subject')}
          ></PromptFormField>
          <EmailFormWidgets></EmailFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default EmailForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/email-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 162 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `EmailFormWidgets`: Exported entity
- `EmailFormPartialSchema`: Exported entity

### Functions (4)

- `InputFormField()`: Function definition
- `PromptFormField()`: Function definition
- `EmailFormWidgets()`: Function definition
- `EmailForm()`: Function definition

### Imports (16)

- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { ReactNode } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialEmailValues } from '../../constant';`
- `import { useFormValues } from '../../hooks/use-form-values';`

## Code Structure Analysis

- Total lines: 162
- Blank lines: 15 (9.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~147


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/input`
- `@/hooks/common-hooks`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `zod`
- `../../constant`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `../components/prompt-editor`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/email-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/email-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/prompt-editor, @/components/form-container, @/components/ui/input, @/hooks/common-hooks, @hookform/resolvers/zod, EmailForm, EmailFormPartialSchema, EmailFormWidgets, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Input, InputFormField, InputFormFieldProps, Output, PromptEditor, PromptFormField, ReactNode, TypeScript, defaultValues, form, hookform, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
