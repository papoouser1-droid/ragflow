# File Documentation: web/src/pages/agent/form/bing-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/bing-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 132
- **Characters**: 3,573
- **Size**: 3,573 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
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
import { Input } from '@/components/ui/input';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialBingValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { BingCountryOptions, BingLanguageOptions } from '../../options';
import { FormWrapper } from '../components/form-wrapper';
import { QueryVariable } from '../components/query-variable';

export const BingFormSchema = {
  channel: z.string(),
  api_key: z.string(),
  country: z.string(),
  language: z.string(),
  top_n: z.number(),
};

export const FormSchema = z.object({
  query: z.string().optional(),
  ...BingFormSchema,
});

export function BingFormWidgets() {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  const options = useMemo(() => {
    return ['Webpages', 'News'].map((x) => ({ label: x, value: x }));
  }, []);

  return (
    <>
      <TopNFormField></TopNFormField>
      <FormField
        control={form.control}
        name="channel"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('channel')}</FormLabel>
            <FormControl>
              <SelectWithSearch {...field} options={options}></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="api_key"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('apiKey')}</FormLabel>
            <FormControl>
              <Input {...field}></Input>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="country"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('country')}</FormLabel>
            <FormControl>
              <SelectWithSearch
                {...field}
                options={BingCountryOptions}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="language"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('language')}</FormLabel>
            <FormControl>
              <SelectWithSearch
                {...field}
                options={BingLanguageOptions}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

function BingForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialBingValues, node);

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues,
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <QueryVariable></QueryVariable>
        <BingFormWidgets></BingFormWidgets>
      </FormWrapper>
    </Form>
  );
}

export default memo(BingForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/bing-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 132 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `BingFormSchema`: Exported entity
- `FormSchema`: Exported entity
- `BingFormWidgets`: Exported entity

### Functions (3)

- `BingFormWidgets()`: Function definition
- `options()`: Function definition
- `BingForm()`: Function definition

### Imports (16)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo, useMemo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialBingValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 132
- Blank lines: 11 (8.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~121


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/top-n-item`
- `@/components/ui/input`
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
- `../components/form-wrapper`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/bing-form`.

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

- Other files in `web/src/pages/agent/form/bing-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../components/form-wrapper, ../components/query-variable, @/components/originui/select-with-search, @/components/top-n-item, @/components/ui/input, @/hooks/common-hooks, @hookform/resolvers/zod, BingCountryOptions, BingForm, BingFormSchema, BingFormWidgets, BingLanguageOptions, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Input, News, QueryVariable, SelectWithSearch, TopNFormField, TypeScript, Webpages, defaultValues, form, hookform, options, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
