# File Documentation: web/src/pages/admin/forms/change-password-form.tsx

## File Metadata

- **Path**: `web/src/pages/admin/forms/change-password-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 151
- **Characters**: 3,852
- **Size**: 3,852 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { zodResolver } from '@hookform/resolvers/zod';
import { useCallback, useId, useMemo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';

interface ChangePasswordFormData {
  newPassword: string;
  confirmPassword: string;
}

interface ChangePasswordFormProps {
  id: string;
  form: ReturnType<typeof useForm<ChangePasswordFormData>>;
  email?: string;
  onSubmit?: (data: ChangePasswordFormData) => void;
}

export const ChangePasswordForm = ({
  id,
  form,
  email,
  onSubmit = () => {},
}: ChangePasswordFormProps) => {
  const { t } = useTranslation();

  return (
    <Form {...form}>
      <form
        id={id}
        onSubmit={form.handleSubmit(onSubmit)}
        className="space-y-6"
      >
        {/* Email field (readonly) */}
        <div>
          <FormLabel className="text-sm font-medium">
            {t('admin.email')}
          </FormLabel>
          <Input
            value={email}
            readOnly
            className="mt-2 px-3 h-10 bg-bg-input border-border-button"
          />
        </div>

        {/* New password field */}
        <FormField
          control={form.control}
          name="newPassword"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="text-sm font-medium">
                {t('admin.newPassword')}
              </FormLabel>

              <FormControl>
                <Input
                  type="password"
                  placeholder={t('admin.newPassword')}
                  autoComplete="new-password"
                  className="mt-2 px-3 h-10 bg-bg-input border-border-button"
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Confirm password field */}
        <FormField
          control={form.control}
          name="confirmPassword"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="text-sm font-medium">
                {t('admin.confirmNewPassword')}
              </FormLabel>
              <FormControl>
                <Input
                  type="password"
                  placeholder={t('admin.confirmNewPassword')}
                  autoComplete="new-password"
                  className="mt-2 px-3 h-10 bg-bg-input border-border-button"
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
      </form>
    </Form>
  );
};

// Export the form validation state for parent component
function useChangePasswordForm() {
  const { t } = useTranslation();
  const id = useId();

  const schema = useMemo(() => {
    return z
      .object({
        newPassword: z
          .string()
          .min(8, { message: t('admin.passwordMinLength') }),
        confirmPassword: z
          .string()
          .min(8, { message: t('admin.confirmPasswordRequired') }),
      })
      .refine((data) => data.newPassword === data.confirmPassword, {
        message: t('admin.confirmPasswordDoNotMatch'),
        path: ['confirmPassword'],
      });
  }, [t]);

  const form = useForm<ChangePasswordFormData>({
    defaultValues: {
      newPassword: '',
      confirmPassword: '',
    },
    resolver: zodResolver(schema),
  });

  const FormComponent = useCallback(
    (props: Partial<ChangePasswordFormProps>) => (
      <ChangePasswordForm id={id} form={form} {...props} />
    ),
    [id, form],
  );

  return {
    schema,
    id,
    form,
    FormComponent,
  };
}

export default useChangePasswordForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/admin/forms/change-password-form.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 151 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ChangePasswordForm`: Exported entity

### Functions (4)

- `ChangePasswordForm()`: Function definition
- `useChangePasswordForm()`: Function definition
- `schema()`: Function definition
- `FormComponent()`: Function definition

### Imports (7)

- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useCallback, useId, useMemo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 151
- Blank lines: 14 (9.3%)
- Comment lines: ~1 (0.7%)
- Code lines: ~136


## Dependencies and Imports

- `@/components/ui/input`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/admin/forms`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/admin/forms/` directory
- Potential test file: `test_change-password-form.tsx`

## Keywords

@/components/ui/input, @hookform/resolvers/zod, ChangePasswordForm, ChangePasswordFormData, ChangePasswordFormProps, Confirm, Email, Export, Form, FormComponent, FormControl, FormField, FormItem, FormLabel, FormMessage, Input, New, Partial, ReturnType, TypeScript, form, hookform, id, react, react-hook-form, react-i18next, schema, useChangePasswordForm, zod

---
*Generated by RAGFlow Repository Documentation Generator*
