# File Documentation: web/src/pages/agent/form/agent-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 304
- **Characters**: 10,717
- **Size**: 10,717 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Collapse } from '@/components/collapse';
import {
  LargeModelFilterFormSchema,
  LargeModelFormField,
} from '@/components/large-model-form-field';
import { LlmSettingSchema } from '@/components/llm-setting-items/next';
import { MessageHistoryWindowSizeFormField } from '@/components/message-history-window-size-item';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { Button } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
} from '@/components/ui/form';
import { Input, NumberInput } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { Switch } from '@/components/ui/switch';
import { LlmModelType } from '@/constants/knowledge';
import { useFindLlmByUuid } from '@/hooks/use-llm-request';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useEffect, useMemo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  AgentExceptionMethod,
  NodeHandleId,
  VariableType,
  initialAgentValues,
} from '../../constant';
import { INextOperatorForm } from '../../interface';
import useGraphStore from '../../store';
import { hasSubAgentOrTool, isBottomSubAgent } from '../../utils';
import { buildOutputList } from '../../utils/build-output-list';
import { DescriptionField } from '../components/description-field';
import { FormWrapper } from '../components/form-wrapper';
import { Output } from '../components/output';
import { PromptEditor } from '../components/prompt-editor';
import { QueryVariable } from '../components/query-variable';
import { AgentTools, Agents } from './agent-tools';
import { StructuredOutputDialog } from './structured-output-dialog';
import { StructuredOutputPanel } from './structured-output-panel';
import { useBuildPromptExtraPromptOptions } from './use-build-prompt-options';
import { useShowStructuredOutputDialog } from './use-show-structured-output-dialog';
import { useValues } from './use-values';
import { useWatchFormChange } from './use-watch-change';

const FormSchema = z.object({
  sys_prompt: z.string(),
  description: z.string().optional(),
  user_prompt: z.string().optional(),
  prompts: z.string().optional(),
  // prompts: z
  //   .array(
  //     z.object({
  //       role: z.string(),
  //       content: z.string(),
  //     }),
  //   )
  //   .optional(),
  message_history_window_size: z.coerce.number(),
  ...LlmSettingSchema,
  max_retries: z.coerce.number(),
  delay_after_error: z.coerce.number().optional(),
  visual_files_var: z.string().optional(),
  max_rounds: z.coerce.number().optional(),
  exception_method: z.string().optional(),
  exception_goto: z.array(z.string()).optional(),
  exception_default_value: z.string().optional(),
  ...LargeModelFilterFormSchema,
  cite: z.boolean().optional(),
});

export type AgentFormSchemaType = z.infer<typeof FormSchema>;

const outputList = buildOutputList(initialAgentValues.outputs);

function AgentForm({ node }: INextOperatorForm) {
  const { t } = useTranslation();
  const { edges, deleteEdgesBySourceAndSourceHandle } = useGraphStore(
    (state) => state,
  );

  const defaultValues = useValues(node);

  const { extraOptions } = useBuildPromptExtraPromptOptions(edges, node?.id);

  const ExceptionMethodOptions = Object.values(AgentExceptionMethod).map(
    (x) => ({
      label: t(`flow.${x}`),
      value: x,
    }),
  );

  const isSubAgent = useMemo(() => {
    return isBottomSubAgent(edges, node?.id);
  }, [edges, node?.id]);

  const form = useForm<AgentFormSchemaType>({
    defaultValues: defaultValues,
    resolver: zodResolver(FormSchema),
  });

  const llmId = useWatch({ control: form.control, name: 'llm_id' });

  const findLlmByUuid = useFindLlmByUuid();

  const exceptionMethod = useWatch({
    control: form.control,
    name: 'exception_method',
  });

  const {
    initialStructuredOutput,
    showStructuredOutputDialog,
    structuredOutputDialogVisible,
    hideStructuredOutputDialog,
    handleStructuredOutputDialogOk,
  } = useShowStructuredOutputDialog(node?.id);

  useEffect(() => {
    if (exceptionMethod !== AgentExceptionMethod.Goto) {
      if (node?.id) {
        deleteEdgesBySourceAndSourceHandle(
          node?.id,
          NodeHandleId.AgentException,
        );
      }
    }
  }, [deleteEdgesBySourceAndSourceHandle, exceptionMethod, node?.id]);

  useWatchFormChange(node?.id, form);

  return (
    <>
      <Form {...form}>
        <FormWrapper>
          {isSubAgent && <DescriptionField></DescriptionField>}
          <LargeModelFormField showSpeech2TextModel></LargeModelFormField>
          {findLlmByUuid(llmId)?.model_type === LlmModelType.Image2text && (
            <QueryVariable
              name="visual_files_var"
              label="Visual Input File"
              type={VariableType.File}
            ></QueryVariable>
          )}
          <FormField
            control={form.control}
            name={`sys_prompt`}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{t('flow.systemPrompt')}</FormLabel>
                <FormControl>
                  <PromptEditor
                    {...field}
                    placeholder={t('flow.messagePlaceholder')}
                    showToolbar={true}
                    extraOptions={extraOptions}
                  ></PromptEditor>
                </FormControl>
              </FormItem>
            )}
          />
          {isSubAgent || (
            <FormField
              control={form.control}
              name={`prompts`}
              render={({ field }) => (
                <FormItem className="flex-1">
                  <FormLabel>{t('flow.userPrompt')}</FormLabel>
                  <FormControl>
                    <section>
                      <PromptEditor
                        {...field}
                        showToolbar={true}
                      ></PromptEditor>
                    </section>
                  </FormControl>
                </FormItem>
              )}
            />
          )}
          <Separator></Separator>
          <AgentTools></AgentTools>
          <Agents node={node}></Agents>
          <Collapse title={<div>{t('flow.advancedSettings')}</div>}>
            <section className="space-y-5">
              <MessageHistoryWindowSizeFormField></MessageHistoryWindowSizeFormField>
              <FormField
                control={form.control}
                name={`cite`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormLabel tooltip={t('flow.citeTip')}>
                      {t('flow.cite')}
                    </FormLabel>
                    <FormControl>
                      <Switch
                        checked={field.value}
                        onCheckedChange={field.onChange}
                      ></Switch>
                    </FormControl>
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name={`max_retries`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormLabel>{t('flow.maxRetries')}</FormLabel>
                    <FormControl>
                      <NumberInput {...field} max={8} min={0}></NumberInput>
                    </FormControl>
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name={`delay_after_error`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormLabel>{t('flow.delayEfterError')}</FormLabel>
                    <FormControl>
                      <NumberInput {...field} max={5} step={0.1}></NumberInput>
                    </FormControl>
                  </FormItem>
                )}
              />
              {hasSubAgentOrTool(edges, node?.id) && (
                <FormField
                  control={form.control}
                  name={`max_rounds`}
                  render={({ field }) => (
                    <FormItem className="flex-1">
                      <FormLabel>{t('flow.maxRounds')}</FormLabel>
                      <FormControl>
                        <NumberInput {...field} min={0}></NumberInput>
                      </FormControl>
                    </FormItem>
                  )}
                />
              )}
              <FormField
                control={form.control}
                name={`exception_method`}
                render={({ field }) => (
                  <FormItem className="flex-1">
                    <FormLabel>{t('flow.exceptionMethod')}</FormLabel>
                    <FormControl>
                      <SelectWithSearch
                        {...field}
                        options={ExceptionMethodOptions}
                        allowClear
                      />
                    </FormControl>
                  </FormItem>
                )}
              />
              {exceptionMethod === AgentExceptionMethod.Comment && (
                <FormField
                  control={form.control}
                  name={`exception_default_value`}
                  render={({ field }) => (
                    <FormItem className="flex-1">
                      <FormLabel>{t('flow.ExceptionDefaultValue')}</FormLabel>
                      <FormControl>
                        <Input {...field} />
                      </FormControl>
                    </FormItem>
                  )}
                />
              )}
            </section>
          </Collapse>
          <Output list={outputList}></Output>
          <section className="space-y-2">
            <div className="flex justify-between items-center">
              {t('flow.structuredOutput.structuredOutput')}
              <Button variant={'outline'} onClick={showStructuredOutputDialog}>
                {t('flow.structuredOutput.configuration')}
              </Button>
            </div>
            <StructuredOutputPanel
              value={initialStructuredOutput}
            ></StructuredOutputPanel>
          </section>
        </FormWrapper>
      </Form>
      {structuredOutputDialogVisible && (
        <StructuredOutputDialog
          hideModal={hideStructuredOutputDialog}
          onOk={handleStructuredOutputDialogOk}
          initialValues={initialStructuredOutput}
        ></StructuredOutputDialog>
      )}
    </>
  );
}

export default memo(AgentForm);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/agent-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 304 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `AgentForm()`: Function definition
- `ExceptionMethodOptions()`: Function definition
- `isSubAgent()`: Function definition

### Imports (34)

- `import { Collapse } from '@/components/collapse';`
- `import {`
- `import { LlmSettingSchema } from '@/components/llm-setting-items/next';`
- `import { MessageHistoryWindowSizeFormField } from '@/components/message-history-window-size-item';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { Input, NumberInput } from '@/components/ui/input';`
- `import { Separator } from '@/components/ui/separator';`
- `import { Switch } from '@/components/ui/switch';`

## Code Structure Analysis

- Total lines: 304
- Blank lines: 18 (5.9%)
- Comment lines: ~8 (2.6%)
- Code lines: ~278


## Dependencies and Imports

- `@/components/collapse`
- `@/components/llm-setting-items/next`
- `@/components/message-history-window-size-item`
- `@/components/originui/select-with-search`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/separator`
- `@/components/ui/switch`
- `@/constants/knowledge`
- `@/hooks/use-llm-request`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../../interface`
- `../../store`
- `../../utils`
- `../../utils/build-output-list`
- `../components/description-field`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form`.

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

- Other files in `web/src/pages/agent/form/agent-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../../store, ../../utils, ../../utils/build-output-list, ../components/description-field, ../components/form-wrapper, ../components/output, ../components/prompt-editor, ../components/query-variable, ./agent-tools, ./structured-output-dialog, ./structured-output-panel, ./use-build-prompt-options, ./use-show-structured-output-dialog, ./use-values, ./use-watch-change, @/components/collapse, @/components/llm-setting-items/next, @/components/message-history-window-size-item, @/components/originui/select-with-search, @/components/ui/button, @/components/ui/input, @/components/ui/separator, @/components/ui/switch, @/constants/knowledge, @/hooks/use-llm-request, @hookform/resolvers/zod, AgentException, AgentExceptionMethod, AgentForm, AgentFormSchemaType, AgentTools, Agents, Button, Collapse, Comment, DescriptionField, ExceptionDefaultValue, ExceptionMethodOptions, File, Form, FormControl, FormField, FormItem, FormLabel, FormSchema, FormWrapper, Goto, INextOperatorForm, Image2text...

---
*Generated by RAGFlow Repository Documentation Generator*
