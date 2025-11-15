# File Documentation: web/src/pages/user-setting/mcp/edit-mcp-form.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/mcp/edit-mcp-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 172
- **Characters**: 4,446
- **Size**: 4,446 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import { UseFormReturn } from 'react-hook-form';
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
import { RAGFlowSelect } from '@/components/ui/select';
import { IModalProps } from '@/interfaces/common';
import { buildOptions } from '@/utils/form';
import { loader } from '@monaco-editor/react';
import { Dispatch, SetStateAction } from 'react';
import { useTranslation } from 'react-i18next';

loader.config({ paths: { vs: '/vs' } });

export const FormId = 'EditMcpForm';

export enum ServerType {
  SSE = 'sse',
  StreamableHttp = 'streamable-http',
}

const ServerTypeOptions = buildOptions(ServerType);

export function useBuildFormSchema() {
  const { t } = useTranslation();

  const FormSchema = z.object({
    name: z
      .string()
      .min(1, {
        message: t('common.mcp.namePlaceholder'),
      })
      .regex(/^[a-zA-Z0-9_-]{1,64}$/, {
        message: t('common.mcp.nameRequired'),
      })
      .trim(),
    url: z
      .string()
      .url()
      .min(1, {
        message: t('common.mcp.urlPlaceholder'),
      })
      .trim(),
    server_type: z
      .string()
      .min(1, {
        message: t('common.pleaseSelect'),
      })
      .trim(),
    authorization_token: z.string().optional(),
  });

  return FormSchema;
}

export function EditMcpForm({
  form,
  onOk,
  setFieldChanged,
}: IModalProps<any> & {
  form: UseFormReturn<any>;
  setFieldChanged: Dispatch<SetStateAction<boolean>>;
}) {
  const { t } = useTranslation();
  const FormSchema = useBuildFormSchema();

  function onSubmit(data: z.infer<typeof FormSchema>) {
    onOk?.(data);
  }

  return (
    <Form {...form}>
      <form
        onSubmit={form.handleSubmit(onSubmit)}
        className="space-y-6"
        id={FormId}
      >
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel required>{t('common.name')}</FormLabel>
              <FormControl>
                <Input
                  placeholder={t('common.mcp.namePlaceholder')}
                  {...field}
                  autoComplete="off"
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="url"
          render={({ field }) => (
            <FormItem>
              <FormLabel required>{t('mcp.url')}</FormLabel>
              <FormControl>
                <Input
                  placeholder={t('common.mcp.urlPlaceholder')}
                  {...field}
                  autoComplete="off"
                  onChange={(e) => {
                    field.onChange(e.target.value.trim());
                    setFieldChanged(true);
                  }}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="server_type"
          render={({ field }) => (
            <FormItem>
              <FormLabel required>{t('mcp.serverType')}</FormLabel>
              <FormControl>
                <RAGFlowSelect
                  {...field}
                  autoComplete="off"
                  options={ServerTypeOptions}
                  onChange={(value) => {
                    field.onChange(value);
                    setFieldChanged(true);
                  }}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="authorization_token"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Authorization Token</FormLabel>
              <FormControl>
                <Input
                  placeholder={t('common.mcp.tokenPlaceholder')}
                  {...field}
                  autoComplete="off"
                  type="password"
                  onChange={(e) => {
                    field.onChange(e.target.value.trim());
                    setFieldChanged(true);
                  }}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
      </form>
    </Form>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/mcp/edit-mcp-form.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 172 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `FormId`: Exported entity
- `useBuildFormSchema`: Exported entity
- `EditMcpForm`: Exported entity

### Functions (3)

- `useBuildFormSchema()`: Function definition
- `EditMcpForm()`: Function definition
- `onSubmit()`: Function definition

### Imports (10)

- `import { UseFormReturn } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { buildOptions } from '@/utils/form';`
- `import { loader } from '@monaco-editor/react';`
- `import { Dispatch, SetStateAction } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 172
- Blank lines: 13 (7.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~159


## Dependencies and Imports

- `react-hook-form`
- `zod`
- `@/components/ui/input`
- `@/components/ui/select`
- `@/interfaces/common`
- `@/utils/form`
- `@monaco-editor/react`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/mcp`.

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

- Other files in `web/src/pages/user-setting/mcp/` directory
- Potential test file: `test_edit-mcp-form.tsx`

## Keywords

@/components/ui/input, @/components/ui/select, @/interfaces/common, @/utils/form, @monaco-editor/react, Authorization, Dispatch, EditMcpForm, Form, FormControl, FormField, FormId, FormItem, FormLabel, FormMessage, FormSchema, IModalProps, Input, RAGFlowSelect, SSE, ServerType, ServerTypeOptions, SetStateAction, StreamableHttp, Token, TypeScript, UseFormReturn, monaco, onSubmit, react, react-hook-form, react-i18next, useBuildFormSchema, zod

---
*Generated by RAGFlow Repository Documentation Generator*
