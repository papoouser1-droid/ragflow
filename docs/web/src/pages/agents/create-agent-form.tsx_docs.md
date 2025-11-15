# Documentation: web/src/pages/agents/create-agent-form.tsx

## File Metadata

- **Path**: `web/src/pages/agents/create-agent-form.tsx`
- **Size**: 3486 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agents/create-agent-form.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agents/create-agent-form.tsx` is located in the `web/src/pages/agents` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to agents.

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

- [agent-card.tsx](agent-card.tsx_docs.md)
- [agent-dropdown.tsx](agent-dropdown.tsx_docs.md)
- [agent-log-detail-modal.tsx](agent-log-detail-modal.tsx_docs.md)
- [agent-log-page.tsx](agent-log-page.tsx_docs.md)
- [agent-templates.tsx](agent-templates.tsx_docs.md)
- [constant.ts](constant.ts_docs.md)
- [create-agent-dialog.tsx](create-agent-dialog.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [name-form-field.tsx](name-form-field.tsx_docs.md)
- [template-card.tsx](template-card.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
