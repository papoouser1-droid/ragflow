# File Documentation: web/src/pages/agent/form/duckduckgo-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/duckduckgo-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 92
- **Characters**: 2,571
- **Size**: 2,571 bytes
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
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { Channel, initialDuckValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

export const DuckDuckGoFormPartialSchema = {
  top_n: z.string(),
  channel: z.string(),
};

const FormSchema = z.object({
  query: z.string(),
  ...DuckDuckGoFormPartialSchema,
});

export function DuckDuckGoWidgets() {
  const { t } = useTranslate('flow');
  const form = useFormContext();

  const options = useMemo(() => {
    return Object.values(Channel).map((x) => ({ value: x, label: t(x) }));
  }, [t]);

  return (
    <>
      <TopNFormField></TopNFormField>
      <FormField
        control={form.control}
        name={'channel'}
        render={({ field }) => (
          <FormItem>
            <FormLabel tooltip={t('channelTip')}>{t('channel')}</FormLabel>
            <FormControl>
              <RAGFlowSelect {...field} options={options} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

const outputList = buildOutputList(initialDuckValues.outputs);

function DuckDuckGoForm({ node }: INextOperatorForm) {
  const defaultValues = useFormValues(initialDuckValues, node);

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
          <DuckDuckGoWidgets></DuckDuckGoWidgets>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(DuckDuckGoForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/duckduckgo-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 92 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `DuckDuckGoFormPartialSchema`: Exported entity
- `DuckDuckGoWidgets`: Exported entity

### Functions (3)

- `DuckDuckGoWidgets()`: Function definition
- `options()`: Function definition
- `DuckDuckGoForm()`: Function definition

### Imports (17)

- `import { FormContainer } from '@/components/form-container';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo, useMemo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { Channel, initialDuckValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 92
- Blank lines: 12 (13.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~80


## Dependencies and Imports

- `@/components/form-container`
- `@/components/top-n-item`
- `@/components/ui/select`
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

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/duckduckgo-form`.

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

- Other files in `web/src/pages/agent/form/duckduckgo-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/top-n-item, @/components/ui/select, @/hooks/common-hooks, @hookform/resolvers/zod, Channel, DuckDuckGoForm, DuckDuckGoFormPartialSchema, DuckDuckGoWidgets, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Object, Output, QueryVariable, RAGFlowSelect, TopNFormField, TypeScript, defaultValues, form, hookform, options, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
