# File Documentation: web/src/pages/agent/form/wikipedia-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/wikipedia-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 89
- **Characters**: 2,510
- **Size**: 2,510 bytes
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
import { memo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialWikipediaValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { LanguageOptions } from '../../options';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

export const WikipediaFormPartialSchema = {
  top_n: z.string(),
  language: z.string(),
};

const FormSchema = z.object({
  query: z.string(),
  ...WikipediaFormPartialSchema,
});

export function WikipediaFormWidgets() {
  const { t } = useTranslate('common');
  const form = useFormContext();

  return (
    <>
      <TopNFormField></TopNFormField>
      <FormField
        control={form.control}
        name="language"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('language')}</FormLabel>
            <FormControl>
              <SelectWithSearch {...field} options={LanguageOptions} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

const outputList = buildOutputList(initialWikipediaValues.outputs);

function WikipediaForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialWikipediaValues, node);

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
          <WikipediaFormWidgets></WikipediaFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(WikipediaForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/wikipedia-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 89 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `WikipediaFormPartialSchema`: Exported entity
- `WikipediaFormWidgets`: Exported entity

### Functions (2)

- `WikipediaFormWidgets()`: Function definition
- `WikipediaForm()`: Function definition

### Imports (18)

- `import { FormContainer } from '@/components/form-container';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialWikipediaValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 89
- Blank lines: 11 (12.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~78


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
- `../../options`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/wikipedia-form`.

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

- Other files in `web/src/pages/agent/form/wikipedia-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/originui/select-with-search, @/components/top-n-item, @/hooks/common-hooks, @hookform/resolvers/zod, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, LanguageOptions, Output, QueryVariable, SelectWithSearch, TopNFormField, TypeScript, WikipediaForm, WikipediaFormPartialSchema, WikipediaFormWidgets, defaultValues, form, hookform, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
