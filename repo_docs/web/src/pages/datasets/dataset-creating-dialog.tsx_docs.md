# File Documentation: web/src/pages/datasets/dataset-creating-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/datasets/dataset-creating-dialog.tsx`
- **Extension**: `.tsx`
- **Lines**: 172
- **Characters**: 4,476
- **Size**: 4,476 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/datasets/dataset-creating-dialog.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 172 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `InputForm`: Exported entity
- `DatasetCreatingDialog`: Exported entity

### Functions (5)

- `InputForm()`: Function definition
- `FormSchema()`: Function definition
- `onSubmit()`: Function definition
- `parseType()`: Function definition
- `DatasetCreatingDialog()`: Function definition

### Imports (13)

- `import { DataFlowSelect } from '@/components/data-pipeline-select';`
- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { FormLayout } from '@/constants/form';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useEffect } from 'react';`
- `import { useForm, useWatch } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 172
- Blank lines: 13 (7.6%)
- Comment lines: ~2 (1.2%)
- Code lines: ~157


## Dependencies and Imports

- `@/components/data-pipeline-select`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/constants/form`
- `@/interfaces/common`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/datasets`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/datasets/` directory
- Potential test file: `test_dataset-creating-dialog.tsx`

## Keywords

@/components/data-pipeline-select, @/components/ui/button, @/components/ui/input, @/constants/form, @/interfaces/common, @hookform/resolvers/zod, ButtonLoading, ChunkMethodItem, DataFlowSelect, DatasetCreatingDialog, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, EmbeddingModelItem, Form, FormControl, FormField, FormId, FormItem, FormLabel, FormLayout, FormMessage, FormSchema, IModalProps, Input, InputForm, ParseTypeItem, TypeScript, Vertical, When, ZodIssueCode, form, hookform, onSubmit, parseType, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
