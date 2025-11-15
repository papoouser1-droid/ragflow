# File Documentation: web/src/pages/next-chats/chat/app-settings/dynamic-variable.tsx

## File Metadata

- **Path**: `web/src/pages/next-chats/chat/app-settings/dynamic-variable.tsx`
- **Extension**: `.tsx`
- **Lines**: 95
- **Characters**: 3,054
- **Size**: 3,054 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { BlurInput } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { Switch } from '@/components/ui/switch';
import { Plus, X } from 'lucide-react';
import { useCallback } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';

export function DynamicVariableForm() {
  const { t } = useTranslation();
  const form = useFormContext();
  const name = 'prompt_config.parameters';

  const { fields, remove, append } = useFieldArray({
    name,
    control: form.control,
  });

  const add = useCallback(() => {
    append({
      key: undefined,
      optional: false,
    });
  }, [append]);

  return (
    <section className="flex flex-col gap-2">
      <div className="flex items-center justify-between">
        <FormLabel tooltip={t('chat.variableTip')}>
          {t('chat.variable')}
        </FormLabel>
        <Button variant={'ghost'} type="button" onClick={add}>
          <Plus />
        </Button>
      </div>
      <div className="flex gap-2 pr-12 text-text-secondary text-xs">
        <span className="flex-1">{t('chat.key')}</span>
        <span className="w-3"></span>
        <span className="flex-1">{t('chat.optional')}</span>
      </div>
      <div className="space-y-5">
        {fields.map((field, index) => {
          const typeField = `${name}.${index}.key`;
          return (
            <div key={field.id} className="flex w-full items-center gap-2">
              <FormField
                control={form.control}
                name={typeField}
                render={({ field }) => (
                  <FormItem className="flex-1 overflow-hidden">
                    <FormControl>
                      <BlurInput
                        {...field}
                        placeholder={t('common.pleaseInput')}
                      ></BlurInput>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <Separator className="w-3 text-text-secondary" />
              <FormField
                control={form.control}
                name={`${name}.${index}.optional`}
                render={({ field }) => (
                  <FormItem className="flex-1 overflow-hidden">
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
              <Button variant={'ghost'} onClick={() => remove(index)}>
                <X className="text-text-sub-title-invert " />
              </Button>
            </div>
          );
        })}
      </div>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/chat/app-settings/dynamic-variable.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 95 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `DynamicVariableForm`: Exported entity

### Functions (2)

- `DynamicVariableForm()`: Function definition
- `add()`: Function definition

### Imports (9)

- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { BlurInput } from '@/components/ui/input';`
- `import { Separator } from '@/components/ui/separator';`
- `import { Switch } from '@/components/ui/switch';`
- `import { Plus, X } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 95
- Blank lines: 5 (5.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~90


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/separator`
- `@/components/ui/switch`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats/chat/app-settings`.

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

- Other files in `web/src/pages/next-chats/chat/app-settings/` directory
- Potential test file: `test_dynamic-variable.tsx`

## Keywords

@/components/ui/button, @/components/ui/input, @/components/ui/separator, @/components/ui/switch, BlurInput, Button, DynamicVariableForm, FormControl, FormField, FormItem, FormLabel, FormMessage, Plus, Separator, Switch, TypeScript, add, form, lucide-react, name, react, react-hook-form, react-i18next, typeField

---
*Generated by RAGFlow Repository Documentation Generator*
