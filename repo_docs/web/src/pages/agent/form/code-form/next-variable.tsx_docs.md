# File Documentation: web/src/pages/agent/form/code-form/next-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/code-form/next-variable.tsx`
- **Extension**: `.tsx`
- **Lines**: 129
- **Characters**: 3,857
- **Size**: 3,857 bytes
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
import { BlurInput } from '@/components/ui/input';
import { RAGFlowSelect } from '@/components/ui/select';
import { Separator } from '@/components/ui/separator';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { X } from 'lucide-react';
import { ReactNode } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { useBuildQueryVariableOptions } from '../../hooks/use-get-begin-query';

interface IProps {
  node?: RAGFlowNodeType;
  name?: string;
  isOutputs: boolean;
}

export const TypeOptions = [
  'String',
  'Number',
  'Boolean',
  'Array<String>',
  'Array<Number>',
  'Object',
].map((x) => ({ label: x, value: x }));

export function DynamicVariableForm({ name = 'arguments', isOutputs }: IProps) {
  const { t } = useTranslation();
  const form = useFormContext();

  const { fields, remove, append } = useFieldArray({
    name: name,
    control: form.control,
  });

  const nextOptions = useBuildQueryVariableOptions();

  return (
    <div className="space-y-5">
      {fields.map((field, index) => {
        const typeField = `${name}.${index}.name`;
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
              name={`${name}.${index}.type`}
              render={({ field }) => (
                <FormItem className="flex-1 overflow-hidden">
                  <FormControl>
                    {isOutputs ? (
                      <RAGFlowSelect
                        placeholder={t('common.pleaseSelect')}
                        options={TypeOptions}
                        {...field}
                      ></RAGFlowSelect>
                    ) : (
                      <SelectWithSearch
                        options={nextOptions}
                        {...field}
                      ></SelectWithSearch>
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
      <BlockButton onClick={() => append({ name: '', type: undefined })}>
        {t('flow.addVariable')}
      </BlockButton>
    </div>
  );
}

export function VariableTitle({ title }: { title: ReactNode }) {
  return <div className="font-medium text-text-primary pb-2">{title}</div>;
}

export function DynamicInputVariable({
  node,
  name,
  title,
  isOutputs = false,
}: IProps & { title: ReactNode }) {
  return (
    <section>
      <VariableTitle title={title}></VariableTitle>
      <FormContainer>
        <DynamicVariableForm
          node={node}
          name={name}
          isOutputs={isOutputs}
        ></DynamicVariableForm>
      </FormContainer>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/code-form/next-variable.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 129 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `TypeOptions`: Exported entity
- `DynamicVariableForm`: Exported entity
- `VariableTitle`: Exported entity
- `DynamicInputVariable`: Exported entity

### Functions (4)

- `TypeOptions()`: Function definition
- `DynamicVariableForm()`: Function definition
- `VariableTitle()`: Function definition
- `DynamicInputVariable()`: Function definition

### Imports (13)

- `import { FormContainer } from '@/components/form-container';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import {`
- `import { BlurInput } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { Separator } from '@/components/ui/separator';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { X } from 'lucide-react';`
- `import { ReactNode } from 'react';`

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
- `@/components/ui/select`
- `@/components/ui/separator`
- `@/interfaces/database/flow`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../hooks/use-get-begin-query`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/code-form`.

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

- Other files in `web/src/pages/agent/form/code-form/` directory
- Potential test file: `test_next-variable.tsx`

## Keywords

../../hooks/use-get-begin-query, @/components/form-container, @/components/originui/select-with-search, @/components/ui/button, @/components/ui/input, @/components/ui/select, @/components/ui/separator, @/interfaces/database/flow, Array, BlockButton, BlurInput, Boolean, Button, DynamicInputVariable, DynamicVariableForm, FormContainer, FormControl, FormField, FormItem, FormMessage, IProps, Number, Object, RAGFlowNodeType, RAGFlowSelect, ReactNode, SelectWithSearch, Separator, String, TypeOptions, TypeScript, VariableTitle, form, lucide-react, nextOptions, react, react-hook-form, react-i18next, typeField

---
*Generated by RAGFlow Repository Documentation Generator*
