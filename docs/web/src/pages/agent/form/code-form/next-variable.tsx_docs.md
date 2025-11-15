# Documentation: web/src/pages/agent/form/code-form/next-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/code-form/next-variable.tsx`
- **Size**: 3857 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/code-form/next-variable.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/code-form/next-variable.tsx` is located in the `web/src/pages/agent/form/code-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to code-form.

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
- [schema.ts](schema.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
