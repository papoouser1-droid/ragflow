# File Documentation: web/src/pages/agent/form/list-operations-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/list-operations-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 141
- **Characters**: 4,606
- **Size**: 4,606 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/list-operations-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 141 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `RetrievalPartialSchema`: Exported entity
- `FormSchema`: Exported entity

### Functions (1)

- `ListOperationsForm()`: Function definition

### Imports (21)

- `import NumberInput from '@/components/originui/number-input';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import {`
- `import { Separator } from '@/components/ui/separator';`
- `import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';`
- `import { buildOptions } from '@/utils/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm, useWatch } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 141
- Blank lines: 12 (8.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~129


## Dependencies and Imports

- `@/components/originui/number-input`
- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/separator`
- `@/hooks/logic-hooks/use-build-operator-options`
- `@/utils/form`
- `@hookform/resolvers/zod`
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
- `../components/prompt-editor`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/list-operations-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/list-operations-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ../components/query-variable, @/components/originui/number-input, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/separator, @/hooks/logic-hooks/use-build-operator-options, @/utils/form, @hookform/resolvers/zod, Array, DataOperationsOperatorOptions, Filter, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, Head, INextOperatorForm, JsonSchemaDataType, ListOperations, ListOperationsForm, ListOperationsFormSchemaType, ListOperationsOptions, NumberInput, Output, OutputSchema, PromptEditor, QueryVariable, RAGFlowFormItem, RetrievalPartialSchema, SelectWithSearch, Separator, Sort, SortMethod, SortMethodOptions, Tail, TopN, TypeScript, defaultValues, form...

---
*Generated by RAGFlow Repository Documentation Generator*
