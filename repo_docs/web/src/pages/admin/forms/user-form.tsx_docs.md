# File Documentation: web/src/pages/admin/forms/user-form.tsx

## File Metadata

- **Path**: `web/src/pages/admin/forms/user-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 224
- **Characters**: 6,240
- **Size**: 6,240 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/admin/forms/user-form.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 224 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `CreateUserForm`: Exported entity

### Functions (4)

- `CreateUserForm()`: Function definition
- `useCreateUserForm()`: Function definition
- `schema()`: Function definition
- `FormComponent()`: Function definition

### Imports (12)

- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useQuery } from '@tanstack/react-query';`
- `import { useCallback, useId, useMemo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import {`
- `import { listRoles } from '@/services/admin-service';`

## Code Structure Analysis

- Total lines: 224
- Blank lines: 19 (8.5%)
- Comment lines: ~1 (0.4%)
- Code lines: ~204


## Dependencies and Imports

- `@hookform/resolvers/zod`
- `@tanstack/react-query`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `@/components/ui/input`
- `@/services/admin-service`
- `../components/enterprise-feature`
- `../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/admin/forms`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

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

- Other files in `web/src/pages/admin/forms/` directory
- Potential test file: `test_user-form.tsx`

## Keywords

../components/enterprise-feature, ../utils, @/components/ui/input, @/services/admin-service, @hookform/resolvers/zod, @tanstack/react-query, Confirm, CreateUserForm, CreateUserFormData, CreateUserFormProps, Email, EnterpriseFeature, Export, Form, FormComponent, FormControl, FormField, FormItem, FormLabel, FormMessage, IS_ENTERPRISE, Input, Partial, Password, ReturnType, Role, Select, SelectContent, SelectGroup, SelectItem, SelectTrigger, SelectValue, TypeScript, form, hookform, id, react, react-hook-form, react-i18next, schema, tanstack, useCreateUserForm, zod

---
*Generated by RAGFlow Repository Documentation Generator*
