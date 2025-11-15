# Documentation: web/src/pages/admin/forms/user-form.tsx

## File Metadata

- **Path**: `web/src/pages/admin/forms/user-form.tsx`
- **Size**: 6240 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/admin/forms/user-form.tsx`.

## Original Source Code

```tsx
import { zodResolver } from '@hookform/resolvers/zod';
import { useQuery } from '@tanstack/react-query';
import { useCallback, useId, useMemo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';

import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';

import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import { listRoles } from '@/services/admin-service';

import EnterpriseFeature from '../components/enterprise-feature';
import { IS_ENTERPRISE } from '../utils';

interface CreateUserFormData {
  email: string;
  password: string;
  confirmPassword: string;
  role?: string;
}

interface CreateUserFormProps {
  id: string;
  form: ReturnType<typeof useForm<CreateUserFormData>>;
  onSubmit?: (data: CreateUserFormData) => void;
}

export const CreateUserForm = ({
  id,
  form,
  onSubmit = () => {},
}: CreateUserFormProps) => {
  const { t } = useTranslation();

  const { data: roleList } = useQuery({
    queryKey: ['admin/listRoles'],
    queryFn: async () => (await listRoles()).data.data.roles,
    enabled: IS_ENTERPRISE,
    retry: false,
  });

  return (
    <Form {...form}>
      <form
        id={id}
        onSubmit={form.handleSubmit(onSubmit)}
        className="space-y-6"
      >
        {/* Email field (editable) */}
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="text-sm font-medium">
                {t('admin.email')}
              </FormLabel>
              <FormControl>
                <Input
                  placeholder={t('admin.email')}
                  autoComplete="username"
                  className="mt-2 px-3 h-10 bg-bg-input border-border-button"
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Password field */}
        <FormField
          control={form.control}
          name="password"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="text-sm font-medium">
                {t('admin.password')}
              </FormLabel>
              <FormControl>
                <Input
                  type="password"
                  placeholder={t('admin.password')}
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
                {t('admin.confirmPassword')}
              </FormLabel>
              <FormControl>
                <Input
                  type="password"
                  placeholder={t('admin.confirmPassword')}
                  autoComplete="new-password"
                  className="mt-2 px-3 h-10 bg-bg-input border-border-button"
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        <EnterpriseFeature>
          {/* Role field */}
          {() => (
            <FormField
              control={form.control}
              name="role"
              render={({ field }) => (
                <FormItem>
                  <FormLabel className="text-sm font-medium">
                    {t('admin.role')}
                  </FormLabel>
                  <FormControl>
                    <Select {...field}>
                      <SelectTrigger className="w-full h-10">
                        <SelectValue />
                      </SelectTrigger>

                      <SelectContent className="bg-bg-base">
                        <SelectGroup>
                          {roleList?.map((role) => (
                            <SelectItem key={role.id} value={role.role_name}>
                              {role.role_name}
                            </SelectItem>
                          )) ?? (
                            <div className="text-text-secondary px-2 py-6 text-sm text-center">
                              {t('common.noData')}
                            </div>
                          )}
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                  </FormControl>
                </FormItem>
              )}
            />
          )}
        </EnterpriseFeature>
      </form>
    </Form>
  );
};

// Export the form validation state for parent component
function useCreateUserForm(props?: {
  defaultValues: Partial<CreateUserFormData>;
}) {
  const { t } = useTranslation();
  const id = useId();

  const schema = useMemo(() => {
    return z
      .object({
        email: z.string().email({ message: t('admin.invalidEmail') }),
        password: z.string().min(6, { message: t('admin.passwordMinLength') }),
        confirmPassword: z
          .string()
          .min(1, { message: t('admin.confirmPasswordRequired') }),
        role: z.string().optional(),
      })
      .refine((data) => data.password === data.confirmPassword, {
        message: t('admin.confirmPasswordDoNotMatch'),
        path: ['confirmPassword'],
      });
  }, [t]);

  const form = useForm<CreateUserFormData>({
    defaultValues: {
      email: '',
      password: '',
      confirmPassword: '',
      ...(props?.defaultValues ?? {}),
    },
    resolver: zodResolver(schema),
  });

  const FormComponent = useCallback(
    (props: Partial<CreateUserFormProps>) => (
      <CreateUserForm id={id} form={form} {...props} />
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

export default useCreateUserForm;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/admin/forms/user-form.tsx` is located in the `web/src/pages/admin/forms` directory.

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

- [change-password-form.tsx](change-password-form.tsx_docs.md)
- [email-form.tsx](email-form.tsx_docs.md)
- [import-excel-form.tsx](import-excel-form.tsx_docs.md)
- [role-form.tsx](role-form.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
