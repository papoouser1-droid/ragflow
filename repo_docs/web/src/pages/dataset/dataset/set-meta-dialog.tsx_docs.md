# File Documentation: web/src/pages/dataset/dataset/set-meta-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/set-meta-dialog.tsx`
- **Extension**: `.tsx`
- **Lines**: 129
- **Characters**: 3,416
- **Size**: 3,416 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Dialog,
  DialogContent,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import { IModalProps } from '@/interfaces/common';
import { TagRenameId } from '@/pages/add-knowledge/constant';
import { useTranslation } from 'react-i18next';

import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';
import { z } from 'zod';

import { ButtonLoading } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { IDocumentInfo } from '@/interfaces/database/document';
import Editor, { loader } from '@monaco-editor/react';
import DOMPurify from 'dompurify';
import { useEffect } from 'react';

loader.config({ paths: { vs: '/vs' } });

export function SetMetaDialog({
  hideModal,
  onOk,
  loading,
  initialMetaData,
}: IModalProps<any> & { initialMetaData?: IDocumentInfo['meta_fields'] }) {
  const { t } = useTranslation();

  const FormSchema = z.object({
    meta: z
      .string()
      .min(1, {
        message: t('knowledgeDetails.pleaseInputJson'),
      })
      .trim()
      .refine(
        (value) => {
          try {
            JSON.parse(value);
            return true;
          } catch (error) {
            return false;
          }
        },
        { message: t('knowledgeDetails.pleaseInputJson') },
      ),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {},
  });

  async function onSubmit(data: z.infer<typeof FormSchema>) {
    const ret = await onOk?.(data.meta);
    if (ret) {
      hideModal?.();
    }
  }

  useEffect(() => {
    form.setValue('meta', JSON.stringify(initialMetaData, null, 4));
  }, [form, initialMetaData]);

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{t('knowledgeDetails.setMetaData')}</DialogTitle>
        </DialogHeader>
        <Form {...form}>
          <form
            onSubmit={form.handleSubmit(onSubmit)}
            className="space-y-6"
            id={TagRenameId}
          >
            <FormField
              control={form.control}
              name="meta"
              render={({ field }) => (
                <FormItem>
                  <FormLabel
                    tooltip={
                      <div
                        dangerouslySetInnerHTML={{
                          __html: DOMPurify.sanitize(
                            t('knowledgeDetails.documentMetaTips'),
                          ),
                        }}
                      ></div>
                    }
                  >
                    {t('knowledgeDetails.metaData')}
                  </FormLabel>
                  <FormControl>
                    <Editor
                      height={200}
                      defaultLanguage="json"
                      theme="vs-dark"
                      {...field}
                    />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          </form>
        </Form>
        <DialogFooter>
          <ButtonLoading type="submit" form={TagRenameId} loading={loading}>
            {t('common.save')}
          </ButtonLoading>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/set-meta-dialog.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 129 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `SetMetaDialog`: Exported entity

### Functions (4)

- `SetMetaDialog()`: Function definition
- `FormSchema()`: Function definition
- `onSubmit()`: Function definition
- `ret()`: Function definition

### Imports (13)

- `import {`
- `import { IModalProps } from '@/interfaces/common';`
- `import { TagRenameId } from '@/pages/add-knowledge/constant';`
- `import { useTranslation } from 'react-i18next';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import { IDocumentInfo } from '@/interfaces/database/document';`

## Code Structure Analysis

- Total lines: 129
- Blank lines: 10 (7.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~119


## Dependencies and Imports

- `@/interfaces/common`
- `@/pages/add-knowledge/constant`
- `react-i18next`
- `@hookform/resolvers/zod`
- `react-hook-form`
- `zod`
- `@/components/ui/button`
- `@/interfaces/database/document`
- `@monaco-editor/react`
- `dompurify`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset/` directory
- Potential test file: `test_set-meta-dialog.tsx`

## Keywords

@/components/ui/button, @/interfaces/common, @/interfaces/database/document, @/pages/add-knowledge/constant, @hookform/resolvers/zod, @monaco-editor/react, ButtonLoading, DOMPurify, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, Editor, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, IDocumentInfo, IModalProps, JSON, SetMetaDialog, TagRenameId, TypeScript, dompurify, form, hookform, monaco, onSubmit, react, react-hook-form, react-i18next, ret, zod

---
*Generated by RAGFlow Repository Documentation Generator*
