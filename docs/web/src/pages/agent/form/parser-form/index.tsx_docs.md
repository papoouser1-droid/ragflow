# Documentation: web/src/pages/agent/form/parser-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/parser-form/index.tsx`
- **Size**: 6478 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/parser-form/index.tsx`.

## Original Source Code

```tsx
import {
  SelectWithSearch,
  SelectWithSearchFlagOptionType,
} from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { BlockButton, Button } from '@/components/ui/button';
import { Form } from '@/components/ui/form';
import { Separator } from '@/components/ui/separator';
import { cn } from '@/lib/utils';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { useHover } from 'ahooks';
import { Trash2 } from 'lucide-react';
import { memo, useCallback, useMemo, useRef } from 'react';
import {
  UseFieldArrayRemove,
  useFieldArray,
  useForm,
  useFormContext,
} from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  FileType,
  InitialOutputFormatMap,
  initialParserValues,
} from '../../constant/pipeline';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { Output } from '../components/output';
import { OutputFormatFormField } from './common-form-fields';
import { EmailFormFields } from './email-form-fields';
import { ImageFormFields } from './image-form-fields';
import { PdfFormFields } from './pdf-form-fields';
import { buildFieldNameWithPrefix } from './utils';
import { AudioFormFields, VideoFormFields } from './video-form-fields';

const outputList = buildOutputList(initialParserValues.outputs);

const FileFormatWidgetMap = {
  [FileType.PDF]: PdfFormFields,
  [FileType.Video]: VideoFormFields,
  [FileType.Audio]: AudioFormFields,
  [FileType.Email]: EmailFormFields,
  [FileType.Image]: ImageFormFields,
};

type ParserItemProps = {
  name: string;
  index: number;
  fieldLength: number;
  remove: UseFieldArrayRemove;
  fileFormatOptions: SelectWithSearchFlagOptionType[];
};

export const FormSchema = z.object({
  setups: z.array(
    z.object({
      fileFormat: z.string().nullish(),
      output_format: z.string().optional(),
      parse_method: z.string().optional(),
      lang: z.string().optional(),
      fields: z.array(z.string()).optional(),
      llm_id: z.string().optional(),
      system_prompt: z.string().optional(),
    }),
  ),
});

export type ParserFormSchemaType = z.infer<typeof FormSchema>;

function ParserItem({
  name,
  index,
  fieldLength,
  remove,
  fileFormatOptions,
}: ParserItemProps) {
  const { t } = useTranslation();
  const form = useFormContext<ParserFormSchemaType>();
  const ref = useRef(null);
  const isHovering = useHover(ref);

  const prefix = `${name}.${index}`;
  const fileFormat = form.getValues(`setups.${index}.fileFormat`);

  const values = form.getValues();
  const parserList = values.setups.slice(); // Adding, deleting, or modifying the parser array will not change the reference.

  const filteredFileFormatOptions = useMemo(() => {
    const otherFileFormatList = parserList
      .filter((_, idx) => idx !== index)
      .map((x) => x.fileFormat);

    return fileFormatOptions.filter((x) => {
      return !otherFileFormatList.includes(x.value);
    });
  }, [fileFormatOptions, index, parserList]);

  const Widget =
    typeof fileFormat === 'string' && fileFormat in FileFormatWidgetMap
      ? FileFormatWidgetMap[fileFormat as keyof typeof FileFormatWidgetMap]
      : () => <></>;

  const handleFileTypeChange = useCallback(
    (value: FileType) => {
      form.setValue(
        `setups.${index}.output_format`,
        InitialOutputFormatMap[value],
        { shouldDirty: true, shouldValidate: true, shouldTouch: true },
      );
    },
    [form, index],
  );

  return (
    <section
      className={cn('space-y-5 py-2.5 rounded-md', {
        'bg-state-error-5': isHovering,
      })}
    >
      <div className="flex justify-between items-center">
        <span className="text-text-primary text-sm font-medium">
          Parser {index + 1}
        </span>
        {index > 0 && (
          <Button variant={'ghost'} onClick={() => remove(index)} ref={ref}>
            <Trash2 />
          </Button>
        )}
      </div>
      <RAGFlowFormItem
        name={buildFieldNameWithPrefix(`fileFormat`, prefix)}
        label={t('flow.fileFormats')}
      >
        {(field) => (
          <SelectWithSearch
            value={field.value}
            onChange={(val) => {
              field.onChange(val);
              handleFileTypeChange(val as FileType);
            }}
            options={filteredFileFormatOptions}
          ></SelectWithSearch>
        )}
      </RAGFlowFormItem>
      <Widget prefix={prefix} fileType={fileFormat as FileType}></Widget>
      <div className="hidden">
        <OutputFormatFormField
          prefix={prefix}
          fileType={fileFormat as FileType}
        />
      </div>
      {index < fieldLength - 1 && <Separator />}
    </section>
  );
}

const ParserForm = ({ node }: INextOperatorForm) => {
  const { t } = useTranslation();
  const defaultValues = useFormValues(initialParserValues, node);

  const FileFormatOptions = buildOptions(FileType, t, 'flow.fileFormatOptions');

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues,
    resolver: zodResolver(FormSchema),
    shouldUnregister: true,
  });

  const name = 'setups';
  const { fields, remove, append } = useFieldArray({
    name,
    control: form.control,
  });

  const add = useCallback(() => {
    append({
      fileFormat: null,
      output_format: '',
      parse_method: '',
      lang: '',
      fields: [],
      llm_id: '',
    });
  }, [append]);

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <form className="px-5">
        {fields.map((field, index) => {
          return (
            <ParserItem
              key={field.id}
              name={name}
              index={index}
              fieldLength={fields.length}
              remove={remove}
              fileFormatOptions={FileFormatOptions}
            ></ParserItem>
          );
        })}
        {fields.length < FileFormatOptions.length && (
          <BlockButton onClick={add} type="button" className="mt-2.5">
            {t('flow.addParser')}
          </BlockButton>
        )}
      </form>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default memo(ParserForm);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/parser-form/index.tsx` is located in the `web/src/pages/agent/form/parser-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parser-form.

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

- [common-form-fields.tsx](common-form-fields.tsx_docs.md)
- [email-form-fields.tsx](email-form-fields.tsx_docs.md)
- [image-form-fields.tsx](image-form-fields.tsx_docs.md)
- [interface.ts](interface.ts_docs.md)
- [pdf-form-fields.tsx](pdf-form-fields.tsx_docs.md)
- [use-set-initial-language.ts](use-set-initial-language.ts_docs.md)
- [utils.ts](utils.ts_docs.md)
- [video-form-fields.tsx](video-form-fields.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
