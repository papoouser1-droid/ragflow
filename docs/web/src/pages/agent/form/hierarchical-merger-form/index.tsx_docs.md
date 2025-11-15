# Documentation: web/src/pages/agent/form/hierarchical-merger-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/hierarchical-merger-form/index.tsx`
- **Size**: 5627 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/hierarchical-merger-form/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/hierarchical-merger-form/index.tsx` is located in the `web/src/pages/agent/form/hierarchical-merger-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hierarchical-merger-form.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
