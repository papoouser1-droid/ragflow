# Documentation: web/src/pages/dataset/dataset-setting/components/tag-item.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/tag-item.tsx`
- **Size**: 3663 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-setting/components/tag-item.tsx`.

## Original Source Code

```tsx
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { SliderInputFormField } from '@/components/slider-input-form-field';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { MultiSelect } from '@/components/ui/multi-select';
import { FormLayout } from '@/constants/form';
import { useFetchKnowledgeList } from '@/hooks/knowledge-hooks';
import { Form, Select, Space } from 'antd';
import DOMPurify from 'dompurify';
import { useFormContext, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';

export const TagSetItem = () => {
  const { t } = useTranslation();
  const form = useFormContext();

  const { list: knowledgeList } = useFetchKnowledgeList(true);

  const knowledgeOptions = knowledgeList
    .filter((x) => x.parser_id === 'tag')
    .map((x) => ({
      label: x.name,
      value: x.id,
      icon: () => (
        <Space>
          <RAGFlowAvatar
            name={x.name}
            avatar={x.avatar}
            className="size-4"
          ></RAGFlowAvatar>
        </Space>
      ),
    }));

  return (
    <FormField
      control={form.control}
      name="parser_config.tag_kb_ids"
      render={({ field }) => (
        <FormItem className=" items-center space-y-0 ">
          <div className="flex items-center">
            <FormLabel
              className="text-sm text-text-secondary whitespace-nowrap w-1/4"
              tooltip={
                <div
                  dangerouslySetInnerHTML={{
                    __html: DOMPurify.sanitize(
                      t('knowledgeConfiguration.tagSetTip'),
                    ),
                  }}
                ></div>
              }
            >
              {t('knowledgeConfiguration.tagSet')}
            </FormLabel>
            <div className="w-3/4">
              <FormControl>
                <MultiSelect
                  options={knowledgeOptions}
                  onValueChange={field.onChange}
                  placeholder={t('chat.knowledgeBasesMessage')}
                  variant="inverted"
                  maxCount={10}
                  {...field}
                />
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className="w-1/4"></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );

  return (
    <Form.Item
      label={t('knowledgeConfiguration.tagSet')}
      name={['parser_config', 'tag_kb_ids']}
      tooltip={
        <div
          dangerouslySetInnerHTML={{
            __html: DOMPurify.sanitize(t('knowledgeConfiguration.tagSetTip')),
          }}
        ></div>
      }
      rules={[
        {
          message: t('chat.knowledgeBasesMessage'),
          type: 'array',
        },
      ]}
    >
      <Select
        mode="multiple"
        options={knowledgeOptions}
        placeholder={t('chat.knowledgeBasesMessage')}
      ></Select>
    </Form.Item>
  );
};

export const TopNTagsItem = () => {
  const { t } = useTranslation();

  return (
    <SliderInputFormField
      name={'parser_config.topn_tags'}
      label={t('knowledgeConfiguration.topnTags')}
      max={10}
      min={1}
      defaultValue={3}
      layout={FormLayout.Horizontal}
    ></SliderInputFormField>
  );
};

export function TagItems() {
  const form = useFormContext();
  const ids: string[] = useWatch({
    control: form.control,
    name: 'parser_config.tag_kb_ids',
  });

  return (
    <>
      <TagSetItem></TagSetItem>
      {Array.isArray(ids) && ids.length > 0 && <TopNTagsItem></TopNTagsItem>}
    </>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-setting/components/tag-item.tsx` is located in the `web/src/pages/dataset/dataset-setting/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [added-source-card.tsx](added-source-card.tsx_docs.md)
- [link-data-pipeline.tsx](link-data-pipeline.tsx_docs.md)
- [link-data-pipline-modal.tsx](link-data-pipline-modal.tsx_docs.md)
- [link-data-source-modal.tsx](link-data-source-modal.tsx_docs.md)
- [link-data-source.tsx](link-data-source.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
