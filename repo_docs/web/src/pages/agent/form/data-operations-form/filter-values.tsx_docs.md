# File Documentation: web/src/pages/agent/form/data-operations-form/filter-values.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/data-operations-form/filter-values.tsx`
- **Extension**: `.tsx`
- **Lines**: 83
- **Characters**: 2,710
- **Size**: 2,710 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { KeyInput } from '@/components/key-input';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Button } from '@/components/ui/button';
import { Separator } from '@/components/ui/separator';
import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';
import { X } from 'lucide-react';
import { ReactNode } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { DataOperationsOperatorOptions } from '../../constant';
import { DynamicFormHeader } from '../components/dynamic-fom-header';
import { PromptEditor } from '../components/prompt-editor';

type SelectKeysProps = {
  name: string;
  label: ReactNode;
  tooltip?: string;
  keyField?: string;
  valueField?: string;
  operatorField?: string;
};
export function FilterValues({
  name,
  label,
  tooltip,
  keyField = 'key',
  valueField = 'value',
  operatorField = 'operator',
}: SelectKeysProps) {
  const form = useFormContext();

  const { fields, remove, append } = useFieldArray({
    name: name,
    control: form.control,
  });

  const operatorOptions = useBuildSwitchOperatorOptions(
    DataOperationsOperatorOptions,
  );

  return (
    <section className="space-y-2">
      <DynamicFormHeader
        label={label}
        tooltip={tooltip}
        onClick={() => append({ [keyField]: '', [valueField]: '' })}
      ></DynamicFormHeader>

      <div className="space-y-5">
        {fields.map((field, index) => {
          const keyFieldAlias = `${name}.${index}.${keyField}`;
          const valueFieldAlias = `${name}.${index}.${valueField}`;
          const operatorFieldAlias = `${name}.${index}.${operatorField}`;

          return (
            <div key={field.id} className="flex items-center gap-2">
              <RAGFlowFormItem name={keyFieldAlias} className="flex-1">
                <KeyInput></KeyInput>
              </RAGFlowFormItem>
              <Separator className="w-2" />

              <RAGFlowFormItem name={operatorFieldAlias} className="flex-1">
                <SelectWithSearch
                  {...field}
                  options={operatorOptions}
                ></SelectWithSearch>
              </RAGFlowFormItem>
              <Separator className="w-2" />

              <RAGFlowFormItem name={valueFieldAlias} className="flex-1">
                <PromptEditor showToolbar={false} multiLine={false} />
              </RAGFlowFormItem>
              <Button variant={'ghost'} onClick={() => remove(index)}>
                <X />
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

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/data-operations-form/filter-values.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 83 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FilterValues`: Exported entity

### Functions (1)

- `FilterValues()`: Function definition

### Imports (12)

- `import { KeyInput } from '@/components/key-input';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Button } from '@/components/ui/button';`
- `import { Separator } from '@/components/ui/separator';`
- `import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';`
- `import { X } from 'lucide-react';`
- `import { ReactNode } from 'react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { DataOperationsOperatorOptions } from '../../constant';`

## Code Structure Analysis

- Total lines: 83
- Blank lines: 9 (10.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~74


## Dependencies and Imports

- `@/components/key-input`
- `@/components/originui/select-with-search`
- `@/components/ragflow-form`
- `@/components/ui/button`
- `@/components/ui/separator`
- `@/hooks/logic-hooks/use-build-operator-options`
- `lucide-react`
- `react`
- `react-hook-form`
- `../../constant`
- `../components/dynamic-fom-header`
- `../components/prompt-editor`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/data-operations-form`.

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

- Other files in `web/src/pages/agent/form/data-operations-form/` directory
- Potential test file: `test_filter-values.tsx`

## Keywords

../../constant, ../components/dynamic-fom-header, ../components/prompt-editor, @/components/key-input, @/components/originui/select-with-search, @/components/ragflow-form, @/components/ui/button, @/components/ui/separator, @/hooks/logic-hooks/use-build-operator-options, Button, DataOperationsOperatorOptions, DynamicFormHeader, FilterValues, KeyInput, PromptEditor, RAGFlowFormItem, ReactNode, SelectKeysProps, SelectWithSearch, Separator, TypeScript, form, keyFieldAlias, lucide-react, operatorFieldAlias, operatorOptions, react, react-hook-form, valueFieldAlias

---
*Generated by RAGFlow Repository Documentation Generator*
