# File Documentation: web/src/pages/agent/form/variable-assigner-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/variable-assigner-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 101
- **Characters**: 3,124
- **Size**: 3,124 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Form } from '@/components/ui/form';
import { Separator } from '@/components/ui/separator';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  JsonSchemaDataType,
  Operations,
  initialDataOperationsValues,
} from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output, OutputSchema } from '../components/output';
import { QueryVariableList } from '../components/query-variable-list';

export const RetrievalPartialSchema = {
  query: z.array(z.object({ input: z.string().optional() })),
  operations: z.string(),
  select_keys: z.array(z.object({ name: z.string().optional() })).optional(),
  remove_keys: z.array(z.object({ name: z.string().optional() })).optional(),
  updates: z
    .array(
      z.object({ key: z.string().optional(), value: z.string().optional() }),
    )
    .optional(),
  rename_keys: z
    .array(
      z.object({
        old_key: z.string().optional(),
        new_key: z.string().optional(),
      }),
    )
    .optional(),
  filter_values: z
    .array(
      z.object({
        key: z.string().optional(),
        value: z.string().optional(),
        operator: z.string().optional(),
      }),
    )
    .optional(),
  ...OutputSchema,
};

export const FormSchema = z.object(RetrievalPartialSchema);

export type DataOperationsFormSchemaType = z.infer<typeof FormSchema>;

const outputList = buildOutputList(initialDataOperationsValues.outputs);

function VariableAssignerForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const defaultValues = useFormValues(initialDataOperationsValues, node);

  const form = useForm<DataOperationsFormSchemaType>({
    defaultValues: defaultValues,
    mode: 'onChange',
    resolver: zodResolver(FormSchema),
    shouldUnregister: true,
  });

  const OperationsOptions = buildOptions(
    Operations,
    t,
    `flow.operationsOptions`,
    true,
  );

  useWatchFormChange(node?.id, form, true);

  return (
    <Form {...form}>
      <FormWrapper>
        <QueryVariableList
          tooltip={t('flow.queryTip')}
          label={t('flow.query')}
          types={[JsonSchemaDataType.Array, JsonSchemaDataType.Object]}
        ></QueryVariableList>
        <Separator />
        <RAGFlowFormItem name="operations" label={t('flow.operations')}>
          <SelectWithSearch options={OperationsOptions} allowClear />
        </RAGFlowFormItem>

        <Output list={outputList} isFormRequired></Output>
      </FormWrapper>
    </Form>
  );
}

export default memo(VariableAssignerForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/variable-assigner-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 101 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `RetrievalPartialSchema`: Exported entity
- `FormSchema`: Exported entity

### Functions (1)

- `VariableAssignerForm()`: Function definition

### Imports (18)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Form } from '@/components/ui/form';`
- `import { Separator } from '@/components/ui/separator';`
- `import { buildOptions } from '@/utils/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 101
- Blank lines: 13 (12.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~88


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/form`
- `@/components/ui/separator`
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
- `../components/query-variable-list`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/variable-assigner-form`.

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

- Other files in `web/src/pages/agent/form/variable-assigner-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable-list, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/form, @/components/ui/separator, @/utils/form, @hookform/resolvers/zod, Array, DataOperationsFormSchemaType, Form, FormSchema, FormWrapper, INextOperatorForm, JsonSchemaDataType, Object, Operations, OperationsOptions, Output, OutputSchema, QueryVariableList, RAGFlowFormItem, RetrievalPartialSchema, SelectWithSearch, Separator, TypeScript, VariableAssignerForm, defaultValues, form, hookform, outputList, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
