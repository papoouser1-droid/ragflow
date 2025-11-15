# Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx`
- **Size**: 3463 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx`.

## Original Source Code

```tsx
import {
  useFetchKnowledgeBaseConfiguration,
  useFetchTagListByKnowledgeIds,
} from '@/hooks/knowledge-hooks';
import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';
import { Button, Form, InputNumber, Select } from 'antd';
import { useCallback, useEffect, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { FormListItem } from '../../utils';

const FieldKey = 'tag_feas';

export const TagFeatureItem = () => {
  const form = Form.useFormInstance();
  const { t } = useTranslation();
  const { data: knowledgeConfiguration } = useFetchKnowledgeBaseConfiguration();

  const { setKnowledgeIds, list } = useFetchTagListByKnowledgeIds();

  const tagKnowledgeIds = useMemo(() => {
    return knowledgeConfiguration?.parser_config?.tag_kb_ids ?? [];
  }, [knowledgeConfiguration?.parser_config?.tag_kb_ids]);

  const options = useMemo(() => {
    return list.map((x) => ({
      value: x[0],
      label: x[0],
    }));
  }, [list]);

  const filterOptions = useCallback(
    (index: number) => {
      const tags: FormListItem[] = form.getFieldValue(FieldKey) ?? [];

      // Exclude it's own current data
      const list = tags
        .filter((x, idx) => x && index !== idx)
        .map((x) => x.tag);

      // Exclude the selected data from other options from one's own options.
      return options.filter((x) => !list.some((y) => x.value === y));
    },
    [form, options],
  );

  useEffect(() => {
    setKnowledgeIds(tagKnowledgeIds);
  }, [setKnowledgeIds, tagKnowledgeIds]);

  return (
    <Form.Item label={t('knowledgeConfiguration.tags')}>
      <Form.List name={FieldKey} initialValue={[]}>
        {(fields, { add, remove }) => (
          <>
            {fields.map(({ key, name, ...restField }) => (
              <div key={key} className="flex gap-3 items-center">
                <div className="flex flex-1  gap-8">
                  <Form.Item
                    {...restField}
                    name={[name, 'tag']}
                    rules={[
                      { required: true, message: t('common.pleaseSelect') },
                    ]}
                    className="w-2/3"
                  >
                    <Select
                      showSearch
                      placeholder={t('knowledgeConfiguration.tagName')}
                      options={filterOptions(name)}
                    />
                  </Form.Item>
                  <Form.Item
                    {...restField}
                    name={[name, 'frequency']}
                    rules={[
                      { required: true, message: t('common.pleaseInput') },
                    ]}
                  >
                    <InputNumber
                      placeholder={t('knowledgeConfiguration.frequency')}
                      max={10}
                      min={0}
                    />
                  </Form.Item>
                </div>
                <MinusCircleOutlined
                  onClick={() => remove(name)}
                  className="mb-6"
                />
              </div>
            ))}
            <Form.Item>
              <Button
                type="dashed"
                onClick={() => add()}
                block
                icon={<PlusOutlined />}
              >
                {t('knowledgeConfiguration.addTag')}
              </Button>
            </Form.Item>
          </>
        )}
      </Form.List>
    </Form.Item>
  );
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-creating-modal.

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

- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
