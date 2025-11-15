# File Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/tag-feature-item.tsx`
- **Extension**: `.tsx`
- **Lines**: 108
- **Characters**: 3,463
- **Size**: 3,463 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

      // Exclude it's own current data

## Detailed Walkthrough

### Exports (1)

- `TagFeatureItem`: Exported entity

### Functions (5)

- `TagFeatureItem()`: Function definition
- `tagKnowledgeIds()`: Function definition
- `options()`: Function definition
- `filterOptions()`: Function definition
- `list()`: Function definition

### Imports (6)

- `import {`
- `import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';`
- `import { Button, Form, InputNumber, Select } from 'antd';`
- `import { useCallback, useEffect, useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { FormListItem } from '../../utils';`

## Code Structure Analysis

- Total lines: 108
- Blank lines: 11 (10.2%)
- Comment lines: ~2 (1.9%)
- Code lines: ~95


## Dependencies and Imports

- `@ant-design/icons`
- `antd`
- `react`
- `react-i18next`
- `../../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/` directory
- Potential test file: `test_tag-feature-item.tsx`

## Keywords

../../utils, @ant-design/icons, Button, Exclude, FieldKey, Form, FormListItem, InputNumber, Item, List, MinusCircleOutlined, PlusOutlined, Select, TagFeatureItem, TypeScript, ant, antd, filterOptions, form, list, options, react, react-i18next, tagKnowledgeIds, tags

---
*Generated by RAGFlow Repository Documentation Generator*
