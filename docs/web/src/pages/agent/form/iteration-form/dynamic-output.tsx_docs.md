# Documentation: web/src/pages/agent/form/iteration-form/dynamic-output.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/iteration-form/dynamic-output.tsx`
- **Size**: 3921 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/iteration-form/dynamic-output.tsx`.

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/iteration-form/dynamic-output.tsx` is located in the `web/src/pages/agent/form/iteration-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to iteration-form.

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
- [interface.ts](interface.ts_docs.md)
- [use-build-options.ts](use-build-options.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-form-change.ts](use-watch-form-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
