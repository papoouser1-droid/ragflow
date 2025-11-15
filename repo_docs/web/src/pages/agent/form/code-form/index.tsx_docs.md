# File Documentation: web/src/pages/agent/form/code-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/code-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 169
- **Characters**: 5,270
- **Size**: 5,270 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import Editor, { loader } from '@monaco-editor/react';
import { INextOperatorForm } from '../../interface';

import { FormContainer } from '@/components/form-container';
import { useIsDarkTheme } from '@/components/theme-provider';
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
import { ProgrammingLanguage } from '@/constants/agent';
import { ICodeForm } from '@/interfaces/database/agent';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import {
  DynamicInputVariable,
  TypeOptions,
  VariableTitle,
} from './next-variable';
import { FormSchema, FormSchemaType } from './schema';
import { useValues } from './use-values';
import {
  useHandleLanguageChange,
  useWatchFormChange,
} from './use-watch-change';

loader.config({ paths: { vs: '/vs' } });

const options = [
  ProgrammingLanguage.Python,
  ProgrammingLanguage.Javascript,
].map((x) => ({ value: x, label: x }));

const DynamicFieldName = 'outputs';

function CodeForm({ node }: INextOperatorForm) {
  const formData = node?.data.form as ICodeForm;
  const { t } = useTranslation();
  const values = useValues(node);
  const isDarkTheme = useIsDarkTheme();

  const form = useForm<FormSchemaType>({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  const handleLanguageChange = useHandleLanguageChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <DynamicInputVariable
          node={node}
          title={t('flow.input')}
          isOutputs={false}
        ></DynamicInputVariable>
        <FormField
          control={form.control}
          name="script"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="flex items-center justify-between">
                Code
                <FormField
                  control={form.control}
                  name="lang"
                  render={({ field }) => (
                    <FormItem>
                      <FormControl>
                        <RAGFlowSelect
                          {...field}
                          onChange={(val) => {
                            field.onChange(val);
                            handleLanguageChange(val);
                          }}
                          options={options}
                        />
                      </FormControl>
                      <FormMessage />
                    </FormItem>
                  )}
                />
              </FormLabel>
              <FormControl>
                <Editor
                  height={300}
                  theme={isDarkTheme ? 'vs-dark' : 'vs'}
                  language={formData.lang}
                  options={{
                    minimap: { enabled: false },
                    automaticLayout: true,
                  }}
                  {...field}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {formData.lang === ProgrammingLanguage.Python ? (
          <DynamicInputVariable
            node={node}
            title={'Return Values'}
            name={DynamicFieldName}
            isOutputs
          ></DynamicInputVariable>
        ) : (
          <div>
            <VariableTitle title={'Return Values'}></VariableTitle>
            <FormContainer className="space-y-5">
              <FormField
                control={form.control}
                name={`${DynamicFieldName}.name`}
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Name</FormLabel>
                    <FormControl>
                      <Input
                        {...field}
                        placeholder={t('common.pleaseInput')}
                      ></Input>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name={`${DynamicFieldName}.type`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormLabel>Type</FormLabel>
                    <FormControl>
                      <RAGFlowSelect
                        placeholder={t('common.pleaseSelect')}
                        options={TypeOptions}
                        {...field}
                      ></RAGFlowSelect>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
            </FormContainer>
          </div>
        )}
      </FormWrapper>
      <div className="p-5">
        <Output list={buildOutputList(formData.outputs)}></Output>
      </div>
    </Form>
  );
}

export default memo(CodeForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/code-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 169 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `options()`: Function definition
- `CodeForm()`: Function definition

### Imports (20)

- `import Editor, { loader } from '@monaco-editor/react';`
- `import { INextOperatorForm } from '../../interface';`
- `import { FormContainer } from '@/components/form-container';`
- `import { useIsDarkTheme } from '@/components/theme-provider';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { ProgrammingLanguage } from '@/constants/agent';`
- `import { ICodeForm } from '@/interfaces/database/agent';`
- `import { zodResolver } from '@hookform/resolvers/zod';`

## Code Structure Analysis

- Total lines: 169
- Blank lines: 12 (7.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~157


## Dependencies and Imports

- `@monaco-editor/react`
- `../../interface`
- `@/components/form-container`
- `@/components/theme-provider`
- `@/components/ui/input`
- `@/components/ui/select`
- `@/constants/agent`
- `@/interfaces/database/agent`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../utils/build-output-list`
- `../components/form-wrapper`
- `../components/output`
- `./schema`
- `./use-values`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/code-form`.

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

- Other files in `web/src/pages/agent/form/code-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ./schema, ./use-values, @/components/form-container, @/components/theme-provider, @/components/ui/input, @/components/ui/select, @/constants/agent, @/interfaces/database/agent, @hookform/resolvers/zod, @monaco-editor/react, Code, CodeForm, DynamicFieldName, DynamicInputVariable, Editor, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormSchemaType, FormWrapper, ICodeForm, INextOperatorForm, Input, Javascript, Name, Output, ProgrammingLanguage, Python, RAGFlowSelect, Return, Type, TypeOptions, TypeScript, Values, VariableTitle, form, formData, handleLanguageChange, hookform, isDarkTheme, monaco...

---
*Generated by RAGFlow Repository Documentation Generator*
