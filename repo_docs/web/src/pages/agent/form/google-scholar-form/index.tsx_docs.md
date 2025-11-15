# File Documentation: web/src/pages/agent/form/google-scholar-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/google-scholar-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 167
- **Characters**: 4,765
- **Size**: 4,765 bytes
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
import { Switch } from '@/components/ui/switch';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { DatePicker, DatePickerProps } from 'antd';
import dayjs from 'dayjs';
import { memo, useCallback, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialGoogleScholarValues } from '../../constant';
import { useBuildSortOptions } from '../../form-hooks';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

// TODO: To be replaced
const YearPicker = ({
  onChange,
  value,
}: {
  onChange?: (val: number | undefined) => void;
  value?: number | undefined;
}) => {
  const handleChange: DatePickerProps['onChange'] = useCallback(
    (val: any) => {
      const nextVal = val?.format('YYYY');
      onChange?.(nextVal ? Number(nextVal) : undefined);
    },
    [onChange],
  );
  // The year needs to be converted into a number and saved to the backend
  const nextValue = useMemo(() => {
    if (value) {
      return dayjs(value.toString());
    }
    return undefined;
  }, [value]);

  return <DatePicker picker="year" onChange={handleChange} value={nextValue} />;
};

export function GoogleScholarFormWidgets() {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  const options = useBuildSortOptions();

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
      <FormField
        control={form.control}
        name={`year_low`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('yearLow')}</FormLabel>
            <FormControl>
              <YearPicker {...field}></YearPicker>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name={`year_high`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('yearHigh')}</FormLabel>
            <FormControl>
              <YearPicker {...field}></YearPicker>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name={`patents`}
        render={({ field }) => (
          <FormItem className="flex-1">
            <FormLabel>{t('patents')}</FormLabel>
            <FormControl>
              <Switch
                checked={field.value}
                onCheckedChange={field.onChange}
              ></Switch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

export const GoogleScholarFormPartialSchema = {
  top_n: z.number(),
  sort_by: z.string(),
  year_low: z.number(),
  year_high: z.number(),
  patents: z.boolean(),
};

export const FormSchema = z.object({
  ...GoogleScholarFormPartialSchema,
  query: z.string(),
});

const outputList = buildOutputList(initialGoogleScholarValues.outputs);

function GoogleScholarForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialGoogleScholarValues, node);

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
          <GoogleScholarFormWidgets></GoogleScholarFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(GoogleScholarForm);

```

## High-Level Overview

// TODO: To be replaced
  // The year needs to be converted into a number and saved to the backend

## Detailed Walkthrough

### Exports (3)

- `GoogleScholarFormWidgets`: Exported entity
- `GoogleScholarFormPartialSchema`: Exported entity
- `FormSchema`: Exported entity

### Functions (4)

- `YearPicker()`: Function definition
- `nextValue()`: Function definition
- `GoogleScholarFormWidgets()`: Function definition
- `GoogleScholarForm()`: Function definition

### Imports (21)

- `import { FormContainer } from '@/components/form-container';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { Switch } from '@/components/ui/switch';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { DatePicker, DatePickerProps } from 'antd';`
- `import dayjs from 'dayjs';`
- `import { memo, useCallback, useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 167
- Blank lines: 14 (8.4%)
- Comment lines: ~2 (1.2%)
- Code lines: ~151


## Dependencies and Imports

- `@/components/form-container`
- `@/components/originui/select-with-search`
- `@/components/top-n-item`
- `@/components/ui/switch`
- `@/hooks/common-hooks`
- `@hookform/resolvers/zod`
- `antd`
- `dayjs`
- `react`
- `react-hook-form`
- `zod`
- `../../constant`
- `../../form-hooks`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/google-scholar-form`.

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

- Other files in `web/src/pages/agent/form/google-scholar-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../form-hooks, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/originui/select-with-search, @/components/top-n-item, @/components/ui/switch, @/hooks/common-hooks, @hookform/resolvers/zod, DatePicker, DatePickerProps, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, GoogleScholarForm, GoogleScholarFormPartialSchema, GoogleScholarFormWidgets, INextOperatorForm, Number, Output, QueryVariable, SelectWithSearch, Switch, TODO, The, TopNFormField, TypeScript, YYYY, YearPicker, antd, dayjs, defaultValues, form, handleChange, hookform, nextVal, nextValue, options...

---
*Generated by RAGFlow Repository Documentation Generator*
