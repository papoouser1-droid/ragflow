# File Documentation: web/src/pages/agent/form/exesql-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/exesql-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 177
- **Characters**: 5,101
- **Size**: 5,101 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import NumberInput from '@/components/originui/number-input';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { ButtonLoading } from '@/components/ui/button';
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
import { memo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { initialExeSqlValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { ExeSQLOptions } from '../../options';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { FormSchema, useSubmitForm } from './use-submit-form';

const outputList = buildOutputList(initialExeSqlValues.outputs);

export function ExeSQLFormWidgets({ loading }: { loading: boolean }) {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  return (
    <>
      <FormField
        control={form.control}
        name="db_type"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('dbType')}</FormLabel>
            <FormControl>
              <SelectWithSearch
                {...field}
                options={ExeSQLOptions}
              ></SelectWithSearch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="database"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('database')}</FormLabel>
            <FormControl>
              <Input {...field}></Input>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="username"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('username')}</FormLabel>
            <FormControl>
              <Input {...field}></Input>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="host"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('host')}</FormLabel>
            <FormControl>
              <Input {...field}></Input>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="port"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('port')}</FormLabel>
            <FormControl>
              <NumberInput {...field} className="w-full"></NumberInput>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name="password"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('password')}</FormLabel>
            <FormControl>
              <Input {...field} type="password"></Input>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />

      <FormField
        control={form.control}
        name="max_records"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('maxRecords')}</FormLabel>
            <FormControl>
              <NumberInput {...field} className="w-full"></NumberInput>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />

      <div className="flex justify-end">
        <ButtonLoading loading={loading} type="submit">
          {t('test')}
        </ButtonLoading>
      </div>
    </>
  );
}

function ExeSQLForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialExeSqlValues, node);
  const { t } = useTranslation();

  const { onSubmit, loading } = useSubmitForm();

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues,
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper onSubmit={form.handleSubmit(onSubmit)}>
        <RAGFlowFormItem
          name="sql"
          label={t('flow.sqlStatement')}
          tooltip={t('flow.sqlStatementTip')}
        >
          <PromptEditor></PromptEditor>
        </RAGFlowFormItem>
        <ExeSQLFormWidgets loading={loading}></ExeSQLFormWidgets>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(ExeSQLForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/exesql-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 177 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ExeSQLFormWidgets`: Exported entity

### Functions (2)

- `ExeSQLFormWidgets()`: Function definition
- `ExeSQLForm()`: Function definition

### Imports (22)

- `import NumberInput from '@/components/originui/number-input';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 177
- Blank lines: 12 (6.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~165


## Dependencies and Imports

- `@/components/originui/number-input`
- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/hooks/common-hooks`
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
- `../components/prompt-editor`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/exesql-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/exesql-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ./use-submit-form, @/components/originui/number-input, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/button, @/components/ui/input, @/hooks/common-hooks, @hookform/resolvers/zod, ButtonLoading, ExeSQLForm, ExeSQLFormWidgets, ExeSQLOptions, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Input, NumberInput, Output, PromptEditor, RAGFlowFormItem, SelectWithSearch, TypeScript, defaultValues, form, hookform, outputList, react, react-hook-form, react-i18next, zod

---
*Generated by RAGFlow Repository Documentation Generator*
