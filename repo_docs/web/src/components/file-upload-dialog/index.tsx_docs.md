# File Documentation: web/src/components/file-upload-dialog/index.tsx

## File Metadata

- **Path**: `web/src/components/file-upload-dialog/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 130
- **Characters**: 3,756
- **Size**: 3,756 bytes
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
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { TFunction } from 'i18next';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { FileUploader } from '../file-uploader';
import { RAGFlowFormItem } from '../ragflow-form';
import { Form } from '../ui/form';
import { Switch } from '../ui/switch';

function buildUploadFormSchema(t: TFunction) {
  const FormSchema = z.object({
    parseOnCreation: z.boolean().optional(),
    fileList: z
      .array(z.instanceof(File))
      .min(1, { message: t('fileManager.pleaseUploadAtLeastOneFile') }),
  });

  return FormSchema;
}

export type UploadFormSchemaType = z.infer<
  ReturnType<typeof buildUploadFormSchema>
>;

const UploadFormId = 'UploadFormId';

type UploadFormProps = {
  submit: (values?: UploadFormSchemaType) => void;
  showParseOnCreation?: boolean;
};
function UploadForm({ submit, showParseOnCreation }: UploadFormProps) {
  const { t } = useTranslation();
  const FormSchema = buildUploadFormSchema(t);

  type UploadFormSchemaType = z.infer<typeof FormSchema>;
  const form = useForm<UploadFormSchemaType>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      parseOnCreation: false,
      fileList: [],
    },
  });

  return (
    <Form {...form}>
      <form
        onSubmit={form.handleSubmit(submit)}
        id={UploadFormId}
        className="space-y-4"
      >
        {showParseOnCreation && (
          <RAGFlowFormItem
            name="parseOnCreation"
            label={t('fileManager.parseOnCreation')}
          >
            {(field) => (
              <Switch
                onCheckedChange={field.onChange}
                checked={field.value}
              ></Switch>
            )}
          </RAGFlowFormItem>
        )}
        <RAGFlowFormItem name="fileList" label={t('fileManager.file')}>
          {(field) => (
            <FileUploader
              value={field.value}
              onValueChange={field.onChange}
              accept={{ '*': [] }}
            />
          )}
        </RAGFlowFormItem>
      </form>
    </Form>
  );
}

type FileUploadDialogProps = IModalProps<UploadFormSchemaType> &
  Pick<UploadFormProps, 'showParseOnCreation'>;
export function FileUploadDialog({
  hideModal,
  onOk,
  loading,
  showParseOnCreation = false,
}: FileUploadDialogProps) {
  const { t } = useTranslation();

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{t('fileManager.uploadFile')}</DialogTitle>
        </DialogHeader>
        {/* <Tabs defaultValue="account">
          <TabsList className="grid w-full grid-cols-2 mb-4">
            <TabsTrigger value="account">{t('fileManager.local')}</TabsTrigger>
            <TabsTrigger value="password">{t('fileManager.s3')}</TabsTrigger>
          </TabsList>
          <TabsContent value="account">
            <UploadForm
              submit={onOk!}
              showParseOnCreation={showParseOnCreation}
            ></UploadForm>
          </TabsContent>
          <TabsContent value="password">{t('common.comingSoon')}</TabsContent>
        </Tabs> */}
        <UploadForm
          submit={onOk!}
          showParseOnCreation={showParseOnCreation}
        ></UploadForm>
        <DialogFooter>
          <ButtonLoading type="submit" loading={loading} form={UploadFormId}>
            {t('common.save')}
          </ButtonLoading>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/file-upload-dialog/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 130 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FileUploadDialog`: Exported entity

### Functions (3)

- `buildUploadFormSchema()`: Function definition
- `UploadForm()`: Function definition
- `FileUploadDialog()`: Function definition

### Imports (12)

- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import { IModalProps } from '@/interfaces/common';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { TFunction } from 'i18next';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`
- `import { FileUploader } from '../file-uploader';`
- `import { RAGFlowFormItem } from '../ragflow-form';`

## Code Structure Analysis

- Total lines: 130
- Blank lines: 10 (7.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~120


## Dependencies and Imports

- `@/components/ui/button`
- `@/interfaces/common`
- `@hookform/resolvers/zod`
- `i18next`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../file-uploader`
- `../ragflow-form`
- `../ui/form`
- `../ui/switch`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/file-upload-dialog`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/file-upload-dialog/` directory
- Potential test file: `test_index.tsx`

## Keywords

../file-uploader, ../ragflow-form, ../ui/form, ../ui/switch, @/components/ui/button, @/interfaces/common, @hookform/resolvers/zod, ButtonLoading, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, File, FileUploadDialog, FileUploadDialogProps, FileUploader, Form, FormSchema, IModalProps, Pick, RAGFlowFormItem, ReturnType, Switch, TFunction, Tabs, TabsContent, TabsList, TabsTrigger, TypeScript, UploadForm, UploadFormId, UploadFormProps, UploadFormSchemaType, buildUploadFormSchema, form, hookform, i18next, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
