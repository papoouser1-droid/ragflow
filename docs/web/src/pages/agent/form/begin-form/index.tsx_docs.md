# Documentation: web/src/pages/agent/form/begin-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/begin-form/index.tsx`
- **Size**: 5815 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/begin-form/index.tsx`.

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
import { RAGFlowSelect } from '@/components/ui/select';
import { Switch } from '@/components/ui/switch';
import { Textarea } from '@/components/ui/textarea';
import { FormTooltip } from '@/components/ui/tooltip';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { Plus } from 'lucide-react';
import { memo, useEffect, useRef } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { AgentDialogueMode } from '../../constant';
import { INextOperatorForm } from '../../interface';
import { ParameterDialog } from './parameter-dialog';
import { QueryTable } from './query-table';
import { useEditQueryRecord } from './use-edit-query';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

const ModeOptions = [
  { value: AgentDialogueMode.Conversational, label: t('flow.conversational') },
  { value: AgentDialogueMode.Task, label: t('flow.task') },
];

function BeginForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();

  const values = useValues(node);

  const FormSchema = z.object({
    enablePrologue: z.boolean().optional(),
    prologue: z.string().trim().optional(),
    mode: z.string(),
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

  const inputs = useWatch({ control: form.control, name: 'inputs' });
  const mode = useWatch({ control: form.control, name: 'mode' });

  const enablePrologue = useWatch({
    control: form.control,
    name: 'enablePrologue',
  });

  const previousModeRef = useRef(mode);

  useEffect(() => {
    if (
      previousModeRef.current === AgentDialogueMode.Task &&
      mode === AgentDialogueMode.Conversational
    ) {
      form.setValue('enablePrologue', true);
    }
    previousModeRef.current = mode;
  }, [mode, form]);

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
          name={'mode'}
          render={({ field }) => (
            <FormItem>
              <FormLabel tooltip={t('flow.modeTip')}>
                {t('flow.mode')}
              </FormLabel>
              <FormControl>
                <RAGFlowSelect
                  placeholder={t('common.pleaseSelect')}
                  options={ModeOptions}
                  {...field}
                ></RAGFlowSelect>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        {mode === AgentDialogueMode.Conversational && (
          <FormField
            control={form.control}
            name={'enablePrologue'}
            render={({ field }) => (
              <FormItem>
                <FormLabel tooltip={t('flow.openingSwitchTip')}>
                  {t('flow.openingSwitch')}
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
        )}
        {mode === AgentDialogueMode.Conversational && enablePrologue && (
          <FormField
            control={form.control}
            name={'prologue'}
            render={({ field }) => (
              <FormItem>
                <FormLabel tooltip={t('chat.setAnOpenerTip')}>
                  {t('flow.openingCopy')}
                </FormLabel>
                <FormControl>
                  <Textarea
                    rows={5}
                    {...field}
                    placeholder={t('common.pleaseInput')}
                  ></Textarea>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        )}
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
    </section>
  );
}

export default memo(BeginForm);

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/begin-form/index.tsx` is located in the `web/src/pages/agent/form/begin-form` directory.

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
- [parameter-dialog.tsx](parameter-dialog.tsx_docs.md)
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
