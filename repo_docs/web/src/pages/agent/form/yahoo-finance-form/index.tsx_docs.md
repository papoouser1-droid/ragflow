# File Documentation: web/src/pages/agent/form/yahoo-finance-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/yahoo-finance-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 126
- **Characters**: 3,392
- **Size**: 3,392 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormContainer } from '@/components/form-container';
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
import { ReactNode } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialYahooFinanceValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

export const YahooFinanceFormPartialSchema = {
  info: z.boolean(),
  history: z.boolean(),
  financials: z.boolean(),
  balance_sheet: z.boolean(),
  cash_flow_statement: z.boolean(),
  news: z.boolean(),
};

const FormSchema = z.object({
  stock_code: z.string(),
  ...YahooFinanceFormPartialSchema,
});

interface SwitchFormFieldProps {
  name: string;
  label: ReactNode;
}
function SwitchFormField({ name, label }: SwitchFormFieldProps) {
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={name}
      render={({ field }) => (
        <FormItem>
          <FormLabel>{label}</FormLabel>
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
  );
}

export function YahooFinanceFormWidgets() {
  const { t } = useTranslate('flow');
  return (
    <>
      <SwitchFormField name="info" label={t('info')}></SwitchFormField>
      <SwitchFormField name="history" label={t('history')}></SwitchFormField>
      <SwitchFormField
        name="financials"
        label={t('financials')}
      ></SwitchFormField>
      <SwitchFormField
        name="balance_sheet"
        label={t('balanceSheet')}
      ></SwitchFormField>

      <SwitchFormField
        name="cash_flow_statement"
        label={t('cashFlowStatement')}
      ></SwitchFormField>

      <SwitchFormField name="news" label={t('news')}></SwitchFormField>
    </>
  );
}

const outputList = buildOutputList(initialYahooFinanceValues.outputs);

const YahooFinanceForm = ({ node }: INextOperatorForm) => {
  const { t } = useTranslate('flow');

  const defaultValues = useFormValues(initialYahooFinanceValues, node);

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <QueryVariable
            name="stock_code"
            label={t('stockCode')}
          ></QueryVariable>
        </FormContainer>
        <FormContainer>
          <YahooFinanceFormWidgets></YahooFinanceFormWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
};

export default YahooFinanceForm;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/yahoo-finance-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 126 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `YahooFinanceFormPartialSchema`: Exported entity
- `YahooFinanceFormWidgets`: Exported entity

### Functions (3)

- `SwitchFormField()`: Function definition
- `YahooFinanceFormWidgets()`: Function definition
- `YahooFinanceForm()`: Function definition

### Imports (16)

- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import { Switch } from '@/components/ui/switch';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { ReactNode } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialYahooFinanceValues } from '../../constant';`
- `import { useFormValues } from '../../hooks/use-form-values';`

## Code Structure Analysis

- Total lines: 126
- Blank lines: 15 (11.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~111


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/switch`
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

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/yahoo-finance-form`.

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

- Other files in `web/src/pages/agent/form/yahoo-finance-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/ui/switch, @/hooks/common-hooks, @hookform/resolvers/zod, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Output, QueryVariable, ReactNode, Switch, SwitchFormField, SwitchFormFieldProps, TypeScript, YahooFinanceForm, YahooFinanceFormPartialSchema, YahooFinanceFormWidgets, defaultValues, form, hookform, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
