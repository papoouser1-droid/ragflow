# File Documentation: web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx`
- **Extension**: `.tsx`
- **Lines**: 106
- **Characters**: 3,296
- **Size**: 3,296 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Plus, Trash2 } from 'lucide-react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';
import { QueryVariable } from '../components/query-variable';
import { NameInput } from './name-input';

type DynamicGroupVariableProps = {
  name: string;
  parentIndex: number;
  removeParent: (index: number) => void;
};

export function DynamicGroupVariable({
  name,
  parentIndex,
  removeParent,
}: DynamicGroupVariableProps) {
  const form = useFormContext();

  const variableFieldName = `${name}.variables`;

  const { getType } = useGetVariableLabelOrTypeByValue();

  const { fields, remove, append } = useFieldArray({
    name: variableFieldName,
    control: form.control,
  });

  const firstValue = form.getValues(`${variableFieldName}.0.value`);
  const firstType = getType(firstValue);

  return (
    <section className="py-3 group space-y-3">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-3">
          <RAGFlowFormItem name={`${name}.group_name`} className="w-32">
            {(field) => (
              <NameInput
                value={field.value}
                onChange={field.onChange}
              ></NameInput>
            )}
          </RAGFlowFormItem>
          {/* Use a hidden form to store data types; otherwise, data loss may occur. */}
          <RAGFlowFormItem name={`${name}.type`} className="hidden">
            <Input></Input>
          </RAGFlowFormItem>
          <Button
            variant={'ghost'}
            type="button"
            className="hidden group-hover:block"
            onClick={() => removeParent(parentIndex)}
          >
            <Trash2 />
          </Button>
        </div>
        <div className="flex gap-2 items-center">
          {firstType && (
            <span className="text-text-secondary border px-1 rounded-md">
              {firstType}
            </span>
          )}
          <Button
            variant={'ghost'}
            type="button"
            onClick={() => append({ value: '' })}
          >
            <Plus />
          </Button>
        </div>
      </div>

      <section className="space-y-3">
        {fields.map((field, index) => (
          <div key={field.id} className="flex gap-2 items-center">
            <QueryVariable
              name={`${variableFieldName}.${index}.value`}
              className="flex-1 min-w-0"
              hideLabel
              types={firstType && fields.length > 1 ? [firstType] : []}
              onChange={(val) => {
                const type = getType(val);
                if (type && index === 0) {
                  form.setValue(`${name}.type`, type, {
                    shouldDirty: true,
                  });
                }
              }}
            ></QueryVariable>
            <Button
              variant={'ghost'}
              type="button"
              onClick={() => remove(index)}
            >
              <Trash2 />
            </Button>
          </div>
        ))}
      </section>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 106 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `DynamicGroupVariable`: Exported entity

### Functions (1)

- `DynamicGroupVariable()`: Function definition

### Imports (8)

- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Button } from '@/components/ui/button';`
- `import { Input } from '@/components/ui/input';`
- `import { Plus, Trash2 } from 'lucide-react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { useGetVariableLabelOrTypeByValue } from '../../hooks/use-get-begin-query';`
- `import { QueryVariable } from '../components/query-variable';`
- `import { NameInput } from './name-input';`

## Code Structure Analysis

- Total lines: 106
- Blank lines: 9 (8.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~97


## Dependencies and Imports

- `@/components/ragflow-form`
- `@/components/ui/button`
- `@/components/ui/input`
- `lucide-react`
- `react-hook-form`
- `../../hooks/use-get-begin-query`
- `../components/query-variable`
- `./name-input`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/variable-aggregator-form`.

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

- Other files in `web/src/pages/agent/form/variable-aggregator-form/` directory
- Potential test file: `test_dynamic-group-variable.tsx`

## Keywords

../../hooks/use-get-begin-query, ../components/query-variable, ./name-input, @/components/ragflow-form, @/components/ui/button, @/components/ui/input, Button, DynamicGroupVariable, DynamicGroupVariableProps, Input, NameInput, Plus, QueryVariable, RAGFlowFormItem, Trash2, TypeScript, Use, firstType, firstValue, form, lucide-react, react-hook-form, type, variableFieldName

---
*Generated by RAGFlow Repository Documentation Generator*
