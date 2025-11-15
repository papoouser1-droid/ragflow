# Documentation: web/src/pages/agent/form/email-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/email-form/index.tsx`
- **Size**: 4100 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/email-form/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/email-form/index.tsx` is located in the `web/src/pages/agent/form/email-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to email-form.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
