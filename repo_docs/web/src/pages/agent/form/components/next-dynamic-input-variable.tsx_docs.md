# File Documentation: web/src/pages/agent/form/components/next-dynamic-input-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/next-dynamic-input-variable.tsx`
- **Extension**: `.tsx`
- **Lines**: 136
- **Characters**: 4,295
- **Size**: 4,295 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import { SideDown } from '@/assets/icon/next-icon';
import { Button } from '@/components/ui/button';
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible';
import {
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { RAGFlowSelect } from '@/components/ui/select';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { Plus, Trash2 } from 'lucide-react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { useBuildVariableOptions } from '../../hooks/use-get-begin-query';

interface IProps {
  node?: RAGFlowNodeType;
}

enum VariableType {
  Reference = 'reference',
  Input = 'input',
}

const getVariableName = (type: string) =>
  type === VariableType.Reference ? 'component_id' : 'value';

export function DynamicVariableForm({ node }: IProps) {
  const { t } = useTranslation();
  const form = useFormContext();
  const { fields, remove, append } = useFieldArray({
    name: 'query',
    control: form.control,
  });

  const valueOptions = useBuildVariableOptions(node?.id, node?.parentId);

  const options = [
    { value: VariableType.Reference, label: t('flow.reference') },
    { value: VariableType.Input, label: t('flow.text') },
  ];

  return (
    <div>
      {fields.map((field, index) => {
        const typeField = `query.${index}.type`;
        const typeValue = form.watch(typeField);
        return (
          <div key={field.id} className="flex items-center gap-1">
            <FormField
              control={form.control}
              name={typeField}
              render={({ field }) => (
                <FormItem className="w-2/5">
                  <FormDescription />
                  <FormControl>
                    <RAGFlowSelect
                      {...field}
                      placeholder={t('common.pleaseSelect')}
                      options={options}
                      onChange={(val) => {
                        field.onChange(val);
                        form.resetField(`query.${index}.value`);
                        form.resetField(`query.${index}.component_id`);
                      }}
                    ></RAGFlowSelect>
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
            <FormField
              control={form.control}
              name={`query.${index}.${getVariableName(typeValue)}`}
              render={({ field }) => (
                <FormItem className="flex-1">
                  <FormDescription />
                  <FormControl>
                    {typeValue === VariableType.Reference ? (
                      <RAGFlowSelect
                        placeholder={t('common.pleaseSelect')}
                        {...field}
                        options={valueOptions}
                      ></RAGFlowSelect>
                    ) : (
                      <Input placeholder={t('common.pleaseInput')} {...field} />
                    )}
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
            <Trash2
              className="cursor-pointer mx-3 size-4 text-colors-text-functional-danger"
              onClick={() => remove(index)}
            />
          </div>
        );
      })}
      <Button onClick={append} className="mt-4" variant={'outline'} size={'sm'}>
        <Plus />
        {t('flow.addVariable')}
      </Button>
    </div>
  );
}

export function DynamicInputVariable({ node }: IProps) {
  const { t } = useTranslation();

  return (
    <Collapsible defaultOpen className="group/collapsible">
      <CollapsibleTrigger className="flex justify-between w-full pb-2">
        <span className="font-bold text-2xl text-colors-text-neutral-strong">
          {t('flow.input')}
        </span>
        <Button variant={'icon'} size={'icon'}>
          <SideDown />
        </Button>
      </CollapsibleTrigger>
      <CollapsibleContent>
        <DynamicVariableForm node={node}></DynamicVariableForm>
      </CollapsibleContent>
    </Collapsible>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/components/next-dynamic-input-variable.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 136 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `DynamicVariableForm`: Exported entity
- `DynamicInputVariable`: Exported entity

### Functions (4)

- `getVariableName()`: Function definition
- `DynamicVariableForm()`: Function definition
- `options()`: Function definition
- `DynamicInputVariable()`: Function definition

### Imports (11)

- `import { SideDown } from '@/assets/icon/next-icon';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { Plus, Trash2 } from 'lucide-react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 136
- Blank lines: 11 (8.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~125


## Dependencies and Imports

- `@/assets/icon/next-icon`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/select`
- `@/interfaces/database/flow`
- `lucide-react`
- `react-hook-form`
- `react-i18next`
- `../../hooks/use-get-begin-query`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/components`.

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

- Other files in `web/src/pages/agent/form/components/` directory
- Potential test file: `test_next-dynamic-input-variable.tsx`

## Keywords

../../hooks/use-get-begin-query, @/assets/icon/next-icon, @/components/ui/button, @/components/ui/input, @/components/ui/select, @/interfaces/database/flow, Button, Collapsible, CollapsibleContent, CollapsibleTrigger, DynamicInputVariable, DynamicVariableForm, FormControl, FormDescription, FormField, FormItem, FormMessage, IProps, Input, Plus, RAGFlowNodeType, RAGFlowSelect, Reference, SideDown, Trash2, TypeScript, VariableType, form, getVariableName, lucide-react, options, react-hook-form, react-i18next, typeField, typeValue, valueOptions

---
*Generated by RAGFlow Repository Documentation Generator*
