# File Documentation: web/src/pages/agent/form/google-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/google-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 140
- **Characters**: 4,134
- **Size**: 4,134 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormContainer } from '@/components/form-container';
import NumberInput from '@/components/originui/number-input';
import { SelectWithSearch } from '@/components/originui/select-with-search';
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
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialGoogleValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { GoogleCountryOptions, GoogleLanguageOptions } from '../../options';
import { buildOutputList } from '../../utils/build-output-list';
import { ApiKeyField } from '../components/api-key-field';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

const outputList = buildOutputList(initialGoogleValues.outputs);

export const GoogleFormPartialSchema = {
  api_key: z.string(),
  country: z.string(),
  language: z.string(),
};

export const FormSchema = z.object({
  ...GoogleFormPartialSchema,
  q: z.string(),
  start: z.number(),
  num: z.number(),
});

export function GoogleFormWidgets() {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  return (
    <>
      <FormField
        control={form.control}
        name={`country`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('country')}</FormLabel>
            <FormControl>
              <SelectWithSearch
                {...field}
                options={GoogleCountryOptions}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name={`language`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('language')}</FormLabel>
            <FormControl>
              <SelectWithSearch
                {...field}
                options={GoogleLanguageOptions}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

const GoogleForm = ({ node }: INextOperatorForm) => {
  const { t } = useTranslate('flow');
  const defaultValues = useFormValues(initialGoogleValues, node);

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <QueryVariable name="q"></QueryVariable>
        </FormContainer>
        <FormContainer>
          <ApiKeyField placeholder={t('apiKeyPlaceholder')}></ApiKeyField>
          <FormField
            control={form.control}
            name={`start`}
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flowStart')}</FormLabel>
                <FormControl>
                  <NumberInput {...field} className="w-full"></NumberInput>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name={`num`}
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
          <GoogleFormWidgets></GoogleFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default GoogleForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/google-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 140 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `GoogleFormPartialSchema`: Exported entity
- `FormSchema`: Exported entity
- `GoogleFormWidgets`: Exported entity

### Functions (2)

- `GoogleFormWidgets()`: Function definition
- `GoogleForm()`: Function definition

### Imports (18)

- `import { FormContainer } from '@/components/form-container';`
- `import NumberInput from '@/components/originui/number-input';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialGoogleValues } from '../../constant';`
- `import { useFormValues } from '../../hooks/use-form-values';`

## Code Structure Analysis

- Total lines: 140
- Blank lines: 11 (7.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~129


## Dependencies and Imports

- `@/components/form-container`
- `@/components/originui/number-input`
- `@/components/originui/select-with-search`
- `@/hooks/common-hooks`
- `@hookform/resolvers/zod`
- `react-hook-form`
- `zod`
- `../../constant`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../options`
- `../../utils/build-output-list`
- `../components/api-key-field`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/google-form`.

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

- Other files in `web/src/pages/agent/form/google-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../../utils/build-output-list, ../components/api-key-field, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/originui/number-input, @/components/originui/select-with-search, @/hooks/common-hooks, @hookform/resolvers/zod, ApiKeyField, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, GoogleCountryOptions, GoogleForm, GoogleFormPartialSchema, GoogleFormWidgets, GoogleLanguageOptions, INextOperatorForm, NumberInput, Output, QueryVariable, SelectWithSearch, TypeScript, defaultValues, form, hookform, outputList, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
