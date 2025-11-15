# File Documentation: web/src/pages/agent/form/invoke-form/variable-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/variable-dialog.tsx`
- **Extension**: `.tsx`
- **Lines**: 144
- **Characters**: 3,506
- **Size**: 3,506 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/invoke-form/variable-dialog.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 144 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `VariableDialog`: Exported entity

### Functions (5)

- `VariableForm()`: Function definition
- `FormSchema()`: Function definition
- `form()`: Function definition
- `onSubmit()`: Function definition
- `VariableDialog()`: Function definition

### Imports (13)

- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { isEmpty } from 'lodash';`
- `import { useEffect } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 144
- Blank lines: 10 (6.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~134


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/input`
- `@/interfaces/common`
- `@hookform/resolvers/zod`
- `lodash`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../components/query-variable`
- `./schema`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/invoke-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/invoke-form/` directory
- Potential test file: `test_variable-dialog.tsx`

## Keywords

../components/query-variable, ./schema, @/components/ui/button, @/components/ui/input, @/interfaces/common, @hookform/resolvers/zod, BeginParameterForm, Button, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, Form, FormControl, FormField, FormId, FormItem, FormLabel, FormMessage, FormSchema, IModalProps, Input, ModalFormProps, QueryVariable, The, TypeScript, VariableDialog, VariableForm, VariableFormSchemaType, form, hookform, lodash, onSubmit, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
