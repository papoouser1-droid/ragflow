# File Documentation: web/src/pages/agent/form/user-fill-up-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/user-fill-up-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 165
- **Characters**: 4,384
- **Size**: 4,384 bytes
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/user-fill-up-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 165 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `UserFillUpForm()`: Function definition
- `outputList()`: Function definition

### Imports (19)

- `import { Collapse } from '@/components/collapse';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { Switch } from '@/components/ui/switch';`
- `import { FormTooltip } from '@/components/ui/tooltip';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { Plus } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useForm, useWatch } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 165
- Blank lines: 14 (8.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~151


## Dependencies and Imports

- `@/components/collapse`
- `@/components/ui/button`
- `@/components/ui/switch`
- `@/components/ui/tooltip`
- `@hookform/resolvers/zod`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../interface`
- `../begin-form/parameter-dialog`
- `../begin-form/query-table`
- `../begin-form/use-edit-query`
- `../components/output`
- `../components/prompt-editor`
- `./use-values`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/user-fill-up-form`.

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

- Other files in `web/src/pages/agent/form/user-fill-up-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../begin-form/parameter-dialog, ../begin-form/query-table, ../begin-form/use-edit-query, ../components/output, ../components/prompt-editor, ./use-values, ./use-watch-change, @/components/collapse, @/components/ui/button, @/components/ui/switch, @/components/ui/tooltip, @hookform/resolvers/zod, BeginQuery, Button, Collapse, Create, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, FormTooltip, INextOperatorForm, Output, ParameterDialog, Plus, PromptEditor, QueryTable, Switch, TypeScript, UserFillUpForm, form, hookform, inputs, lucide-react, outputList, react, react-hook-form, react-i18next, values, zod

---
*Generated by RAGFlow Repository Documentation Generator*
