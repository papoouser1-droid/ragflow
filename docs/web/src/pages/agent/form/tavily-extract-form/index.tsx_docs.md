# Documentation: web/src/pages/agent/form/tavily-extract-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tavily-extract-form/index.tsx`
- **Size**: 3689 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/tavily-extract-form/index.tsx`.

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/tavily-extract-form/index.tsx` is located in the `web/src/pages/agent/form/tavily-extract-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to tavily-extract-form.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
