# File Documentation: web/src/pages/dataset/dataset-setting/components/link-data-pipline-modal.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/link-data-pipline-modal.tsx`
- **Extension**: `.tsx`
- **Lines**: 164
- **Characters**: 5,539
- **Size**: 5,539 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  DataFlowSelect,
  IDataPipelineSelectNode,
} from '@/components/data-pipeline-select';
import { Button } from '@/components/ui/button';
import { Form } from '@/components/ui/form';
import { Modal } from '@/components/ui/modal/modal';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { zodResolver } from '@hookform/resolvers/zod';
import { t } from 'i18next';
import { useState } from 'react';
import { useForm } from 'react-hook-form';
import { z } from 'zod';
import { pipelineFormSchema } from '../form-schema';
import { IDataPipelineNodeProps } from './link-data-pipeline';

const LinkDataPipelineModal = ({
  data,
  open,
  setOpen,
  onSubmit,
}: {
  data: IDataPipelineNodeProps | undefined;
  open: boolean;
  setOpen: (open: boolean) => void;
  onSubmit?: (pipeline: IDataPipelineSelectNode | undefined) => void;
}) => {
  const isEdit = !!data;
  const [list, setList] = useState<IDataPipelineSelectNode[]>();
  const form = useForm<z.infer<typeof pipelineFormSchema>>({
    resolver: zodResolver(pipelineFormSchema),
    defaultValues: {
      pipeline_id: '',
      set_default: false,
      file_filter: '',
    },
  });
  //   const [open, setOpen] = useState(false);
  const { navigateToAgents } = useNavigatePage();
  const handleFormSubmit = (values: any) => {
    console.log(values, data);
    // const param = {
    //   ...data,
    //   ...values,
    // };
    const pipeline = list?.find((item) => item.id === values.pipeline_id);
    onSubmit?.(pipeline);
  };
  return (
    <Modal
      className="!w-[560px]"
      title={
        !isEdit
          ? t('knowledgeConfiguration.linkDataPipeline')
          : t('knowledgeConfiguration.eidtLinkDataPipeline')
      }
      open={open}
      onOpenChange={setOpen}
      showfooter={false}
    >
      <Form {...form}>
        <form onSubmit={form.handleSubmit(handleFormSubmit)}>
          <div className="flex flex-col gap-4 ">
            {!isEdit && (
              <DataFlowSelect
                toDataPipeline={navigateToAgents}
                formFieldName="pipeline_id"
                setDataList={setList}
              />
            )}
            {/* <FormField
              control={form.control}
              name={'file_filter'}
              render={({ field }) => (
                <FormItem className=" items-center space-y-0 ">
                  <div className="flex flex-col gap-1">
                    <div className="flex gap-2 justify-between ">
                      <FormLabel
                        tooltip={t('knowledgeConfiguration.fileFilterTip')}
                        className="text-sm text-text-primary whitespace-wrap "
                      >
                        {t('knowledgeConfiguration.fileFilter')}
                      </FormLabel>
                    </div>

                    <div className="text-muted-foreground">
                      <FormControl>
                        <Input
                          placeholder={t(
                            'knowledgeConfiguration.filterPlaceholder',
                          )}
                          {...field}
                        />
                      </FormControl>
                    </div>
                  </div>
                  <div className="flex pt-1">
                    <div className="w-full"></div>
                    <FormMessage />
                  </div>
                </FormItem>
              )}
            />
            {isEdit && (
              <FormField
                control={form.control}
                name={'set_default'}
                render={({ field }) => (
                  <FormItem className=" items-center space-y-0 ">
                    <div className="flex flex-col gap-1">
                      <div className="flex gap-2 justify-between ">
                        <FormLabel
                          tooltip={t('knowledgeConfiguration.setDefaultTip')}
                          className="text-sm text-text-primary whitespace-wrap "
                        >
                          {t('knowledgeConfiguration.setDefault')}
                        </FormLabel>
                      </div>

                      <div className="text-muted-foreground">
                        <FormControl>
                          <Switch
                            value={field.value}
                            onCheckedChange={field.onChange}
                          />
                        </FormControl>
                      </div>
                    </div>
                    <div className="flex pt-1">
                      <div className="w-full"></div>
                      <FormMessage />
                    </div>
                  </FormItem>
                )}
              />
            )} */}
            <div className="flex justify-end gap-1">
              <Button
                type="button"
                variant={'outline'}
                className="btn-primary"
                onClick={() => {
                  setOpen(false);
                }}
              >
                {t('modal.cancelText')}
              </Button>
              <Button
                type="button"
                variant={'default'}
                className="btn-primary"
                onClick={form.handleSubmit(handleFormSubmit)}
              >
                {t('modal.okText')}
              </Button>
            </div>
          </div>
        </form>
      </Form>
    </Modal>
  );
};
export default LinkDataPipelineModal;

```

## High-Level Overview

  //   const [open, setOpen] = useState(false);
    // const param = {
    //   ...data,
    //   ...values,
    // };

## Detailed Walkthrough


### Functions (3)

- `LinkDataPipelineModal()`: Function definition
- `handleFormSubmit()`: Function definition
- `pipeline()`: Function definition

### Imports (12)

- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { Form } from '@/components/ui/form';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { t } from 'i18next';`
- `import { useState } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`

## Code Structure Analysis

- Total lines: 164
- Blank lines: 4 (2.4%)
- Comment lines: ~5 (3.0%)
- Code lines: ~155


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/form`
- `@/components/ui/modal/modal`
- `@/hooks/logic-hooks/navigate-hooks`
- `@hookform/resolvers/zod`
- `i18next`
- `react`
- `react-hook-form`
- `zod`
- `../form-schema`
- `./link-data-pipeline`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset-setting/components/` directory
- Potential test file: `test_link-data-pipline-modal.tsx`

## Keywords

../form-schema, ./link-data-pipeline, @/components/ui/button, @/components/ui/form, @/components/ui/modal/modal, @/hooks/logic-hooks/navigate-hooks, @hookform/resolvers/zod, Button, DataFlowSelect, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, IDataPipelineNodeProps, IDataPipelineSelectNode, Input, LinkDataPipelineModal, Modal, Switch, TypeScript, form, handleFormSubmit, hookform, i18next, isEdit, param, pipeline, react, react-hook-form, zod

---
*Generated by RAGFlow Repository Documentation Generator*
