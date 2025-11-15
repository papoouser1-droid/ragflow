# File Documentation: web/src/components/metadata-filter/metadata-filter-conditions.tsx

## File Metadata

- **Path**: `web/src/components/metadata-filter/metadata-filter-conditions.tsx`
- **Extension**: `.tsx`
- **Lines**: 150
- **Characters**: 4,916
- **Size**: 4,916 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { Button } from '@/components/ui/button';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { SwitchOperatorOptions } from '@/constants/agent';
import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';
import { useFetchKnowledgeMetadata } from '@/hooks/use-knowledge-request';
import { PromptEditor } from '@/pages/agent/form/components/prompt-editor';
import { Plus, X } from 'lucide-react';
import { useCallback } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';

export function MetadataFilterConditions({
  kbIds,
  prefix = '',
  canReference,
}: {
  kbIds: string[];
  prefix?: string;
  canReference?: boolean;
}) {
  const { t } = useTranslation();
  const form = useFormContext();
  const name = prefix + 'meta_data_filter.manual';
  const metadata = useFetchKnowledgeMetadata(kbIds);

  const switchOperatorOptions = useBuildSwitchOperatorOptions();

  const { fields, remove, append } = useFieldArray({
    name,
    control: form.control,
  });

  const add = useCallback(
    (key: string) => () => {
      append({
        key,
        value: '',
        op: SwitchOperatorOptions[0].value,
      });
    },
    [append],
  );

  return (
    <section className="flex flex-col gap-2">
      <div className="flex items-center justify-between">
        <FormLabel>{t('chat.conditions')}</FormLabel>
        <DropdownMenu>
          <DropdownMenuTrigger>
            <Button variant={'ghost'} type="button">
              <Plus />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent className="max-h-[300px] !overflow-y-auto scrollbar-auto">
            {Object.keys(metadata.data).map((key, idx) => {
              return (
                <DropdownMenuItem key={idx} onClick={add(key)}>
                  {key}
                </DropdownMenuItem>
              );
            })}
          </DropdownMenuContent>
        </DropdownMenu>
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
                      <Input
                        {...field}
                        placeholder={t('common.pleaseInput')}
                      ></Input>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <Separator className="w-3 text-text-secondary" />
              <FormField
                control={form.control}
                name={`${name}.${index}.op`}
                render={({ field }) => (
                  <FormItem className="flex-1 overflow-hidden">
                    <FormControl>
                      <SelectWithSearch
                        {...field}
                        options={switchOperatorOptions}
                      ></SelectWithSearch>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <Separator className="w-3 text-text-secondary" />
              <FormField
                control={form.control}
                name={`${name}.${index}.value`}
                render={({ field }) => (
                  <FormItem className="flex-1 overflow-hidden">
                    <FormControl>
                      {canReference ? (
                        <PromptEditor
                          {...field}
                          multiLine={false}
                          showToolbar={false}
                        ></PromptEditor>
                      ) : (
                        <Input
                          placeholder={t('common.pleaseInput')}
                          {...field}
                        />
                      )}
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

This file is part of the RAGFlow repository located at `web/src/components/metadata-filter/metadata-filter-conditions.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 150 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `MetadataFilterConditions`: Exported entity

### Functions (2)

- `MetadataFilterConditions()`: Function definition
- `add()`: Function definition

### Imports (14)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Separator } from '@/components/ui/separator';`
- `import { SwitchOperatorOptions } from '@/constants/agent';`
- `import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';`
- `import { useFetchKnowledgeMetadata } from '@/hooks/use-knowledge-request';`
- `import { PromptEditor } from '@/pages/agent/form/components/prompt-editor';`

## Code Structure Analysis

- Total lines: 150
- Blank lines: 6 (4.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~144


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/separator`
- `@/constants/agent`
- `@/hooks/logic-hooks/use-build-operator-options`
- `@/hooks/use-knowledge-request`
- `@/pages/agent/form/components/prompt-editor`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/metadata-filter`.

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

- Other files in `web/src/components/metadata-filter/` directory
- Potential test file: `test_metadata-filter-conditions.tsx`

## Keywords

@/components/originui/select-with-search, @/components/ui/button, @/components/ui/input, @/components/ui/separator, @/constants/agent, @/hooks/logic-hooks/use-build-operator-options, @/hooks/use-knowledge-request, @/pages/agent/form/components/prompt-editor, Button, DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuTrigger, FormControl, FormField, FormItem, FormLabel, FormMessage, Input, MetadataFilterConditions, Object, Plus, PromptEditor, SelectWithSearch, Separator, SwitchOperatorOptions, TypeScript, add, form, lucide-react, metadata, name, react, react-hook-form, react-i18next, switchOperatorOptions, typeField

---
*Generated by RAGFlow Repository Documentation Generator*
