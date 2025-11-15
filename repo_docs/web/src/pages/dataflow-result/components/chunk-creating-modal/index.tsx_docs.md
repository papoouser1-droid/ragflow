# File Documentation: web/src/pages/dataflow-result/components/chunk-creating-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/chunk-creating-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 207
- **Characters**: 6,050
- **Size**: 6,050 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import EditTag from '@/components/edit-tag';
import Divider from '@/components/ui/divider';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/components/ui/hover-card';
import { Modal } from '@/components/ui/modal/modal';
import Space from '@/components/ui/space';
import { Switch } from '@/components/ui/switch';
import { Textarea } from '@/components/ui/textarea';
import { useFetchChunk } from '@/hooks/chunk-hooks';
import { IModalProps } from '@/interfaces/common';
import { Trash2 } from 'lucide-react';
import React, { useCallback, useEffect, useState } from 'react';
import { FieldValues, FormProvider, useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { useDeleteChunkByIds } from '../../hooks';
import {
  transformTagFeaturesArrayToObject,
  transformTagFeaturesObjectToArray,
} from '../../utils';
import { TagFeatureItem } from './tag-feature-item';

interface kFProps {
  doc_id: string;
  chunkId: string | undefined;
  parserId: string;
}

const ChunkCreatingModal: React.FC<IModalProps<any> & kFProps> = ({
  doc_id,
  chunkId,
  hideModal,
  onOk,
  loading,
  parserId,
}) => {
  // const [form] = Form.useForm();
  // const form = useFormContext();
  const form = useForm<FieldValues>({
    defaultValues: {
      content_with_weight: '',
      tag_kwd: [],
      question_kwd: [],
      important_kwd: [],
      tag_feas: [],
    },
  });
  const [checked, setChecked] = useState(false);
  const { removeChunk } = useDeleteChunkByIds();
  const { data } = useFetchChunk(chunkId);
  const { t } = useTranslation();

  const isTagParser = parserId === 'tag';
  const onSubmit = useCallback(
    (values: FieldValues) => {
      onOk?.({
        ...values,
        tag_feas: transformTagFeaturesArrayToObject(values.tag_feas),
        available_int: checked ? 1 : 0,
      });
    },
    [checked, onOk],
  );

  const handleOk = form.handleSubmit(onSubmit);

  const handleRemove = useCallback(() => {
    if (chunkId) {
      return removeChunk([chunkId], doc_id);
    }
  }, [chunkId, doc_id, removeChunk]);

  const handleCheck = useCallback(() => {
    setChecked(!checked);
  }, [checked]);

  useEffect(() => {
    if (data?.code === 0) {
      const { available_int, tag_feas } = data.data;
      form.reset({
        ...data.data,
        tag_feas: transformTagFeaturesObjectToArray(tag_feas),
      });

      setChecked(available_int !== 0);
    }
  }, [data, form, chunkId]);

  return (
    <Modal
      title={`${chunkId ? t('common.edit') : t('common.create')} ${t('chunk.chunk')}`}
      open={true}
      onOk={handleOk}
      onCancel={hideModal}
      confirmLoading={loading}
      destroyOnClose
    >
      <Form {...form}>
        <div className="flex flex-col gap-4">
          <FormField
            control={form.control}
            name="content_with_weight"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('chunk.chunk')}</FormLabel>
                <FormControl>
                  <Textarea {...field} autoSize={{ minRows: 4, maxRows: 10 }} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <FormField
            control={form.control}
            name="important_kwd"
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('chunk.keyword')}</FormLabel>
                <FormControl>
                  <EditTag {...field} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />

          <FormField
            control={form.control}
            name="question_kwd"
            render={({ field }) => (
              <FormItem>
                <FormLabel className="flex justify-start items-start">
                  <div className="flex items-center gap-0">
                    <span>{t('chunk.question')}</span>
                    <HoverCard>
                      <HoverCardTrigger asChild>
                        <span className="text-xs mt-[-3px] text-center scale-[90%] font-thin text-primary cursor-pointer rounded-full w-[16px] h-[16px] border-muted-foreground/50 border">
                          ?
                        </span>
                      </HoverCardTrigger>
                      <HoverCardContent className="w-80" side="top">
                        {t('chunk.questionTip')}
                      </HoverCardContent>
                    </HoverCard>
                  </div>
                </FormLabel>
                <FormControl>
                  <EditTag {...field} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />

          {isTagParser && (
            <FormField
              control={form.control}
              name="tag_kwd"
              render={({ field }) => (
                <FormItem>
                  <FormLabel>{t('knowledgeConfiguration.tagName')}</FormLabel>
                  <FormControl>
                    <EditTag {...field} />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          )}

          {!isTagParser && (
            <FormProvider {...form}>
              <TagFeatureItem />
            </FormProvider>
          )}
        </div>
      </Form>

      {chunkId && (
        <section>
          <Divider />
          <Space size={'large'}>
            <div className="flex items-center gap-2">
              {t('chunk.enabled')}
              <Switch checked={checked} onCheckedChange={handleCheck} />
            </div>
            <div className="flex items-center gap-1" onClick={handleRemove}>
              <Trash2 size={16} /> {t('common.delete')}
            </div>
          </Space>
        </section>
      )}
    </Modal>
  );
};
export default ChunkCreatingModal;

```

## High-Level Overview

  // const [form] = Form.useForm();
  // const form = useFormContext();

## Detailed Walkthrough


### Functions (3)

- `onSubmit()`: Function definition
- `handleRemove()`: Function definition
- `handleCheck()`: Function definition

### Imports (17)

- `import EditTag from '@/components/edit-tag';`
- `import Divider from '@/components/ui/divider';`
- `import {`
- `import {`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import Space from '@/components/ui/space';`
- `import { Switch } from '@/components/ui/switch';`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { useFetchChunk } from '@/hooks/chunk-hooks';`
- `import { IModalProps } from '@/interfaces/common';`

## Code Structure Analysis

- Total lines: 207
- Blank lines: 14 (6.8%)
- Comment lines: ~2 (1.0%)
- Code lines: ~191


## Dependencies and Imports

- `@/components/edit-tag`
- `@/components/ui/divider`
- `@/components/ui/modal/modal`
- `@/components/ui/space`
- `@/components/ui/switch`
- `@/components/ui/textarea`
- `@/hooks/chunk-hooks`
- `@/interfaces/common`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../hooks`
- `./tag-feature-item`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataflow-result/components/chunk-creating-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataflow-result/components/chunk-creating-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks, ./tag-feature-item, @/components/edit-tag, @/components/ui/divider, @/components/ui/modal/modal, @/components/ui/space, @/components/ui/switch, @/components/ui/textarea, @/hooks/chunk-hooks, @/interfaces/common, ChunkCreatingModal, Divider, EditTag, FieldValues, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormProvider, HoverCard, HoverCardContent, HoverCardTrigger, IModalProps, Modal, React, Space, Switch, TagFeatureItem, Textarea, Trash2, TypeScript, form, handleCheck, handleOk, handleRemove, isTagParser, kFProps, lucide-react, onSubmit, react, react-hook-form, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
