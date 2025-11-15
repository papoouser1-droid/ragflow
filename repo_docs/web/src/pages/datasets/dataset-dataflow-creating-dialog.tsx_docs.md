# File Documentation: web/src/pages/datasets/dataset-dataflow-creating-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/datasets/dataset-dataflow-creating-dialog.tsx`
- **Extension**: `.tsx`
- **Lines**: 124
- **Characters**: 3,001
- **Size**: 3,001 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
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
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  DataExtractKnowledgeItem,
  DataFlowItem,
  EmbeddingModelItem,
  ParseTypeItem,
  TeamItem,
} from '../dataset/dataset-setting/configuration/common-item';

const FormId = 'dataset-creating-form';

export function InputForm({ onOk }: IModalProps<any>) {
  const { t } = useTranslation();

  const FormSchema = z.object({
    name: z
      .string()
      .min(1, {
        message: t('knowledgeList.namePlaceholder'),
      })
      .trim(),
    parseType: z.number().optional(),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      name: '',
      parseType: 1,
    },
  });

  function onSubmit(data: z.infer<typeof FormSchema>) {
    onOk?.(data.name);
  }
  const parseType = useWatch({
    control: form.control,
    name: 'parseType',
  });
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
        <EmbeddingModelItem line={2} />
        <ParseTypeItem />
        {parseType === 2 && (
          <>
            <DataFlowItem />
            <DataExtractKnowledgeItem />
            <TeamItem />
          </>
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
      <DialogContent className="sm:max-w-[425px]">
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

This file is part of the RAGFlow repository located at `web/src/pages/datasets/dataset-dataflow-creating-dialog.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 124 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `InputForm`: Exported entity
- `DatasetCreatingDialog`: Exported entity

### Functions (3)

- `InputForm()`: Function definition
- `onSubmit()`: Function definition
- `DatasetCreatingDialog()`: Function definition

### Imports (10)

- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useForm, useWatch } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`
- `import {`

## Code Structure Analysis

- Total lines: 124
- Blank lines: 8 (6.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~116


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/input`
- `@/interfaces/common`
- `@hookform/resolvers/zod`
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
- Potential test file: `test_dataset-dataflow-creating-dialog.tsx`

## Keywords

@/components/ui/button, @/components/ui/input, @/interfaces/common, @hookform/resolvers/zod, ButtonLoading, DataExtractKnowledgeItem, DataFlowItem, DatasetCreatingDialog, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, EmbeddingModelItem, Form, FormControl, FormField, FormId, FormItem, FormLabel, FormMessage, FormSchema, IModalProps, Input, InputForm, ParseTypeItem, TeamItem, TypeScript, form, hookform, onSubmit, parseType, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
