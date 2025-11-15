# File Documentation: web/src/pages/agent/form/arxiv-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/arxiv-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 97
- **Characters**: 2,675
- **Size**: 2,675 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormContainer } from '@/components/form-container';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { TopNFormField } from '@/components/top-n-item';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialArXivValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

export const ArXivFormPartialSchema = {
  top_n: z.number(),
  sort_by: z.string(),
};

export const FormSchema = z.object({
  ...ArXivFormPartialSchema,
  query: z.string(),
});

export function ArXivFormWidgets() {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  const options = useMemo(() => {
    return ['submittedDate', 'lastUpdatedDate', 'relevance'].map((x) => ({
      value: x,
      label: t(x),
    }));
  }, [t]);

  return (
    <>
      <TopNFormField></TopNFormField>
      <FormField
        control={form.control}
        name={`sort_by`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('sortBy')}</FormLabel>
            <FormControl>
              <SelectWithSearch {...field} options={options}></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

const outputList = buildOutputList(initialArXivValues.outputs);

function ArXivForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialArXivValues, node);

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
          <ArXivFormWidgets></ArXivFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(ArXivForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/arxiv-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 97 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `ArXivFormPartialSchema`: Exported entity
- `FormSchema`: Exported entity
- `ArXivFormWidgets`: Exported entity

### Functions (3)

- `ArXivFormWidgets()`: Function definition
- `options()`: Function definition
- `ArXivForm()`: Function definition

### Imports (17)

- `import { FormContainer } from '@/components/form-container';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo, useMemo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialArXivValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 97
- Blank lines: 12 (12.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~85


## Dependencies and Imports

- `@/components/form-container`
- `@/components/originui/select-with-search`
- `@/components/top-n-item`
- `@/hooks/common-hooks`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `zod`
- `../../constant`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/arxiv-form`.

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

- Other files in `web/src/pages/agent/form/arxiv-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/originui/select-with-search, @/components/top-n-item, @/hooks/common-hooks, @hookform/resolvers/zod, ArXivForm, ArXivFormPartialSchema, ArXivFormWidgets, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Output, QueryVariable, SelectWithSearch, TopNFormField, TypeScript, defaultValues, form, hookform, options, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
