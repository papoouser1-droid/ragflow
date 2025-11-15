# File Documentation: web/src/pages/agent/form/searxng-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/searxng-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 74
- **Characters**: 2,221
- **Size**: 2,221 bytes
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
import { Input } from '@/components/ui/input';
import { useTranslate } from '@/hooks/common-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import { initialSearXNGValues } from '../../constant';
import { useFormValues } from '../../hooks/use-form-values';
import { useWatchFormChange } from '../../hooks/use-watch-form-change';
import { INextOperatorForm } from '../../interface';
import { buildOutputList } from '../../utils/build-output-list';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { QueryVariable } from '../components/query-variable';

const FormSchema = z.object({
  query: z.string(),
  searxng_url: z.string().min(1),
  top_n: z.string(),
});

const outputList = buildOutputList(initialSearXNGValues.outputs);

function SearXNGForm({ node }: INextOperatorForm) {
  const { t } = useTranslate('flow');
  const defaultValues = useFormValues(initialSearXNGValues, node);

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
          <TopNFormField></TopNFormField>
          <FormField
            control={form.control}
            name="searxng_url"
            render={({ field }) => (
              <FormItem>
                <FormLabel>SearXNG URL</FormLabel>
                <FormControl>
                  <Input {...field} placeholder="http://localhost:4000" />
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

export default memo(SearXNGForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/searxng-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 74 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `SearXNGForm()`: Function definition

### Imports (17)

- `import { FormContainer } from '@/components/form-container';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { initialSearXNGValues } from '../../constant';`

## Code Structure Analysis

- Total lines: 74
- Blank lines: 8 (10.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~66


## Dependencies and Imports

- `@/components/form-container`
- `@/components/top-n-item`
- `@/components/ui/input`
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

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/searxng-form`.

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

- Other files in `web/src/pages/agent/form/searxng-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../hooks/use-form-values, ../../hooks/use-watch-form-change, ../../interface, ../../utils/build-output-list, ../components/form-wrapper, ../components/output, ../components/query-variable, @/components/form-container, @/components/top-n-item, @/components/ui/input, @/hooks/common-hooks, @hookform/resolvers/zod, Form, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormWrapper, INextOperatorForm, Input, Output, QueryVariable, SearXNG, SearXNGForm, TopNFormField, TypeScript, URL, defaultValues, form, hookform, outputList, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
