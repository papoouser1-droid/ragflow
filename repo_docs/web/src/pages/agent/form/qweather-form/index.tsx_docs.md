# File Documentation: web/src/pages/agent/form/qweather-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/qweather-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 158
- **Characters**: 4,361
- **Size**: 4,361 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { RAGFlowSelect } from '@/components/ui/select';
import { useCallback, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { INextOperatorForm } from '../../interface';
import {
  QWeatherLangOptions,
  QWeatherTimePeriodOptions,
  QWeatherTypeOptions,
  QWeatherUserTypeOptions,
} from '../../options';
import { DynamicInputVariable } from '../components/next-dynamic-input-variable';

enum FormFieldName {
  Type = 'type',
  UserType = 'user_type',
}

const QWeatherForm = ({ form, node }: INextOperatorForm) => {
  const { t } = useTranslation();
  const typeValue = form.watch(FormFieldName.Type);

  const qWeatherLangOptions = useMemo(() => {
    return QWeatherLangOptions.map((x) => ({
      value: x,
      label: t(`flow.qWeatherLangOptions.${x}`),
    }));
  }, [t]);

  const qWeatherTypeOptions = useMemo(() => {
    return QWeatherTypeOptions.map((x) => ({
      value: x,
      label: t(`flow.qWeatherTypeOptions.${x}`),
    }));
  }, [t]);

  const qWeatherUserTypeOptions = useMemo(() => {
    return QWeatherUserTypeOptions.map((x) => ({
      value: x,
      label: t(`flow.qWeatherUserTypeOptions.${x}`),
    }));
  }, [t]);

  const getQWeatherTimePeriodOptions = useCallback(() => {
    let options = QWeatherTimePeriodOptions;
    const userType = form.getValues(FormFieldName.UserType);
    if (userType === 'free') {
      options = options.slice(0, 3);
    }
    return options.map((x) => ({
      value: x,
      label: t(`flow.qWeatherTimePeriodOptions.${x}`),
    }));
  }, [form, t]);

  return (
    <Form {...form}>
      <form
        className="space-y-6"
        onSubmit={(e) => {
          e.preventDefault();
        }}
      >
        <DynamicInputVariable node={node}></DynamicInputVariable>
        <FormField
          control={form.control}
          name="web_apikey"
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.webApiKey')}</FormLabel>
              <FormControl>
                <Input {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="lang"
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.lang')}</FormLabel>
              <FormControl>
                <RAGFlowSelect
                  {...field}
                  options={qWeatherLangOptions}
                ></RAGFlowSelect>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name={FormFieldName.Type}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.type')}</FormLabel>
              <FormControl>
                <RAGFlowSelect
                  {...field}
                  options={qWeatherTypeOptions}
                ></RAGFlowSelect>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name={FormFieldName.UserType}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('flow.userType')}</FormLabel>
              <FormControl>
                <RAGFlowSelect
                  {...field}
                  options={qWeatherUserTypeOptions}
                ></RAGFlowSelect>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        {typeValue === 'weather' && (
          <FormField
            control={form.control}
            name={'time_period'}
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.timePeriod')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    {...field}
                    options={getQWeatherTimePeriodOptions()}
                  ></RAGFlowSelect>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        )}
      </form>
    </Form>
  );
};

export default QWeatherForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/qweather-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 158 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `QWeatherForm()`: Function definition
- `qWeatherLangOptions()`: Function definition
- `qWeatherTypeOptions()`: Function definition
- `qWeatherUserTypeOptions()`: Function definition
- `getQWeatherTimePeriodOptions()`: Function definition

### Imports (8)

- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { useCallback, useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { INextOperatorForm } from '../../interface';`
- `import {`
- `import { DynamicInputVariable } from '../components/next-dynamic-input-variable';`

## Code Structure Analysis

- Total lines: 158
- Blank lines: 9 (5.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~149


## Dependencies and Imports

- `@/components/ui/input`
- `@/components/ui/select`
- `react`
- `react-i18next`
- `../../interface`
- `../components/next-dynamic-input-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/qweather-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/qweather-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../components/next-dynamic-input-variable, @/components/ui/input, @/components/ui/select, DynamicInputVariable, Form, FormControl, FormField, FormFieldName, FormItem, FormLabel, FormMessage, INextOperatorForm, Input, QWeatherForm, QWeatherLangOptions, QWeatherTimePeriodOptions, QWeatherTypeOptions, QWeatherUserTypeOptions, RAGFlowSelect, Type, TypeScript, UserType, getQWeatherTimePeriodOptions, options, qWeatherLangOptions, qWeatherTypeOptions, qWeatherUserTypeOptions, react, react-i18next, typeValue, userType

---
*Generated by RAGFlow Repository Documentation Generator*
