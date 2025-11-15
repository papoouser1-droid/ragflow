# Documentation: web/src/pages/agent/form/code-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/code-form/index.tsx`
- **Size**: 5270 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/code-form/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/code-form/index.tsx` is located in the `web/src/pages/agent/form/code-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to code-form.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [next-variable.tsx](next-variable.tsx_docs.md)
- [schema.ts](schema.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
