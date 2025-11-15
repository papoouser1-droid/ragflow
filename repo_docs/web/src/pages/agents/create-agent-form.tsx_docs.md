# File Documentation: web/src/pages/agents/create-agent-form.tsx

## File Metadata

- **Path**: `web/src/pages/agents/create-agent-form.tsx`
- **Extension**: `.tsx`
- **Lines**: 127
- **Characters**: 3,486
- **Size**: 3,486 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';
import { z } from 'zod';

import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Card, CardContent } from '@/components/ui/card';
import { Form } from '@/components/ui/form';
import { IModalProps } from '@/interfaces/common';
import { cn } from '@/lib/utils';
import { TagRenameId } from '@/pages/add-knowledge/constant';
import { BrainCircuit, Check, Route } from 'lucide-react';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { FlowType } from './constant';
import { NameFormField, NameFormSchema } from './name-form-field';

export type CreateAgentFormProps = IModalProps<any> & {
  shouldChooseAgent?: boolean;
};

type FlowTypeCardProps = {
  value?: FlowType;
  onChange?: (value: FlowType) => void;
};
function FlowTypeCards({ value, onChange }: FlowTypeCardProps) {
  const { t } = useTranslation();
  const handleChange = useCallback(
    (value: FlowType) => () => {
      onChange?.(value);
    },
    [onChange],
  );

  return (
    <section className="flex gap-10">
      {Object.values(FlowType).map((val) => {
        const isActive = value === val;
        return (
          <Card
            key={val}
            className={cn('flex-1 rounded-lg  border bg-transparent', {
              'border-text-primary': isActive,
              'border-border-default': !isActive,
            })}
          >
            <CardContent
              onClick={handleChange(val)}
              className={cn(
                'cursor-pointer p-5 text-text-secondary flex justify-between items-center',
                {
                  'text-text-primary': isActive,
                },
              )}
            >
              <div className="flex gap-2">
                {val === FlowType.Agent ? (
                  <BrainCircuit className="size-6" />
                ) : (
                  <Route className="size-6" />
                )}
                <p>
                  {t(
                    `flow.${val === FlowType.Agent ? 'createAgent' : 'createPipeline'}`,
                  )}
                </p>
              </div>
              {isActive && <Check />}
            </CardContent>
          </Card>
        );
      })}
    </section>
  );
}

export const FormSchema = z.object({
  ...NameFormSchema,
  tag: z.string().trim().optional(),
  description: z.string().trim().optional(),
  type: z.nativeEnum(FlowType).optional(),
});

export type FormSchemaType = z.infer<typeof FormSchema>;

export function CreateAgentForm({
  hideModal,
  onOk,
  shouldChooseAgent = false,
}: CreateAgentFormProps) {
  const { t } = useTranslation();

  const form = useForm<FormSchemaType>({
    resolver: zodResolver(FormSchema),
    defaultValues: { name: '', type: FlowType.Agent },
  });

  async function onSubmit(data: FormSchemaType) {
    const ret = await onOk?.(data);
    if (ret) {
      hideModal?.();
    }
  }

  return (
    <Form {...form}>
      <form
        onSubmit={form.handleSubmit(onSubmit)}
        className="space-y-6"
        id={TagRenameId}
      >
        {shouldChooseAgent && (
          <RAGFlowFormItem
            required
            name="type"
            label={t('flow.chooseAgentType')}
          >
            <FlowTypeCards></FlowTypeCards>
          </RAGFlowFormItem>
        )}
        <NameFormField></NameFormField>
      </form>
    </Form>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agents/create-agent-form.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 127 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `FormSchema`: Exported entity
- `CreateAgentForm`: Exported entity

### Functions (4)

- `FlowTypeCards()`: Function definition
- `handleChange()`: Function definition
- `CreateAgentForm()`: Function definition
- `onSubmit()`: Function definition

### Imports (14)

- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Card, CardContent } from '@/components/ui/card';`
- `import { Form } from '@/components/ui/form';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { cn } from '@/lib/utils';`
- `import { TagRenameId } from '@/pages/add-knowledge/constant';`
- `import { BrainCircuit, Check, Route } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 127
- Blank lines: 12 (9.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~115


## Dependencies and Imports

- `@hookform/resolvers/zod`
- `react-hook-form`
- `zod`
- `@/components/ragflow-form`
- `@/components/ui/card`
- `@/components/ui/form`
- `@/interfaces/common`
- `@/lib/utils`
- `@/pages/add-knowledge/constant`
- `lucide-react`
- `react`
- `react-i18next`
- `./constant`
- `./name-form-field`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agents`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agents/` directory
- Potential test file: `test_create-agent-form.tsx`

## Keywords

./constant, ./name-form-field, @/components/ragflow-form, @/components/ui/card, @/components/ui/form, @/interfaces/common, @/lib/utils, @/pages/add-knowledge/constant, @hookform/resolvers/zod, Agent, BrainCircuit, Card, CardContent, Check, CreateAgentForm, CreateAgentFormProps, FlowType, FlowTypeCardProps, FlowTypeCards, Form, FormSchema, FormSchemaType, IModalProps, NameFormField, NameFormSchema, Object, RAGFlowFormItem, Route, TagRenameId, TypeScript, form, handleChange, hookform, isActive, lucide-react, onSubmit, react, react-hook-form, react-i18next, ret, zod

---
*Generated by RAGFlow Repository Documentation Generator*
