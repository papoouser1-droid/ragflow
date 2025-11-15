# File Documentation: web/src/pages/agent/form/splitter-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/splitter-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 102
- **Characters**: 3,382
- **Size**: 3,382 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { DelimiterInput } from '@/components/delimiter-form-field';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { SliderInputFormField } from '@/components/slider-input-form-field';
import { BlockButton, Button } from '@/components/ui/button';
import { Form } from '@/components/ui/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { Trash2 } from 'lucide-react';
import { memo } from 'react';
import { useFieldArray, useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { initialSplitterValues } from '../../constant/pipeline';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';

const outputList = buildOutputList(initialSplitterValues.outputs);

export const FormSchema = z.object({
  chunk_token_size: z.number(),
  delimiters: z.array(
    z.object({
      value: z.string().optional(),
    }),
  ),
  overlapped_percent: z.number(), // 0.0 - 0.3 , 0% - 30%
});

export type SplitterFormSchemaType = z.infer<typeof FormSchema>;

const SplitterForm = ({ node }: INextOperatorForm) => {
  const defaultValues = useFormValues(initialSplitterValues, node);
  const { t } = useTranslation();

  const form = useForm<SplitterFormSchemaType>({
    defaultValues,
    resolver: zodResolver(FormSchema),
  });
  const name = 'delimiters';

  const { fields, append, remove } = useFieldArray({
    name: name,
    control: form.control,
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <SliderInputFormField
          name="chunk_token_size"
          max={2048}
          label={t('knowledgeConfiguration.chunkTokenNumber')}
        ></SliderInputFormField>
        <SliderInputFormField
          name="overlapped_percent"
          max={30}
          min={0}
          label={t('flow.overlappedPercent')}
        ></SliderInputFormField>
        <section>
          <span className="mb-2 inline-block">{t('flow.delimiters')}</span>
          <div className="space-y-4">
            {fields.map((field, index) => (
              <div key={field.id} className="flex items-center gap-2">
                <div className="space-y-2 flex-1">
                  <RAGFlowFormItem
                    name={`${name}.${index}.value`}
                    label="delimiter"
                    labelClassName="!hidden"
                  >
                    <DelimiterInput className="!m-0"></DelimiterInput>
                  </RAGFlowFormItem>
                </div>
                <Button
                  type="button"
                  variant={'ghost'}
                  onClick={() => remove(index)}
                >
                  <Trash2 />
                </Button>
              </div>
            ))}
          </div>
        </section>
        <BlockButton onClick={() => append({ value: '\n' })}>
          {t('common.add')}
        </BlockButton>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default memo(SplitterForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/splitter-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 102 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FormSchema`: Exported entity

### Functions (1)

- `SplitterForm()`: Function definition

### Imports (18)

- `import { DelimiterInput } from '@/components/delimiter-form-field';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { SliderInputFormField } from '@/components/slider-input-form-field';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import { Form } from '@/components/ui/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { Trash2 } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useFieldArray, useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 102
- Blank lines: 10 (9.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~92


## Dependencies and Imports

- `@/components/delimiter-form-field`
- `@/components/ragflow-form`
- `@/components/slider-input-form-field`
- `@/components/ui/button`
- `@/components/ui/form`
- `@hookform/resolvers/zod`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../constant/pipeline`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/splitter-form`.

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

- Other files in `web/src/pages/agent/form/splitter-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant/pipeline, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, @/components/delimiter-form-field, @/components/ragflow-form, @/components/slider-input-form-field, @/components/ui/button, @/components/ui/form, @hookform/resolvers/zod, BlockButton, Button, DelimiterInput, Form, FormSchema, FormWrapper, INextOperatorForm, Output, RAGFlowFormItem, SliderInputFormField, SplitterForm, SplitterFormSchemaType, Trash2, TypeScript, defaultValues, form, hookform, lucide-react, name, outputList, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
