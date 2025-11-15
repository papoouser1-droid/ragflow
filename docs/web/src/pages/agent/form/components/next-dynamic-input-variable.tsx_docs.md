# Documentation: web/src/pages/agent/form/components/next-dynamic-input-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/next-dynamic-input-variable.tsx`
- **Size**: 4295 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/components/next-dynamic-input-variable.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/components/next-dynamic-input-variable.tsx` is located in the `web/src/pages/agent/form/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [api-key-field.tsx](api-key-field.tsx_docs.md)
- [description-field.tsx](description-field.tsx_docs.md)
- [dynamic-fom-header.tsx](dynamic-fom-header.tsx_docs.md)
- [dynamic-input-variable.tsx](dynamic-input-variable.tsx_docs.md)
- [form-wrapper.tsx](form-wrapper.tsx_docs.md)
- [index.less](index.less_docs.md)
- [output.tsx](output.tsx_docs.md)
- [query-variable-list.tsx](query-variable-list.tsx_docs.md)
- [query-variable.tsx](query-variable.tsx_docs.md)
- [select-with-secondary-menu.tsx](select-with-secondary-menu.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
