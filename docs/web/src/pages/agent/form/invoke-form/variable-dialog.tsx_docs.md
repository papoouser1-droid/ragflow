# Documentation: web/src/pages/agent/form/invoke-form/variable-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/variable-dialog.tsx`
- **Size**: 3506 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/invoke-form/variable-dialog.tsx`.

## Original Source Code

```tsx
import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogContent,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { isEmpty } from 'lodash';
import { useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { QueryVariable } from '../components/query-variable';
import { VariableFormSchemaType } from './schema';

type ModalFormProps = {
  initialValue: VariableFormSchemaType;
  otherThanCurrentQuery: VariableFormSchemaType[];
  submit(values: any): void;
};

const FormId = 'BeginParameterForm';

function VariableForm({
  initialValue,
  otherThanCurrentQuery,
  submit,
}: ModalFormProps) {
  const { t } = useTranslation();
  const FormSchema = z.object({
    key: z
      .string()
      .trim()
      .min(1)
      .refine(
        (value) =>
          !value || !otherThanCurrentQuery.some((x) => x.key === value),
        { message: 'The key cannot be repeated!' },
      ),
    ref: z.string(),
    value: z.string(),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    mode: 'onChange',
    defaultValues: {
      key: '',
      value: '',
      ref: '',
    },
  });

  useEffect(() => {
    if (!isEmpty(initialValue)) {
      form.reset(initialValue);
    }
  }, [form, initialValue]);

  function onSubmit(data: z.infer<typeof FormSchema>) {
    submit(data);
  }

  return (
    <Form {...form}>
      <form
        onSubmit={form.handleSubmit(onSubmit)}
        id={FormId}
        className="space-y-5"
        autoComplete="off"
      >
        <FormField
          name="key"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.key')}</FormLabel>
              <FormControl>
                <Input {...field} autoComplete="off" />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <QueryVariable name="ref" label={t('flow.ref')}></QueryVariable>
        <FormField
          name="value"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.value')}</FormLabel>
              <FormControl>
                <Input {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
      </form>
    </Form>
  );
}

export function VariableDialog({
  initialValue,
  hideModal,
  otherThanCurrentQuery,
  submit,
}: ModalFormProps & IModalProps<VariableFormSchemaType>) {
  const { t } = useTranslation();

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{t('flow.variableSettings')}</DialogTitle>
        </DialogHeader>
        <VariableForm
          initialValue={initialValue}
          otherThanCurrentQuery={otherThanCurrentQuery}
          submit={submit}
        ></VariableForm>
        <DialogFooter>
          <Button type="submit" form={FormId}>
            {t('modal.okText')}
          </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/invoke-form/variable-dialog.tsx` is located in the `web/src/pages/agent/form/invoke-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to invoke-form.

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

- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [schema.ts](schema.ts_docs.md)
- [use-edit-variable.ts](use-edit-variable.ts_docs.md)
- [variable-table.tsx](variable-table.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
