# File Documentation: web/src/pages/agent/form/categorize-form/dynamic-categorize.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/categorize-form/dynamic-categorize.tsx`
- **Extension**: `.tsx`
- **Lines**: 250
- **Characters**: 6,820
- **Size**: 6,820 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { BlurTextarea } from '@/components/ui/textarea';
import { useTranslate } from '@/hooks/common-hooks';
import { PlusOutlined } from '@ant-design/icons';
import { useUpdateNodeInternals } from '@xyflow/react';
import humanId from 'human-id';
import trim from 'lodash/trim';
import { ChevronsUpDown, X } from 'lucide-react';
import {
  ChangeEventHandler,
  FocusEventHandler,
  memo,
  useCallback,
  useEffect,
  useState,
} from 'react';
import { UseFormReturn, useFieldArray, useFormContext } from 'react-hook-form';
import { v4 as uuid } from 'uuid';
import { z } from 'zod';
import useGraphStore from '../../store';
import DynamicExample from './dynamic-example';
import { useCreateCategorizeFormSchema } from './use-form-schema';

interface IProps {
  nodeId?: string;
}

interface INameInputProps {
  value?: string;
  onChange?: (value: string) => void;
  otherNames?: string[];
  validate(error?: string): void;
}

const getOtherFieldValues = (
  form: UseFormReturn,
  formListName: string = 'items',
  index: number,
  latestField: string,
) =>
  (form.getValues(formListName) ?? [])
    .map((x: any) => x[latestField])
    .filter(
      (x: string) =>
        x !== form.getValues(`${formListName}.${index}.${latestField}`),
    );

const InnerNameInput = ({
  value,
  onChange,
  otherNames,
  validate,
}: INameInputProps) => {
  const [name, setName] = useState<string | undefined>();
  const { t } = useTranslate('flow');

  const handleNameChange: ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      const val = e.target.value;
      setName(val);
      const trimmedVal = trim(val);
      // trigger validation
      if (otherNames?.some((x) => x === trimmedVal)) {
        validate(t('nameRepeatedMsg'));
      } else if (trimmedVal === '') {
        validate(t('nameRequiredMsg'));
      } else {
        validate('');
      }
    },
    [otherNames, validate, t],
  );

  const handleNameBlur: FocusEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      const val = e.target.value;
      if (otherNames?.every((x) => x !== val) && trim(val) !== '') {
        onChange?.(val);
      }
    },
    [onChange, otherNames],
  );

  useEffect(() => {
    setName(value);
  }, [value]);

  return (
    <Input
      value={name}
      onChange={handleNameChange}
      onBlur={handleNameBlur}
    ></Input>
  );
};

const NameInput = memo(InnerNameInput);

const InnerFormSet = ({ index }: IProps & { index: number }) => {
  const form = useFormContext();
  const { t } = useTranslate('flow');

  const buildFieldName = useCallback(
    (name: string) => {
      return `items.${index}.${name}`;
    },
    [index],
  );

  return (
    <section className="space-y-4">
      <FormField
        control={form.control}
        name={buildFieldName('name')}
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('categoryName')}</FormLabel>
            <FormControl>
              <NameInput
                {...field}
                otherNames={getOtherFieldValues(form, 'items', index, 'name')}
                validate={(error?: string) => {
                  const fieldName = buildFieldName('name');
                  if (error) {
                    form.setError(fieldName, { message: error });
                  } else {
                    form.clearErrors(fieldName);
                  }
                }}
              ></NameInput>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <FormField
        control={form.control}
        name={buildFieldName('description')}
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('description')}</FormLabel>
            <FormControl>
              <BlurTextarea {...field} rows={3} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      {/* Create a hidden field to make Form instance record this */}
      <FormField
        control={form.control}
        name={'uuid'}
        render={() => <div></div>}
      />
      <DynamicExample name={buildFieldName('examples')}></DynamicExample>
    </section>
  );
};

const FormSet = memo(InnerFormSet);

const DynamicCategorize = ({ nodeId }: IProps) => {
  const updateNodeInternals = useUpdateNodeInternals();
  const FormSchema = useCreateCategorizeFormSchema();

  const deleteCategorizeCaseEdges = useGraphStore(
    (state) => state.deleteEdgesBySourceAndSourceHandle,
  );
  const form = useFormContext<z.infer<typeof FormSchema>>();
  const { t } = useTranslate('flow');
  const { fields, remove, append } = useFieldArray({
    name: 'items',
    control: form.control,
  });

  const handleAdd = useCallback(() => {
    append({
      name: humanId(),
      description: '',
      uuid: uuid(),
      examples: [{ value: '' }],
    });
    if (nodeId) updateNodeInternals(nodeId);
  }, [append, nodeId, updateNodeInternals]);

  const handleRemove = useCallback(
    (index: number) => () => {
      remove(index);
      if (nodeId) {
        const uuid = fields[index].uuid;
        deleteCategorizeCaseEdges(nodeId, uuid);
      }
    },
    [deleteCategorizeCaseEdges, fields, nodeId, remove],
  );

  return (
    <div className="flex flex-col gap-4 ">
      {fields.map((field, index) => (
        <Collapsible key={field.id} defaultOpen>
          <div className="flex items-center justify-between space-x-4">
            <h4 className="font-bold">
              {form.getValues(`items.${index}.name`)}
            </h4>
            <CollapsibleTrigger asChild>
              <div className="flex gap-4">
                <Button
                  variant="ghost"
                  size="sm"
                  className="w-9 p-0"
                  onClick={handleRemove(index)}
                >
                  <X className="h-4 w-4" />
                </Button>
                <Button variant="ghost" size="sm" className="w-9 p-0">
                  <ChevronsUpDown className="h-4 w-4" />
                  <span className="sr-only">Toggle</span>
                </Button>
              </div>
            </CollapsibleTrigger>
          </div>
          <CollapsibleContent>
            <FormSet nodeId={nodeId} index={index}></FormSet>
          </CollapsibleContent>
        </Collapsible>
      ))}

      <Button type={'button'} onClick={handleAdd}>
        <PlusOutlined />
        {t('addCategory')}
      </Button>
    </div>
  );
};

export default memo(DynamicCategorize);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/categorize-form/dynamic-categorize.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 250 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (10)

- `getOtherFieldValues()`: Function definition
- `InnerNameInput()`: Function definition
- `trimmedVal()`: Function definition
- `val()`: Function definition
- `InnerFormSet()`: Function definition
- `buildFieldName()`: Function definition
- `DynamicCategorize()`: Function definition
- `deleteCategorizeCaseEdges()`: Function definition
- `handleAdd()`: Function definition
- `handleRemove()`: Function definition

### Imports (18)

- `import { Button } from '@/components/ui/button';`
- `import {`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { BlurTextarea } from '@/components/ui/textarea';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { PlusOutlined } from '@ant-design/icons';`
- `import { useUpdateNodeInternals } from '@xyflow/react';`
- `import humanId from 'human-id';`
- `import trim from 'lodash/trim';`

## Code Structure Analysis

- Total lines: 250
- Blank lines: 21 (8.4%)
- Comment lines: ~1 (0.4%)
- Code lines: ~228


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/textarea`
- `@/hooks/common-hooks`
- `@ant-design/icons`
- `@xyflow/react`
- `human-id`
- `lodash/trim`
- `lucide-react`
- `react-hook-form`
- `uuid`
- `zod`
- `../../store`
- `./dynamic-example`
- `./use-form-schema`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/categorize-form`.

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

- Other files in `web/src/pages/agent/form/categorize-form/` directory
- Potential test file: `test_dynamic-categorize.tsx`

## Keywords

../../store, ./dynamic-example, ./use-form-schema, @/components/ui/button, @/components/ui/input, @/components/ui/textarea, @/hooks/common-hooks, @ant-design/icons, @xyflow/react, BlurTextarea, Button, ChangeEventHandler, ChevronsUpDown, Collapsible, CollapsibleContent, CollapsibleTrigger, Create, DynamicCategorize, DynamicExample, FocusEventHandler, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormSet, HTMLInputElement, INameInputProps, IProps, InnerFormSet, InnerNameInput, Input, NameInput, PlusOutlined, Toggle, TypeScript, UseFormReturn, ant, buildFieldName, deleteCategorizeCaseEdges, fieldName, form, getOtherFieldValues, handleAdd, handleNameBlur, handleNameChange, handleRemove, human-id...

---
*Generated by RAGFlow Repository Documentation Generator*
