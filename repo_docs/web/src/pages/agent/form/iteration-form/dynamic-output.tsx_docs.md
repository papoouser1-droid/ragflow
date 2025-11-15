# File Documentation: web/src/pages/agent/form/iteration-form/dynamic-output.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/iteration-form/dynamic-output.tsx`
- **Extension**: `.tsx`
- **Lines**: 129
- **Characters**: 3,921
- **Size**: 3,921 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import { FormContainer } from '@/components/form-container';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { BlockButton, Button } from '@/components/ui/button';
import {
  FormControl,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { t } from 'i18next';
import { X } from 'lucide-react';
import { ReactNode, useCallback, useMemo } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { useBuildSubNodeOutputOptions } from './use-build-options';

interface IProps {
  node?: RAGFlowNodeType;
}

export function DynamicOutputForm({ node }: IProps) {
  const { t } = useTranslation();
  const form = useFormContext();
  const options = useBuildSubNodeOutputOptions(node?.id);
  const name = 'outputs';

  const flatOptions = useMemo(() => {
    return options.reduce<{ label: string; value: string; type: string }[]>(
      (pre, cur) => {
        pre.push(...cur.options);
        return pre;
      },
      [],
    );
  }, [options]);

  const findType = useCallback(
    (val: string) => {
      const type = flatOptions.find((x) => x.value === val)?.type;
      if (type) {
        return `Array<${type}>`;
      }
    },
    [flatOptions],
  );

  const { fields, remove, append } = useFieldArray({
    name: name,
    control: form.control,
  });

  return (
    <div className="space-y-5">
      {fields.map((field, index) => {
        const nameField = `${name}.${index}.name`;
        const typeField = `${name}.${index}.type`;
        return (
          <div key={field.id} className="flex items-center gap-2">
            <FormField
              control={form.control}
              name={nameField}
              render={({ field }) => (
                <FormItem className="flex-1">
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
              name={`${name}.${index}.ref`}
              render={({ field }) => (
                <FormItem className="w-2/5">
                  <FormControl>
                    <SelectWithSearch
                      options={options}
                      {...field}
                      onChange={(val) => {
                        form.setValue(typeField, findType(val));
                        field.onChange(val);
                      }}
                    ></SelectWithSearch>
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
            <FormField
              control={form.control}
              name={typeField}
              render={() => <div></div>}
            />
            <Button variant={'ghost'} onClick={() => remove(index)}>
              <X className="text-text-sub-title-invert " />
            </Button>
          </div>
        );
      })}
      <BlockButton onClick={() => append({ name: '', ref: undefined })}>
        {t('common.add')}
      </BlockButton>
    </div>
  );
}

export function VariableTitle({ title }: { title: ReactNode }) {
  return <div className="font-medium text-text-primary pb-2">{title}</div>;
}

export function DynamicOutput({ node }: IProps) {
  return (
    <FormContainer>
      <VariableTitle title={t('flow.output')}></VariableTitle>
      <DynamicOutputForm node={node}></DynamicOutputForm>
    </FormContainer>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/iteration-form/dynamic-output.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 129 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `DynamicOutputForm`: Exported entity
- `VariableTitle`: Exported entity
- `DynamicOutput`: Exported entity

### Functions (6)

- `DynamicOutputForm()`: Function definition
- `flatOptions()`: Function definition
- `findType()`: Function definition
- `type()`: Function definition
- `VariableTitle()`: Function definition
- `DynamicOutput()`: Function definition

### Imports (13)

- `import { FormContainer } from '@/components/form-container';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Separator } from '@/components/ui/separator';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { t } from 'i18next';`
- `import { X } from 'lucide-react';`
- `import { ReactNode, useCallback, useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 129
- Blank lines: 10 (7.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~119


## Dependencies and Imports

- `@/components/form-container`
- `@/components/originui/select-with-search`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/separator`
- `@/interfaces/database/flow`
- `i18next`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `./use-build-options`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/iteration-form`.

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

- Other files in `web/src/pages/agent/form/iteration-form/` directory
- Potential test file: `test_dynamic-output.tsx`

## Keywords

./use-build-options, @/components/form-container, @/components/originui/select-with-search, @/components/ui/button, @/components/ui/input, @/components/ui/separator, @/interfaces/database/flow, Array, BlockButton, Button, DynamicOutput, DynamicOutputForm, FormContainer, FormControl, FormField, FormItem, FormMessage, IProps, Input, RAGFlowNodeType, ReactNode, SelectWithSearch, Separator, TypeScript, VariableTitle, findType, flatOptions, form, i18next, lucide-react, name, nameField, options, react, react-hook-form, react-i18next, type, typeField

---
*Generated by RAGFlow Repository Documentation Generator*
