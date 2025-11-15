# File Documentation: web/src/pages/agent/form/begin-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/begin-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 206
- **Characters**: 5,815
- **Size**: 5,815 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/begin-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 206 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `BeginForm()`: Function definition
- `previousModeRef()`: Function definition

### Imports (21)

- `import { Collapse } from '@/components/collapse';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { Switch } from '@/components/ui/switch';`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { FormTooltip } from '@/components/ui/tooltip';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { t } from 'i18next';`
- `import { Plus } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 206
- Blank lines: 14 (6.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~192


## Dependencies and Imports

- `@/components/collapse`
- `@/components/ui/button`
- `@/components/ui/select`
- `@/components/ui/switch`
- `@/components/ui/textarea`
- `@/components/ui/tooltip`
- `@hookform/resolvers/zod`
- `i18next`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../constant`
- `../../interface`
- `./parameter-dialog`
- `./query-table`
- `./use-edit-query`
- `./use-values`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/begin-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/begin-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../interface, ./parameter-dialog, ./query-table, ./use-edit-query, ./use-values, ./use-watch-change, @/components/collapse, @/components/ui/button, @/components/ui/select, @/components/ui/switch, @/components/ui/textarea, @/components/ui/tooltip, @hookform/resolvers/zod, AgentDialogueMode, BeginForm, Button, Collapse, Conversational, Create, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormTooltip, INextOperatorForm, ModeOptions, ParameterDialog, Plus, QueryTable, RAGFlowSelect, Switch, Task, Textarea, TypeScript, enablePrologue, form, hookform, i18next, inputs, lucide-react, mode, previousModeRef, react, react-hook-form, react-i18next, values...

---
*Generated by RAGFlow Repository Documentation Generator*
