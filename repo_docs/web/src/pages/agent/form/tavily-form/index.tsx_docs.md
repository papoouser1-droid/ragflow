# File Documentation: web/src/pages/agent/form/tavily-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tavily-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 215
- **Characters**: 6,820
- **Size**: 6,820 bytes
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
import { Input } from '@/components/ui/input';
import { RAGFlowSelect } from '@/components/ui/select';
import { Switch } from '@/components/ui/switch';
import { buildOptions } from '@/utils/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import {
  TavilySearchDepth,
  TavilyTopic,
  initialTavilyValues,
} from '../../constant';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { ApiKeyField } from '../components/api-key-field';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';
import { DynamicDomain } from './dynamic-domain';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

export const TavilyFormSchema = {
  api_key: z.string(),
};

const outputList = buildOutputList(initialTavilyValues.outputs);

function TavilyForm({ node }: INextOperatorForm) {
  const values = useValues(node);

  const FormSchema = z.object({
    ...TavilyFormSchema,
    query: z.string(),
    search_depth: z.enum([TavilySearchDepth.Advanced, TavilySearchDepth.Basic]),
    topic: z.enum([TavilyTopic.News, TavilyTopic.General]),
    max_results: z.coerce.number(),
    days: z.coerce.number(),
    include_answer: z.boolean(),
    include_raw_content: z.boolean(),
    include_images: z.boolean(),
    include_image_descriptions: z.boolean(),
    include_domains: z.array(z.object({ value: z.any() })), // TODO: z.string should be used, but an error will be reported
    exclude_domains: z.array(z.object({ value: z.any() })),
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
          <QueryVariable></QueryVariable>
          <FormField
            control={form.control}
            name="search_depth"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.searchDepth')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    placeholder="shadcn"
                    {...field}
                    options={buildOptions(TavilySearchDepth, t, 'flow')}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="topic"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.tavilyTopic')}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    placeholder="shadcn"
                    {...field}
                    options={buildOptions(TavilyTopic, t, 'flow')}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="max_results"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.maxResults')}</FormLabel>
                <FormControl>
                  <Input type={'number'} {...field}></Input>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="days"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.days')}</FormLabel>
                <FormControl>
                  <Input type={'number'} {...field}></Input>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="include_answer"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.includeAnswer')}</FormLabel>
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
          <FormField
            control={form.control}
            name="include_raw_content"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.includeRawContent')}</FormLabel>
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
          <FormField
            control={form.control}
            name="include_images"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.includeImages')}</FormLabel>
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
          <FormField
            control={form.control}
            name="include_image_descriptions"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('flow.includeImageDescriptions')}</FormLabel>
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
          <DynamicDomain
            name="include_domains"
            label={t('flow.includeDomains')}
          ></DynamicDomain>
          <DynamicDomain
            name="exclude_domains"
            label={t('flow.ExcludeDomains')}
          ></DynamicDomain>
        </FormContainer>
      </FormWrapper>
      <div className="p-5">
        <Output list={outputList}></Output>
      </div>
    </Form>
  );
}

export default memo(TavilyForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/tavily-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 215 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `TavilyFormSchema`: Exported entity

### Functions (1)

- `TavilyForm()`: Function definition

### Imports (21)

- `import { FormContainer } from '@/components/form-container';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { Switch } from '@/components/ui/switch';`
- `import { buildOptions } from '@/utils/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { t } from 'i18next';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 215
- Blank lines: 9 (4.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~206


## Dependencies and Imports

- `@/components/form-container`
- `@/components/ui/input`
- `@/components/ui/select`
- `@/components/ui/switch`
- `@/utils/form`
- `@hookform/resolvers/zod`
- `i18next`
- `react`
- `react-hook-form`
- `zod`
- `../../interface`
- `../../utils/build-output-list`
- `../components/api-key-field`
- `../components/form-wrapper`
- `../components/output`
- `../components/query-variable`
- `./dynamic-domain`
- `./use-values`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/tavily-form`.

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

- Other files in `web/src/pages/agent/form/tavily-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../../utils/build-output-list, ../components/api-key-field, ../components/form-wrapper, ../components/output, ../components/query-variable, ./dynamic-domain, ./use-values, ./use-watch-change, @/components/form-container, @/components/ui/input, @/components/ui/select, @/components/ui/switch, @/utils/form, @hookform/resolvers/zod, Advanced, ApiKeyField, Basic, DynamicDomain, ExcludeDomains, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, General, INextOperatorForm, Input, News, Output, QueryVariable, RAGFlowSelect, Switch, TODO, TavilyForm, TavilyFormSchema, TavilySearchDepth, TavilyTopic, TypeScript, form, hookform, i18next, outputList, react, react-hook-form, values...

---
*Generated by RAGFlow Repository Documentation Generator*
