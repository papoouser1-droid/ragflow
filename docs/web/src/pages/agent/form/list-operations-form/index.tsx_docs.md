# Documentation: web/src/pages/agent/form/list-operations-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/list-operations-form/index.tsx`
- **Size**: 4606 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/list-operations-form/index.tsx`.

## Original Source Code

```tsx
import NumberInput from '@/components/originui/number-input';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Separator } from '@/components/ui/separator';
import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  DataOperationsOperatorOptions,
  JsonSchemaDataType,
  ListOperations,
  SortMethod,
  initialListOperationsValues,
} from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output, OutputSchema } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { QueryVariable } from '../components/query-variable';

export const RetrievalPartialSchema = {
  query: z.string(),
  operations: z.string(),
  n: z.number().int().min(0).optional(),
  sort_method: z.string().optional(),
  filter: z
    .object({
      value: z.string().optional(),
      operator: z.string().optional(),
    })
    .optional(),
  ...OutputSchema,
};

export const FormSchema = z.object(RetrievalPartialSchema);

export type ListOperationsFormSchemaType = z.infer<typeof FormSchema>;

const outputList = buildOutputList(initialListOperationsValues.outputs);

function ListOperationsForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const defaultValues = useFormValues(initialListOperationsValues, node);

  const form = useForm<ListOperationsFormSchemaType>({
    defaultValues: defaultValues,
    mode: 'onChange',
    resolver: zodResolver(FormSchema),
    shouldUnregister: true,
  });

  const operations = useWatch({ control: form.control, name: 'operations' });

  const ListOperationsOptions = buildOptions(
    ListOperations,
    t,
    `flow.ListOperationsOptions`,
    true,
  );
  const SortMethodOptions = buildOptions(
    SortMethod,
    t,
    `flow.SortMethodOptions`,
    true,
  );
  const operatorOptions = useBuildSwitchOperatorOptions(
    DataOperationsOperatorOptions,
  );
  useWatchFormChange(node?.id, form, true);

  return (
    <Form {...form}>
      <FormWrapper>
        <QueryVariable
          name="query"
          className="flex-1"
          types={[JsonSchemaDataType.Array]}
        ></QueryVariable>
        <Separator />
        <RAGFlowFormItem name="operations" label={t('flow.operations')}>
          <SelectWithSearch options={ListOperationsOptions} />
        </RAGFlowFormItem>
        {[
          ListOperations.TopN,
          ListOperations.Head,
          ListOperations.Tail,
        ].includes(operations as ListOperations) && (
          <FormField
            control={form.control}
            name="n"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flowNum')}</FormLabel>
                <FormControl>
                  <NumberInput {...field} className="w-full"></NumberInput>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        )}
        {[ListOperations.Sort].includes(operations as ListOperations) && (
          <RAGFlowFormItem name="sort_method" label={t('flow.sortMethod')}>
            <SelectWithSearch options={SortMethodOptions} />
          </RAGFlowFormItem>
        )}
        {[ListOperations.Filter].includes(operations as ListOperations) && (
          <div className="flex items-center gap-2">
            <RAGFlowFormItem name="filter.operator" className="flex-1">
              <SelectWithSearch options={operatorOptions}></SelectWithSearch>
            </RAGFlowFormItem>
            <Separator className="w-2" />
            <RAGFlowFormItem name="filter.value" className="flex-1">
              <PromptEditor showToolbar={false} multiLine={false} />
            </RAGFlowFormItem>
          </div>
        )}
        <Output list={outputList} isFormRequired></Output>
      </FormWrapper>
    </Form>
  );
}

export default memo(ListOperationsForm);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/list-operations-form/index.tsx` is located in the `web/src/pages/agent/form/list-operations-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to list-operations-form.

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
