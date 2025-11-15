# Documentation: web/src/pages/datasets/dataset-creating-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/datasets/dataset-creating-dialog.tsx`
- **Size**: 4476 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/datasets/dataset-creating-dialog.tsx`.

## Original Source Code

```tsx
import { DataFlowSelect } from '@/components/data-pipeline-select';
import { ButtonLoading } from '@/components/ui/button';
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
import { FormLayout } from '@/constants/form';
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { useEffect } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  ChunkMethodItem,
  EmbeddingModelItem,
  ParseTypeItem,
} from '../dataset/dataset-setting/configuration/common-item';

const FormId = 'dataset-creating-form';

export function InputForm({ onOk }: IModalProps<any>) {
  const { t } = useTranslation();

  const FormSchema = z
    .object({
      name: z
        .string()
        .min(1, {
          message: t('knowledgeList.namePlaceholder'),
        })
        .trim(),
      parseType: z.number().optional(),
      embd_id: z
        .string()
        .min(1, {
          message: t('knowledgeConfiguration.embeddingModelPlaceholder'),
        })
        .trim(),
      parser_id: z.string().optional(),
      pipeline_id: z.string().optional(),
    })
    .superRefine((data, ctx) => {
      // When parseType === 1, parser_id is required
      if (
        data.parseType === 1 &&
        (!data.parser_id || data.parser_id.trim() === '')
      ) {
        ctx.addIssue({
          code: z.ZodIssueCode.custom,
          message: t('knowledgeList.parserRequired'),
          path: ['parser_id'],
        });
      }

      console.log('form-data', data);
      // When parseType === 1, pipline_id required
      if (data.parseType === 2 && !data.pipeline_id) {
        ctx.addIssue({
          code: z.ZodIssueCode.custom,
          message: t('knowledgeList.dataFlowRequired'),
          path: ['pipeline_id'],
        });
      }
    });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      name: '',
      parseType: 1,
      parser_id: '',
      embd_id: '',
    },
  });

  function onSubmit(data: z.infer<typeof FormSchema>) {
    console.log('submit', data);
    onOk?.(data);
  }

  const parseType = useWatch({
    control: form.control,
    name: 'parseType',
  });

  useEffect(() => {
    console.log('parseType', parseType);
    if (parseType === 1) {
      form.setValue('pipeline_id', '');
    }
  }, [parseType, form]);

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
              <FormLabel>
                <span className="text-destructive mr-1"> *</span>
                {t('knowledgeList.name')}
              </FormLabel>
              <FormControl>
                <Input
                  placeholder={t('knowledgeList.namePlaceholder')}
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        <EmbeddingModelItem line={2} isEdit={false} />
        <ParseTypeItem />
        {parseType === 1 && <ChunkMethodItem></ChunkMethodItem>}
        {parseType === 2 && (
          <DataFlowSelect
            isMult={false}
            showToDataPipeline={true}
            formFieldName="pipeline_id"
            layout={FormLayout.Vertical}
          />
        )}
      </form>
    </Form>
  );
}

export function DatasetCreatingDialog({
  hideModal,
  onOk,
  loading,
}: IModalProps<any>) {
  const { t } = useTranslation();

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent className="sm:max-w-[425px] focus-visible:!outline-none flex flex-col">
        <DialogHeader>
          <DialogTitle>{t('knowledgeList.createKnowledgeBase')}</DialogTitle>
        </DialogHeader>
        <InputForm onOk={onOk}></InputForm>
        <DialogFooter>
          <ButtonLoading type="submit" form={FormId} loading={loading}>
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

The file `web/src/pages/datasets/dataset-creating-dialog.tsx` is located in the `web/src/pages/datasets` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to datasets.

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

- [dataset-card.tsx](dataset-card.tsx_docs.md)
- [dataset-dataflow-creating-dialog.tsx](dataset-dataflow-creating-dialog.tsx_docs.md)
- [dataset-dropdown.tsx](dataset-dropdown.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [use-display-owner.ts](use-display-owner.ts_docs.md)
- [use-rename-dataset.ts](use-rename-dataset.ts_docs.md)
- [use-select-owners.ts](use-select-owners.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
