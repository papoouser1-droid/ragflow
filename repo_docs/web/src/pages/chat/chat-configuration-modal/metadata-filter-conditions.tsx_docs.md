# File Documentation: web/src/pages/chat/chat-configuration-modal/metadata-filter-conditions.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/metadata-filter-conditions.tsx`
- **Extension**: `.tsx`
- **Lines**: 89
- **Characters**: 2,800
- **Size**: 2,800 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SwitchOperatorOptions } from '@/constants/agent';
import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';
import { useFetchKnowledgeMetadata } from '@/hooks/use-knowledge-request';
import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';
import {
  Button,
  Dropdown,
  Empty,
  Form,
  FormListOperation,
  Input,
  Select,
  Space,
} from 'antd';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';

export function MetadataFilterConditions({ kbIds }: { kbIds: string[] }) {
  const metadata = useFetchKnowledgeMetadata(kbIds);
  const { t } = useTranslation();
  const switchOperatorOptions = useBuildSwitchOperatorOptions();

  const renderItems = useCallback(
    (add: FormListOperation['add']) => {
      if (Object.keys(metadata.data).length === 0) {
        return [{ key: 'noData', label: <Empty></Empty> }];
      }
      return Object.keys(metadata.data).map((key) => {
        return {
          key,
          onClick: () => {
            add({
              key,
              value: '',
              op: SwitchOperatorOptions[0].value,
            });
          },
          label: key,
        };
      });
    },
    [metadata],
  );
  return (
    <Form.List name={['meta_data_filter', 'manual']}>
      {(fields, { add, remove }) => (
        <>
          {fields.map(({ key, name, ...restField }) => (
            <Space
              key={key}
              style={{ display: 'flex', marginBottom: 8 }}
              align="baseline"
            >
              <Form.Item
                {...restField}
                name={[name, 'key']}
                rules={[{ required: true, message: t('common.pleaseInput') }]}
              >
                <Input placeholder={t('common.pleaseInput')} />
              </Form.Item>
              <Form.Item {...restField} name={[name, 'op']} className="w-20">
                <Select
                  options={switchOperatorOptions}
                  popupMatchSelectWidth={false}
                />
              </Form.Item>
              <Form.Item
                {...restField}
                name={[name, 'value']}
                rules={[{ required: true, message: t('common.pleaseInput') }]}
              >
                <Input placeholder={t('common.pleaseInput')} />
              </Form.Item>
              <MinusCircleOutlined onClick={() => remove(name)} />
            </Space>
          ))}
          <Form.Item>
            <Dropdown trigger={['click']} menu={{ items: renderItems(add) }}>
              <Button type="dashed" block icon={<PlusOutlined />}>
                {t('chat.addCondition')}
              </Button>
            </Dropdown>
          </Form.Item>
        </>
      )}
    </Form.List>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chat/chat-configuration-modal/metadata-filter-conditions.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 89 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `MetadataFilterConditions`: Exported entity

### Functions (2)

- `MetadataFilterConditions()`: Function definition
- `renderItems()`: Function definition

### Imports (7)

- `import { SwitchOperatorOptions } from '@/constants/agent';`
- `import { useBuildSwitchOperatorOptions } from '@/hooks/logic-hooks/use-build-operator-options';`
- `import { useFetchKnowledgeMetadata } from '@/hooks/use-knowledge-request';`
- `import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';`
- `import {`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 89
- Blank lines: 3 (3.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~86


## Dependencies and Imports

- `@/constants/agent`
- `@/hooks/logic-hooks/use-build-operator-options`
- `@/hooks/use-knowledge-request`
- `@ant-design/icons`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/chat-configuration-modal`.

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

- Other files in `web/src/pages/chat/chat-configuration-modal/` directory
- Potential test file: `test_metadata-filter-conditions.tsx`

## Keywords

@/constants/agent, @/hooks/logic-hooks/use-build-operator-options, @/hooks/use-knowledge-request, @ant-design/icons, Button, Dropdown, Empty, Form, FormListOperation, Input, Item, List, MetadataFilterConditions, MinusCircleOutlined, Object, PlusOutlined, Select, Space, SwitchOperatorOptions, TypeScript, ant, metadata, react, react-i18next, renderItems, switchOperatorOptions

---
*Generated by RAGFlow Repository Documentation Generator*
