# File Documentation: web/src/pages/agent/form/tool-form/mcp-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/tool-form/mcp-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 104
- **Characters**: 3,342
- **Size**: 3,342 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Card, CardContent, CardHeader } from '@/components/ui/card';
import { Checkbox } from '@/components/ui/checkbox';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { useGetMcpServer } from '@/hooks/use-mcp-request';
import useGraphStore from '@/pages/agent/store';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import { MCPCard } from './mcp-card';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

const FormSchema = z.object({
  items: z.array(z.string()),
});

function MCPForm() {
  const clickedToolId = useGraphStore((state) => state.clickedToolId);
  const values = useValues();
  const form = useForm({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });
  const { data } = useGetMcpServer(clickedToolId);

  useWatchFormChange(form);

  return (
    <Form {...form}>
      <form
        className="space-y-6 p-4"
        onSubmit={(e) => {
          e.preventDefault();
        }}
      >
        <Card className="bg-background-highlight p-5">
          <CardHeader className="p-0 pb-3">
            <div>{data.name}</div>
          </CardHeader>
          <CardContent className="p-0 text-sm">
            <span className="pr-2"> URL:</span>
            <a href={data.url} className="text-accent-primary">
              {data.url}
            </a>
          </CardContent>
        </Card>
        <FormField
          control={form.control}
          name="items"
          render={() => (
            <FormItem className="space-y-2">
              {Object.entries(data.variables?.tools || {}).map(
                ([name, mcp]) => (
                  <FormField
                    key={name}
                    control={form.control}
                    name="items"
                    render={({ field }) => {
                      return (
                        <FormItem
                          key={name}
                          className="flex flex-row items-center gap-2"
                        >
                          <FormControl>
                            <MCPCard key={name} data={{ ...mcp, name }}>
                              <Checkbox
                                className="translate-y-0.5"
                                checked={field.value?.includes(name)}
                                onCheckedChange={(checked) => {
                                  return checked
                                    ? field.onChange([...field.value, name])
                                    : field.onChange(
                                        field.value?.filter(
                                          (value) => value !== name,
                                        ),
                                      );
                                }}
                              />
                            </MCPCard>
                          </FormControl>
                        </FormItem>
                      );
                    }}
                  />
                ),
              )}
              <FormMessage />
            </FormItem>
          )}
        />
      </form>
    </Form>
  );
}

export default memo(MCPForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/tool-form/mcp-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 104 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `MCPForm()`: Function definition
- `clickedToolId()`: Function definition

### Imports (12)

- `import { Card, CardContent, CardHeader } from '@/components/ui/card';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import {`
- `import { useGetMcpServer } from '@/hooks/use-mcp-request';`
- `import useGraphStore from '@/pages/agent/store';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { MCPCard } from './mcp-card';`

## Code Structure Analysis

- Total lines: 104
- Blank lines: 6 (5.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~98


## Dependencies and Imports

- `@/components/ui/card`
- `@/components/ui/checkbox`
- `@/hooks/use-mcp-request`
- `@/pages/agent/store`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `zod`
- `./mcp-card`
- `./use-values`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/tool-form/mcp-form`.

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

- Other files in `web/src/pages/agent/form/tool-form/mcp-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

./mcp-card, ./use-values, ./use-watch-change, @/components/ui/card, @/components/ui/checkbox, @/hooks/use-mcp-request, @/pages/agent/store, @hookform/resolvers/zod, Card, CardContent, CardHeader, Checkbox, Form, FormControl, FormField, FormItem, FormMessage, FormSchema, MCPCard, MCPForm, Object, TypeScript, URL, clickedToolId, form, hookform, react, react-hook-form, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
