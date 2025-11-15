# Documentation: web/src/pages/profile-setting/mcp/edit-mcp-form.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/mcp/edit-mcp-form.tsx`
- **Size**: 4446 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/profile-setting/mcp/edit-mcp-form.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/profile-setting/mcp/edit-mcp-form.tsx` is located in the `web/src/pages/profile-setting/mcp` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to mcp.

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

- [edit-mcp-dialog.tsx](edit-mcp-dialog.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [mcp-card.tsx](mcp-card.tsx_docs.md)
- [mcp-operation.tsx](mcp-operation.tsx_docs.md)
- [tool-card.tsx](tool-card.tsx_docs.md)
- [use-bulk-operate-mcp.tsx](use-bulk-operate-mcp.tsx_docs.md)
- [use-edit-mcp.ts](use-edit-mcp.ts_docs.md)
- [use-export-mcp.ts](use-export-mcp.ts_docs.md)
- [use-import-mcp.ts](use-import-mcp.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
