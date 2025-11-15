# Documentation: web/src/pages/dataset/dataset/set-meta-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/set-meta-dialog.tsx`
- **Size**: 3416 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset/set-meta-dialog.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset/set-meta-dialog.tsx` is located in the `web/src/pages/dataset/dataset` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset.

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

- [constant.ts](constant.ts_docs.md)
- [dataset-action-cell.tsx](dataset-action-cell.tsx_docs.md)
- [dataset-table.tsx](dataset-table.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [parsing-card.tsx](parsing-card.tsx_docs.md)
- [parsing-status-cell.tsx](parsing-status-cell.tsx_docs.md)
- [use-bulk-operate-dataset.tsx](use-bulk-operate-dataset.tsx_docs.md)
- [use-change-document-parser.ts](use-change-document-parser.ts_docs.md)
- [use-create-empty-document.ts](use-create-empty-document.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
