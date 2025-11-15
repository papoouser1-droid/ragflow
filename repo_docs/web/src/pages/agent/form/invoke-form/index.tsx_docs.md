# File Documentation: web/src/pages/agent/form/invoke-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 237
- **Characters**: 7,006
- **Size**: 7,006 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Collapse } from '@/components/collapse';
import { FormContainer } from '@/components/form-container';
import NumberInput from '@/components/originui/number-input';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { useIsDarkTheme } from '@/components/theme-provider';
import { Button } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Switch } from '@/components/ui/switch';
import { zodResolver } from '@hookform/resolvers/zod';
import Editor, { loader } from '@monaco-editor/react';
import { Plus } from 'lucide-react';
import { memo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { initialInvokeValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { FormSchema, FormSchemaType } from './schema';
import { useEditVariableRecord } from './use-edit-variable';
import { VariableDialog } from './variable-dialog';
import { VariableTable } from './variable-table';

loader.config({ paths: { vs: '/vs' } });

enum Method {
  GET = 'GET',
  POST = 'POST',
  PUT = 'PUT',
}

const MethodOptions = [Method.GET, Method.POST, Method.PUT].map((x) => ({
  label: x,
  value: x,
}));

interface TimeoutInputProps {
  value?: number;
  onChange?: (value: number | null) => void;
}

const TimeoutInput = ({ value, onChange }: TimeoutInputProps) => {
  const { t } = useTranslation();
  return (
    <div className="flex gap-2 items-center">
      <NumberInput value={value} onChange={onChange} /> {t('flow.seconds')}
    </div>
  );
};

const outputList = buildOutputList(initialInvokeValues.outputs);

function InvokeForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();
  const defaultValues = useFormValues(initialInvokeValues, node);

  const form = useForm<FormSchemaType>({
    defaultValues,
    resolver: zodResolver(FormSchema),
    mode: 'onChange',
  });

  const {
    visible,
    hideModal,
    showModal,
    ok,
    currentRecord,
    otherThanCurrentQuery,
    handleDeleteRecord,
  } = useEditVariableRecord({
    form,
    node,
  });

  const variables = useWatch({ control: form.control, name: 'variables' });

  const isDarkTheme = useIsDarkTheme();

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <FormField
            control={form.control}
            name="url"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.url')}</FormLabel>
                <FormControl>
                  <PromptEditor
                    value={field.value}
                    onChange={field.onChange}
                    placeholder="http://"
                    showToolbar={false}
                    multiLine={false}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="method"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.method')}</FormLabel>
                <FormControl>
                  <SelectWithSearch {...field} options={MethodOptions} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="timeout"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.timeout')}</FormLabel>
                <FormControl>
                  <TimeoutInput {...field} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="headers"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.headers')}</FormLabel>
                <FormControl>
                  <Editor
                    height={200}
                    defaultLanguage="json"
                    theme={isDarkTheme ? 'vs-dark' : undefined}
                    {...field}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="proxy"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.proxy')}</FormLabel>
                <FormControl>
                  <Input {...field} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="clean_html"
            render={({ field }) => (
              <FormItem>
                <FormLabel tooltip={t('flow.cleanHtmlTip')}>
                  {t('flow.cleanHtml')}
                </FormLabel>
                <FormControl>
                  <Switch
                    onCheckedChange={field.onChange}
                    checked={field.value}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          {/* Create a hidden field to make Form instance record this */}
          <FormField
            control={form.control}
            name={'variables'}
            render={() => <div></div>}
          />
        </FormContainer>
        <Collapse
          title={<div>{t('flow.parameter')}</div>}
          rightContent={
            <Button
              variant={'ghost'}
              onClick={(e) => {
                e.preventDefault();
                showModal();
              }}
            >
              <Plus />
            </Button>
          }
        >
          <VariableTable
            data={variables}
            showModal={showModal}
            deleteRecord={handleDeleteRecord}
            nodeId={node?.id}
          ></VariableTable>
        </Collapse>
        {visible && (
          <VariableDialog
            hideModal={hideModal}
            initialValue={currentRecord}
            otherThanCurrentQuery={otherThanCurrentQuery}
            submit={ok}
          ></VariableDialog>
        )}
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(InvokeForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/invoke-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 237 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `MethodOptions()`: Function definition
- `TimeoutInput()`: Function definition
- `InvokeForm()`: Function definition

### Imports (27)

- `import { Collapse } from '@/components/collapse';`
- `import { FormContainer } from '@/components/form-container';`
- `import NumberInput from '@/components/originui/number-input';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { useIsDarkTheme } from '@/components/theme-provider';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Switch } from '@/components/ui/switch';`
- `import { zodResolver } from '@hookform/resolvers/zod';`

## Code Structure Analysis

- Total lines: 237
- Blank lines: 15 (6.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~222


## Dependencies and Imports

- `@/components/collapse`
- `@/components/form-container`
- `@/components/originui/number-input`
- `@/components/originui/select-with-search`
- `@/components/theme-provider`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/switch`
- `@hookform/resolvers/zod`
- `@monaco-editor/react`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../constant`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/form-wrapper`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/invoke-form`.

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

- Other files in `web/src/pages/agent/form/invoke-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ./schema, ./use-edit-variable, ./variable-dialog, ./variable-table, @/components/collapse, @/components/form-container, @/components/originui/number-input, @/components/originui/select-with-search, @/components/theme-provider, @/components/ui/button, @/components/ui/input, @/components/ui/switch, @hookform/resolvers/zod, @monaco-editor/react, Button, Collapse, Create, Editor, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormSchemaType, FormWrapper, GET, INextOperatorForm, Input, InvokeForm, Method, MethodOptions, NumberInput, Output, POST, PUT, Plus, PromptEditor, SelectWithSearch, Switch...

---
*Generated by RAGFlow Repository Documentation Generator*
