# Documentation: web/src/pages/agent/form/user-fill-up-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/user-fill-up-form/index.tsx`
- **Size**: 4384 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/user-fill-up-form/index.tsx`.

## Original Source Code

```tsx
import { Collapse } from '@/components/collapse';
import { Button } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Switch } from '@/components/ui/switch';
import { FormTooltip } from '@/components/ui/tooltip';
import { zodResolver } from '@hookform/resolvers/zod';
import { Plus } from 'lucide-react';
import { memo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { BeginQuery, INextOperatorForm } from '../../interface';
import { ParameterDialog } from '../begin-form/parameter-dialog';
import { QueryTable } from '../begin-form/query-table';
import { useEditQueryRecord } from '../begin-form/use-edit-query';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

function UserFillUpForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const values = useValues(node);

  const FormSchema = z.object({
    enable_tips: z.boolean().optional(),
    tips: z.string().trim().optional(),
    inputs: z
      .array(
        z.object({
          key: z.string(),
          type: z.string(),
          value: z.string(),
          optional: z.boolean(),
          name: z.string(),
          options: z.array(z.union([z.number(), z.string(), z.boolean()])),
        }),
      )
      .optional(),
  });

  const form = useForm({
    defaultValues: values,
    resolver: zodResolver(FormSchema),
  });

  useWatchFormChange(node?.id, form);

  const inputs: BeginQuery[] = useWatch({
    control: form.control,
    name: 'inputs',
  });

  const outputList = inputs?.map((item) => ({
    title: item.name,
    type: item.type,
  }));

  const {
    ok,
    currentRecord,
    visible,
    hideModal,
    showModal,
    otherThanCurrentQuery,
    handleDeleteRecord,
  } = useEditQueryRecord({
    form,
    node,
  });

  return (
    <section className="px-5 space-y-5">
      <Form {...form}>
        <FormField
          control={form.control}
          name={'enable_tips'}
          render={({ field }) => (
            <FormItem>
              <FormLabel tooltip={t('flow.openingSwitchTip')}>
                {t('flow.guidingQuestion')}
              </FormLabel>
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

        <FormField
          control={form.control}
          name={'tips'}
          render={({ field }) => (
            <FormItem>
              <FormLabel tooltip={t('chat.setAnOpenerTip')}>
                {t('flow.msg')}
              </FormLabel>
              <FormControl>
                <PromptEditor value={field.value} onChange={field.onChange} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />

        {/* Create a hidden field to make Form instance record this */}
        <FormField
          control={form.control}
          name={'inputs'}
          render={() => <div></div>}
        />
        <Collapse
          title={
            <div>
              {t('flow.input')}
              <FormTooltip tooltip={t('flow.beginInputTip')}></FormTooltip>
            </div>
          }
          rightContent={
            <Button
              variant={'ghost'}
              onClick={(e) => {
                e.preventDefault();
                showModal();
              }}
            >
              <Plus />
            </Button>
          }
        >
          <QueryTable
            data={inputs}
            showModal={showModal}
            deleteRecord={handleDeleteRecord}
          ></QueryTable>
        </Collapse>

        {visible && (
          <ParameterDialog
            hideModal={hideModal}
            initialValue={currentRecord}
            otherThanCurrentQuery={otherThanCurrentQuery}
            submit={ok}
          ></ParameterDialog>
        )}
      </Form>
      <Output list={outputList}></Output>
    </section>
  );
}

export default memo(UserFillUpForm);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/user-fill-up-form/index.tsx` is located in the `web/src/pages/agent/form/user-fill-up-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to user-fill-up-form.

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

- [use-values.ts](use-values.ts_docs.md)
- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
