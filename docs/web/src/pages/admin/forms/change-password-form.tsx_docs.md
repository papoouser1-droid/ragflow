# Documentation: web/src/pages/admin/forms/change-password-form.tsx

## File Metadata

- **Path**: `web/src/pages/admin/forms/change-password-form.tsx`
- **Size**: 3852 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/admin/forms/change-password-form.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/admin/forms/change-password-form.tsx` is located in the `web/src/pages/admin/forms` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to forms.

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

- [email-form.tsx](email-form.tsx_docs.md)
- [import-excel-form.tsx](import-excel-form.tsx_docs.md)
- [role-form.tsx](role-form.tsx_docs.md)
- [user-form.tsx](user-form.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
