# File Documentation: web/src/pages/agent/form/string-transform-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/string-transform-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 167
- **Characters**: 5,186
- **Size**: 5,186 bytes
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
import { MultiSelect } from '@/components/ui/multi-select';
import { RAGFlowSelect } from '@/components/ui/select';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { toLower } from 'lodash';
import { memo, useCallback, useMemo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { z } from 'zod';
import {
  StringTransformDelimiter,
  StringTransformMethod,
  initialStringTransformValues,
} from '../../constant';
import { INextOperatorForm } from '../../interface';
import { FormWrapper } from '../components/form-wrapper';
import { Output, transferOutputs } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { QueryVariable } from '../components/query-variable';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-form-change';

const DelimiterOptions = Object.entries(StringTransformDelimiter).map(
  ([key, val]) => ({ label: t('flow.' + toLower(key)), value: val }),
);

function StringTransformForm({ node }: INextOperatorForm) {
  const values = useValues(node);

  const FormSchema = z.object({
    method: z.string(),
    split_ref: z.string().optional(),
    script: z.string().optional(),
    delimiters: z.array(z.string()).or(z.string()),
    outputs: z.object({ result: z.object({ type: z.string() }) }).optional(),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });

  const method = useWatch({ control: form.control, name: 'method' });

  const isSplit = method === StringTransformMethod.Split;

  const outputList = useMemo(() => {
    return transferOutputs(values.outputs);
  }, [values.outputs]);

  const handleMethodChange = useCallback(
    (value: StringTransformMethod) => {
      const isMerge = value === StringTransformMethod.Merge;
      const outputs = {
        ...initialStringTransformValues.outputs,
        result: {
          type: isMerge ? 'string' : 'Array<string>',
        },
      };
      form.setValue('outputs', outputs);
      form.setValue(
        'delimiters',
        isMerge ? StringTransformDelimiter.Comma : [],
      );
    },
    [form],
  );

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <FormField
            control={form.control}
            name="method"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.method')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    {...field}
                    options={Object.values(StringTransformMethod).map(
                      (val) => ({ label: t('flow.' + val), value: val }),
                    )}
                    onChange={(value) => {
                      handleMethodChange(value);
                      field.onChange(value);
                    }}
                  ></RAGFlowSelect>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          {isSplit && (
            <QueryVariable
              label={<FormLabel>split_ref</FormLabel>}
              name="split_ref"
            ></QueryVariable>
          )}
          {isSplit || (
            <FormField
              control={form.control}
              name="script"
              render={({ field }) => (
                <FormItem>
                  <FormLabel>{t('flow.script')}</FormLabel>
                  <FormControl>
                    <PromptEditor {...field} showToolbar={false}></PromptEditor>
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          )}
          <FormField
            control={form.control}
            name="delimiters"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.delimiters')}</FormLabel>
                <FormControl>
                  {isSplit ? (
                    <MultiSelect
                      options={DelimiterOptions}
                      onValueChange={field.onChange}
                      defaultValue={field.value as string[]}
                      variant="inverted"
                      // {...field}
                    />
                  ) : (
                    <RAGFlowSelect
                      {...field}
                      options={DelimiterOptions}
                    ></RAGFlowSelect>
                  )}
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="outputs"
            render={() => <div></div>}
          />
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(StringTransformForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/string-transform-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 167 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `DelimiterOptions()`: Function definition
- `StringTransformForm()`: Function definition
- `outputList()`: Function definition
- `handleMethodChange()`: Function definition

### Imports (18)

- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import { MultiSelect } from '@/components/ui/multi-select';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { t } from 'i18next';`
- `import { toLower } from 'lodash';`
- `import { memo, useCallback, useMemo } from 'react';`
- `import { useForm, useWatch } from 'react-hook-form';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 167
- Blank lines: 12 (7.2%)
- Comment lines: ~1 (0.6%)
- Code lines: ~154


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/multi-select`
- `@/components/ui/select`
- `@hookform/resolvers/zod`
- `i18next`
- `lodash`
- `react`
- `react-hook-form`
- `zod`
- `../../interface`
- `../components/form-wrapper`
- `../components/output`
- `../components/prompt-editor`
- `../components/query-variable`
- `./use-values`
- `./use-watch-form-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/string-transform-form`.

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

- Other files in `web/src/pages/agent/form/string-transform-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ../components/query-variable, ./use-values, ./use-watch-form-change, @/components/form-container, @/components/ui/multi-select, @/components/ui/select, @hookform/resolvers/zod, Array, Comma, DelimiterOptions, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Merge, MultiSelect, Object, Output, PromptEditor, QueryVariable, RAGFlowSelect, Split, StringTransformDelimiter, StringTransformForm, StringTransformMethod, TypeScript, form, handleMethodChange, hookform, i18next, isMerge, isSplit, lodash, method, outputList, outputs, react, react-hook-form, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
