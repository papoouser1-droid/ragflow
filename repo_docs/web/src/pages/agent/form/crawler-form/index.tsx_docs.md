# File Documentation: web/src/pages/agent/form/crawler-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/crawler-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 106
- **Characters**: 2,741
- **Size**: 2,741 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useMemo } from 'react';
import { useForm, useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { initialCrawlerValues } from '../../constant';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { CrawlerResultOptions } from '../../options';
import { QueryVariable } from '../components/query-variable';

export function CrawlerProxyFormField() {
  const { t } = useTranslate('flow');
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name="proxy"
      render={({ field }) => (
        <FormItem>
          <FormLabel>{t('proxy')}</FormLabel>
          <FormControl>
            <Input placeholder="like: http://127.0.0.1:8888" {...field} />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}

export function CrawlerExtractTypeFormField() {
  const { t } = useTranslate('flow');
  const form = useFormContext();
  const crawlerResultOptions = useMemo(() => {
    return CrawlerResultOptions.map((x) => ({
      value: x,
      label: t(`crawlerResultOptions.${x}`),
    }));
  }, [t]);

  return (
    <FormField
      control={form.control}
      name="extract_type"
      render={({ field }) => (
        <FormItem>
          <FormLabel>{t('extractType')}</FormLabel>
          <FormControl>
            <SelectWithSearch {...field} options={crawlerResultOptions} />
          </FormControl>
          <FormMessage />
        </FormItem>
      )}
    />
  );
}

export const CrawlerFormSchema = {
  proxy: z.string().url(),
  extract_type: z.string(),
};

const FormSchema = z.object({
  query: z.string().optional(),
  ...CrawlerFormSchema,
});

function CrawlerForm({ node }: INextOperatorForm) {
  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: initialCrawlerValues,
    mode: 'onChange',
  });

  useWatchFormChange(node?.id, form);

  return (
    <Form {...form}>
      <form
        className="space-y-6 p-4"
        onSubmit={(e) => {
          e.preventDefault();
        }}
      >
        <QueryVariable></QueryVariable>
        <CrawlerProxyFormField></CrawlerProxyFormField>
        <CrawlerExtractTypeFormField></CrawlerExtractTypeFormField>
      </form>
    </Form>
  );
}

export default memo(CrawlerForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/crawler-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 106 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `CrawlerProxyFormField`: Exported entity
- `CrawlerExtractTypeFormField`: Exported entity
- `CrawlerFormSchema`: Exported entity

### Functions (4)

- `CrawlerProxyFormField()`: Function definition
- `CrawlerExtractTypeFormField()`: Function definition
- `crawlerResultOptions()`: Function definition
- `CrawlerForm()`: Function definition

### Imports (13)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo, useMemo } from 'react';`
- `import { useForm, useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialCrawlerValues } from '../../constant';`
- `import { useWatchFormChange } from '../../hooks/use-watch-form-change';`

## Code Structure Analysis

- Total lines: 106
- Blank lines: 11 (10.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~95


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ui/input`
- `@/hooks/common-hooks`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `zod`
- `../../constant`
- `../../hooks/use-watch-form-change`
- `../../interface`
- `../../options`
- `../components/query-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/crawler-form`.

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

- Other files in `web/src/pages/agent/form/crawler-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-watch-form-change, ../../interface, ../../options, ../components/query-variable, @/components/originui/select-with-search, @/components/ui/input, @/hooks/common-hooks, @hookform/resolvers/zod, CrawlerExtractTypeFormField, CrawlerForm, CrawlerFormSchema, CrawlerProxyFormField, CrawlerResultOptions, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, INextOperatorForm, Input, QueryVariable, SelectWithSearch, TypeScript, crawlerResultOptions, form, hookform, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
