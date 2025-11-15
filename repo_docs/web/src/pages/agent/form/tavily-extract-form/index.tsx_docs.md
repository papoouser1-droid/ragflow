# File Documentation: web/src/pages/agent/form/tavily-extract-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tavily-extract-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 122
- **Characters**: 3,689
- **Size**: 3,689 bytes
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
import { RAGFlowSelect } from '@/components/ui/select';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import {
  TavilyExtractDepth,
  TavilyExtractFormat,
  initialTavilyExtractValues,
} from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { ApiKeyField } from '../components/api-key-field';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { TavilyFormSchema } from '../tavily-form';

const outputList = buildOutputList(initialTavilyExtractValues.outputs);

function TavilyExtractForm({ node }: INextOperatorForm) {
  const values = useFormValues(initialTavilyExtractValues, node);

  const FormSchema = z.object({
    ...TavilyFormSchema,
    urls: z.string(),
    extract_depth: z.enum([
      TavilyExtractDepth.Advanced,
      TavilyExtractDepth.Basic,
    ]),
    format: z.enum([TavilyExtractFormat.Text, TavilyExtractFormat.Markdown]),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <FormWrapper>
        <FormContainer>
          <ApiKeyField></ApiKeyField>
        </FormContainer>
        <FormContainer>
          <FormField
            control={form.control}
            name="urls"
            render={({ field }) => (
              <FormItem>
                <FormLabel>URL</FormLabel>
                <FormControl>
                  <PromptEditor
                    {...field}
                    multiLine={false}
                    showToolbar={false}
                  ></PromptEditor>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="extract_depth"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.extractDepth')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    placeholder="shadcn"
                    {...field}
                    options={buildOptions(TavilyExtractDepth, t, 'flow')}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="format"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.format')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    placeholder="shadcn"
                    {...field}
                    options={buildOptions(TavilyExtractFormat)}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(TavilyExtractForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/tavily-extract-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 122 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `TavilyExtractForm()`: Function definition

### Imports (19)

- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { buildOptions } from '@/utils/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { t } from 'i18next';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import {`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 8 (6.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~114


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/select`
- `@/utils/form`
- `@hookform/resolvers/zod`
- `i18next`
- `react`
- `react-hook-form`
- `zod`
- `../../hooks/use-form-values`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../utils/build-output-list`
- `../components/api-key-field`
- `../components/form-wrapper`
- `../components/output`
- `../components/prompt-editor`
- `../tavily-form`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/tavily-extract-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/tavily-extract-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/api-key-field, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ../tavily-form, @/components/form-container, @/components/ui/select, @/utils/form, @hookform/resolvers/zod, Advanced, ApiKeyField, Basic, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Markdown, Output, PromptEditor, RAGFlowSelect, TavilyExtractDepth, TavilyExtractForm, TavilyExtractFormat, TavilyFormSchema, Text, TypeScript, URL, form, hookform, i18next, outputList, react, react-hook-form, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
