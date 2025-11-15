# Documentation: web/src/pages/agent/form/begin-form/parameter-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/begin-form/parameter-dialog.tsx`
- **Size**: 5993 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/begin-form/parameter-dialog.tsx`.

## Original Source Code

```tsx
import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogContent,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { RAGFlowSelect, RAGFlowSelectOptionType } from '@/components/ui/select';
import { Switch } from '@/components/ui/switch';
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { zodResolver } from '@hookform/resolvers/zod';
import { isEmpty } from 'lodash';
import { ChangeEvent, useEffect, useMemo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { BeginQueryType, BeginQueryTypeIconMap } from '../../constant';
import { BeginQuery } from '../../interface';
import { BeginDynamicOptions } from './begin-dynamic-options';

type ModalFormProps = {
  initialValue: BeginQuery;
  otherThanCurrentQuery: BeginQuery[];
  submit(values: any): void;
};

const FormId = 'BeginParameterForm';

function ParameterForm({
  initialValue,
  otherThanCurrentQuery,
  submit,
}: ModalFormProps) {
  const { t } = useTranslate('flow');
  const FormSchema = z.object({
    type: z.string(),
    key: z
      .string()
      .trim()
      .min(1)
      .refine(
        (value) =>
          !value || !otherThanCurrentQuery.some((x) => x.key === value),
        { message: 'The key cannot be repeated!' },
      ),
    optional: z.boolean(),
    name: z.string().trim().min(1),
    options: z
      .array(z.object({ value: z.string().or(z.boolean()).or(z.number()) }))
      .optional(),
  });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    mode: 'onChange',
    defaultValues: {
      type: BeginQueryType.Line,
      optional: false,
      key: '',
      name: '',
      options: [],
    },
  });

  const options = useMemo(() => {
    return Object.values(BeginQueryType).reduce<RAGFlowSelectOptionType[]>(
      (pre, cur) => {
        const Icon = BeginQueryTypeIconMap[cur];

        return [
          ...pre,
          {
            label: (
              <div className="flex items-center gap-2">
                <Icon
                  className={`size-${cur === BeginQueryType.Options ? 4 : 5}`}
                ></Icon>
                {t(cur.toLowerCase())}
              </div>
            ),
            value: cur,
          },
        ];
      },
      [],
    );
  }, []);

  const type = useWatch({
    control: form.control,
    name: 'type',
  });

  useEffect(() => {
    if (!isEmpty(initialValue)) {
      form.reset({
        ...initialValue,
        options: initialValue.options?.map((x) => ({ value: x })),
      });
    }
  }, [form, initialValue]);

  function onSubmit(data: z.infer<typeof FormSchema>) {
    const values = { ...data, options: data.options?.map((x) => x.value) };
    console.log('🚀 ~ onSubmit ~ values:', values);

    submit(values);
  }

  const handleKeyChange = (e: ChangeEvent<HTMLInputElement>) => {
    const name = form.getValues().name || '';
    form.setValue('key', e.target.value.trim());
    if (!name) {
      form.setValue('name', e.target.value.trim());
    }
  };
  return (
    <Form {...form}>
      <form
        onSubmit={form.handleSubmit(onSubmit)}
        id={FormId}
        className="space-y-5"
        autoComplete="off"
      >
        <FormField
          name="type"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('type')}</FormLabel>
              <FormControl>
                <RAGFlowSelect {...field} options={options} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          name="key"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('key')}</FormLabel>
              <FormControl>
                <Input {...field} autoComplete="off" onBlur={handleKeyChange} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          name="name"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('name')}</FormLabel>
              <FormControl>
                <Input {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          name="optional"
          control={form.control}
          render={({ field }) => (
            <FormItem>
              <FormLabel>{t('optional')}</FormLabel>
              <FormControl>
                <Switch
                  checked={field.value}
                  onCheckedChange={field.onChange}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        {type === BeginQueryType.Options && (
          <BeginDynamicOptions></BeginDynamicOptions>
        )}
      </form>
    </Form>
  );
}

export function ParameterDialog({
  initialValue,
  hideModal,
  otherThanCurrentQuery,
  submit,
}: ModalFormProps & IModalProps<BeginQuery>) {
  const { t } = useTranslation();

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{t('flow.variableSettings')}</DialogTitle>
        </DialogHeader>
        <ParameterForm
          initialValue={initialValue}
          otherThanCurrentQuery={otherThanCurrentQuery}
          submit={submit}
        ></ParameterForm>
        <DialogFooter>
          <Button type="submit" form={FormId}>
            {t('modal.okText')}
          </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/begin-form/parameter-dialog.tsx` is located in the `web/src/pages/agent/form/begin-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to begin-form.

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

- [begin-dynamic-options.tsx](begin-dynamic-options.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [query-table.tsx](query-table.tsx_docs.md)
- [use-edit-query.ts](use-edit-query.ts_docs.md)
- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
