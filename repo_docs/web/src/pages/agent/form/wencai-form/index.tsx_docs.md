# File Documentation: web/src/pages/agent/form/wencai-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/wencai-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 98
- **Characters**: 2,721
- **Size**: 2,721 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormContainer } from '@/components/form-container';
import { TopNFormField } from '@/components/top-n-item';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { RAGFlowSelect } from '@/components/ui/select';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { initialWenCaiValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { WenCaiQueryTypeOptions } from '../../options';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

export const WenCaiPartialSchema = {
  top_n: z.number(),
  query_type: z.string(),
};

export const FormSchema = z.object({
  ...WenCaiPartialSchema,
  query: z.string(),
});

export function WenCaiFormWidgets() {
  const { t } = useTranslation();
  const form = useFormContext();

  const wenCaiQueryTypeOptions = useMemo(() => {
    return WenCaiQueryTypeOptions.map((x) => ({
      value: x,
      label: t(`flow.wenCaiQueryTypeOptions.${x}`),
    }));
  }, [t]);

  return (
    <>
      <TopNFormField max={99}></TopNFormField>
      <FormField
        control={form.control}
        name="query_type"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('flow.queryType')}</FormLabel>
            <FormControl>
              <RAGFlowSelect {...field} options={wenCaiQueryTypeOptions} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

const outputList = buildOutputList(initialWenCaiValues.outputs);

function WenCaiForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialWenCaiValues, node);

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <QueryVariable></QueryVariable>
        </FormContainer>
        <FormContainer>
          <WenCaiFormWidgets></WenCaiFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(WenCaiForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/wencai-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 98 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `WenCaiPartialSchema`: Exported entity
- `FormSchema`: Exported entity
- `WenCaiFormWidgets`: Exported entity

### Functions (3)

- `WenCaiFormWidgets()`: Function definition
- `wenCaiQueryTypeOptions()`: Function definition
- `WenCaiForm()`: Function definition

### Imports (18)

- `import { FormContainer } from '@/components/form-container';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo, useMemo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`
- `import { initialWenCaiValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 98
- Blank lines: 12 (12.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~86


## Dependencies and Imports

- `@/components/form-container`
- `@/components/top-n-item`
- `@/components/ui/select`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../constant`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../options`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/wencai-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/wencai-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/top-n-item, @/components/ui/select, @hookform/resolvers/zod, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Output, QueryVariable, RAGFlowSelect, TopNFormField, TypeScript, WenCaiForm, WenCaiFormWidgets, WenCaiPartialSchema, WenCaiQueryTypeOptions, defaultValues, form, hookform, outputList, react, react-hook-form, react-i18next, wenCaiQueryTypeOptions, zod

---
*Generated by RAGFlow Repository Documentation Generator*
