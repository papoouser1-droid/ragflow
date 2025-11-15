# File Documentation: web/src/pages/agent/debug-content/popover-form.tsx

## File Metadata

- **Path**: `web/src/pages/agent/debug-content/popover-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 104
- **Characters**: 2,647
- **Size**: 2,647 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Popover, PopoverContent } from '@/components/ui/popover';
import { useParseDocument } from '@/hooks/document-hooks';
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { PropsWithChildren } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';

const reg =
  /^(((ht|f)tps?):\/\/)?([^!@#$%^&*?.\s-]([^!@#$%^&*?.\s]{0,63}[^!@#$%^&*?.\s])?\.)+[a-z]{2,6}\/?/;

const FormSchema = z.object({
  url: z.string(),
  result: z.any(),
});

const values = {
  url: '',
  result: null,
};

export const PopoverForm = ({
  children,
  visible,
  switchVisible,
}: PropsWithChildren<IModalProps<any>>) => {
  const form = useForm({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });
  const { parseDocument, loading } = useParseDocument();
  const { t } = useTranslation();

  // useResetFormOnCloseModal({
  //   form,
  //   visible,
  // });

  async function onSubmit(values: z.infer<typeof FormSchema>) {
    const val = values.url;

    if (reg.test(val)) {
      const ret = await parseDocument(val);
      if (ret?.data?.code === 0) {
        form.setValue('result', ret?.data?.data);
      }
    }
  }

  const content = (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)}>
        <FormField
          control={form.control}
          name={`url`}
          render={({ field }) => (
            <FormItem className="flex-1">
              <FormControl>
                <Input
                  {...field}
                  // onPressEnter={(e) => e.preventDefault()}
                  placeholder={t('flow.pasteFileLink')}
                  // suffix={
                  //   <Button
                  //     type="primary"
                  //     onClick={onOk}
                  //     size={'small'}
                  //     loading={loading}
                  //   >
                  //     {t('common.submit')}
                  //   </Button>
                  // }
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name={`result`}
          render={() => <></>}
        />
      </form>
    </Form>
  );

  return (
    <Popover open={visible} onOpenChange={switchVisible}>
      {children}
      <PopoverContent>{content}</PopoverContent>
    </Popover>
  );
};

```

## High-Level Overview

  // useResetFormOnCloseModal({
  //   form,
  //   visible,
  // });

## Detailed Walkthrough

### Exports (1)

- `PopoverForm`: Exported entity

### Functions (2)

- `PopoverForm()`: Function definition
- `onSubmit()`: Function definition

### Imports (10)

- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Popover, PopoverContent } from '@/components/ui/popover';`
- `import { useParseDocument } from '@/hooks/document-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { PropsWithChildren } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 104
- Blank lines: 10 (9.6%)
- Comment lines: ~15 (14.4%)
- Code lines: ~79


## Dependencies and Imports

- `@/components/ui/input`
- `@/components/ui/popover`
- `@/hooks/document-hooks`
- `@/interfaces/common`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/debug-content`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/debug-content/` directory
- Potential test file: `test_popover-form.tsx`

## Keywords

@/components/ui/input, @/components/ui/popover, @/hooks/document-hooks, @/interfaces/common, @hookform/resolvers/zod, Button, Form, FormControl, FormField, FormItem, FormMessage, FormSchema, IModalProps, Input, Popover, PopoverContent, PopoverForm, PropsWithChildren, TypeScript, content, form, hookform, onSubmit, react, react-hook-form, react-i18next, reg, ret, val, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
