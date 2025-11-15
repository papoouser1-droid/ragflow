# Documentation: web/src/pages/agent/form/tavily-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tavily-form/index.tsx`
- **Size**: 6820 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/tavily-form/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/tavily-form/index.tsx` is located in the `web/src/pages/agent/form/tavily-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to tavily-form.

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

- [dynamic-domain.tsx](dynamic-domain.tsx_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
