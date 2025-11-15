# Documentation: web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx`
- **Size**: 3296 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/variable-aggregator-form/dynamic-group-variable.tsx` is located in the `web/src/pages/agent/form/variable-aggregator-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to variable-aggregator-form.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [index.tsx](index.tsx_docs.md)
- [name-input.tsx](name-input.tsx_docs.md)
- [schema.ts](schema.ts_docs.md)
- [use-handle-name-change.ts](use-handle-name-change.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
