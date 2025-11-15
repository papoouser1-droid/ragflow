# File Documentation: web/src/pages/agent/form/hierarchical-merger-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/hierarchical-merger-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 192
- **Characters**: 5,627
- **Size**: 5,627 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { BlockButton, Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader } from '@/components/ui/card';
import { Form, FormLabel } from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { zodResolver } from '@hookform/resolvers/zod';
import { Plus, Trash2 } from 'lucide-react';
import { memo } from 'react';
import { useFieldArray, useForm, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  Hierarchy,
  initialHierarchicalMergerValues,
} from '../../constant/pipeline';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';

const outputList = buildOutputList(initialHierarchicalMergerValues.outputs);

const HierarchyOptions = [
  { label: 'H1', value: Hierarchy.H1 },
  { label: 'H2', value: Hierarchy.H2 },
  { label: 'H3', value: Hierarchy.H3 },
  { label: 'H4', value: Hierarchy.H4 },
  { label: 'H5', value: Hierarchy.H5 },
];

export const FormSchema = z.object({
  hierarchy: z.string(),
  levels: z.array(
    z.object({
      expressions: z.array(
        z.object({
          expression: z.string().refine(
            (val) => {
              try {
                // Try converting the string to a RegExp
                new RegExp(val);
                return true;
              } catch {
                return false;
              }
            },
            {
              message: 'Must be a valid regular expression string',
            },
          ),
        }),
      ),
    }),
  ),
});

export type HierarchicalMergerFormSchemaType = z.infer<typeof FormSchema>;

type RegularExpressionsProps = {
  index: number;
  parentName: string;
  removeParent: (index: number) => void;
  isLatest: boolean;
};

export function RegularExpressions({
  index,
  parentName,
  isLatest,
  removeParent,
}: RegularExpressionsProps) {
  const { t } = useTranslation();
  const form = useFormContext();

  const name = `${parentName}.${index}.expressions`;

  const { fields, append, remove } = useFieldArray({
    name: name,
    control: form.control,
  });

  return (
    <Card>
      <CardHeader className="flex-row justify-between items-center">
        <span>H{index + 1}</span>
        {isLatest && (
          <Button
            type="button"
            variant={'ghost'}
            onClick={() => removeParent(index)}
          >
            <Trash2 />
          </Button>
        )}
      </CardHeader>
      <CardContent>
        <FormLabel required className="mb-2 text-text-secondary">
          {t('flow.regularExpressions')}
        </FormLabel>
        <section className="space-y-4">
          {fields.map((field, index) => (
            <div key={field.id} className="flex items-center gap-2">
              <div className="space-y-2 flex-1">
                <RAGFlowFormItem
                  name={`${name}.${index}.expression`}
                  label={'expression'}
                  labelClassName="!hidden"
                >
                  <Input className="!m-0"></Input>
                </RAGFlowFormItem>
              </div>
              {index === 0 ? (
                <Button
                  onClick={() => append({ expression: '' })}
                  variant={'ghost'}
                >
                  <Plus></Plus>
                </Button>
              ) : (
                <Button
                  type="button"
                  variant={'ghost'}
                  onClick={() => remove(index)}
                >
                  <Trash2 />
                </Button>
              )}
            </div>
          ))}
        </section>
      </CardContent>
    </Card>
  );
}

const HierarchicalMergerForm = ({ node }: INextOperatorForm) => {
  const { t } = useTranslation();
  const defaultValues = useFormValues(initialHierarchicalMergerValues, node);

  const form = useForm<HierarchicalMergerFormSchemaType>({
    defaultValues,
    resolver: zodResolver(FormSchema),
    mode: 'onChange',
  });

  const name = 'levels';

  const { fields, append, remove } = useFieldArray({
    name: name,
    control: form.control,
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <RAGFlowFormItem name={'hierarchy'} label={t('flow.hierarchy')}>
          <SelectWithSearch options={HierarchyOptions}></SelectWithSearch>
        </RAGFlowFormItem>
        {fields.map((field, index) => (
          <div key={field.id} className="flex items-center">
            <div className="flex-1">
              <RegularExpressions
                parentName={name}
                index={index}
                removeParent={remove}
                isLatest={index === fields.length - 1}
              ></RegularExpressions>
            </div>
          </div>
        ))}
        {fields.length < 5 && (
          <BlockButton
            onClick={() => append({ expressions: [{ expression: '' }] })}
          >
            {t('common.add')}
          </BlockButton>
        )}
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default memo(HierarchicalMergerForm);

```

## High-Level Overview

                // Try converting the string to a RegExp

## Detailed Walkthrough

### Exports (2)

- `FormSchema`: Exported entity
- `RegularExpressions`: Exported entity

### Functions (3)

- `FormSchema()`: Function definition
- `RegularExpressions()`: Function definition
- `HierarchicalMergerForm()`: Function definition

### Imports (19)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import { Card, CardContent, CardHeader } from '@/components/ui/card';`
- `import { Form, FormLabel } from '@/components/ui/form';`
- `import { Input } from '@/components/ui/input';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { Plus, Trash2 } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useFieldArray, useForm, useFormContext } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 192
- Blank lines: 17 (8.9%)
- Comment lines: ~1 (0.5%)
- Code lines: ~174


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/button`
- `@/components/ui/card`
- `@/components/ui/form`
- `@/components/ui/input`
- `@hookform/resolvers/zod`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/hierarchical-merger-form`.

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

- Other files in `web/src/pages/agent/form/hierarchical-merger-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/button, @/components/ui/card, @/components/ui/form, @/components/ui/input, @hookform/resolvers/zod, BlockButton, Button, Card, CardContent, CardHeader, Form, FormLabel, FormSchema, FormWrapper, HierarchicalMergerForm, HierarchicalMergerFormSchemaType, Hierarchy, HierarchyOptions, INextOperatorForm, Input, Must, Output, Plus, RAGFlowFormItem, RegExp, RegularExpressions, RegularExpressionsProps, SelectWithSearch, Trash2, Try, TypeScript, defaultValues, form, hookform, lucide-react, name, outputList, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
