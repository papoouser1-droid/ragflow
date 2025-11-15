# File Documentation: web/src/pages/agent/form/agent-form/dynamic-prompt.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/dynamic-prompt.tsx`
- **Extension**: `.tsx`
- **Lines**: 94
- **Characters**: 2,704
- **Size**: 2,704 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { BlockButton, Button } from '@/components/ui/button';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { RAGFlowSelect } from '@/components/ui/select';
import { X } from 'lucide-react';
import { memo } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { PromptRole } from '../../constant';
import { PromptEditor } from '../components/prompt-editor';

const options = [
  { label: 'User', value: PromptRole.User },
  { label: 'Assistant', value: PromptRole.Assistant },
];

const DynamicPrompt = () => {
  const { t } = useTranslation();
  const form = useFormContext();
  const name = 'prompts';

  const { fields, append, remove } = useFieldArray({
    name: name,
    control: form.control,
  });

  return (
    <FormItem>
      <FormLabel tooltip={t('flow.msgTip')}>{t('flow.msg')}</FormLabel>
      <div className="space-y-4">
        {fields.map((field, index) => (
          <div key={field.id} className="flex">
            <div className="space-y-2 flex-1">
              <FormField
                control={form.control}
                name={`${name}.${index}.role`}
                render={({ field }) => (
                  <FormItem className="w-1/3">
                    <FormLabel />
                    <FormControl>
                      <RAGFlowSelect
                        {...field}
                        options={options}
                      ></RAGFlowSelect>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />

              <FormField
                control={form.control}
                name={`${name}.${index}.content`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormControl>
                      <section>
                        <PromptEditor
                          {...field}
                          showToolbar={false}
                        ></PromptEditor>
                      </section>
                    </FormControl>
                  </FormItem>
                )}
              />
            </div>
            <Button
              type="button"
              variant={'ghost'}
              onClick={() => remove(index)}
            >
              <X />
            </Button>
          </div>
        ))}
      </div>
      <FormMessage />
      <BlockButton
        onClick={() => append({ content: '', role: PromptRole.User })}
      >
        Add
      </BlockButton>
    </FormItem>
  );
};

export default memo(DynamicPrompt);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/dynamic-prompt.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 94 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `DynamicPrompt()`: Function definition

### Imports (9)

- `import { BlockButton, Button } from '@/components/ui/button';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { X } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { PromptRole } from '../../constant';`
- `import { PromptEditor } from '../components/prompt-editor';`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 7 (7.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~87


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/select`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../constant`
- `../components/prompt-editor`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form`.

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

- Other files in `web/src/pages/agent/form/agent-form/` directory
- Potential test file: `test_dynamic-prompt.tsx`

## Keywords

../../constant, ../components/prompt-editor, @/components/ui/button, @/components/ui/select, Add, Assistant, BlockButton, Button, DynamicPrompt, FormControl, FormField, FormItem, FormLabel, FormMessage, PromptEditor, PromptRole, RAGFlowSelect, TypeScript, User, form, lucide-react, name, options, react, react-hook-form, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
