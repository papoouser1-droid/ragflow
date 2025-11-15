# File Documentation: web/src/components/parse-configuration/index.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 218
- **Characters**: 7,431
- **Size**: 7,431 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { DocumentParserType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { PlusOutlined } from '@ant-design/icons';
import { Button, Flex, Form, Input, InputNumber, Slider, Switch } from 'antd';
import random from 'lodash/random';

export const excludedParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.One,
  DocumentParserType.Picture,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Qa,
  DocumentParserType.Tag,
];

export const showRaptorParseConfiguration = (
  parserId: DocumentParserType | undefined,
) => {
  return !excludedParseMethods.some((x) => x === parserId);
};

export const excludedTagParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const showTagItems = (parserId: DocumentParserType) => {
  return !excludedTagParseMethods.includes(parserId);
};

// The three types "table", "resume" and "one" do not display this configuration.
const ParseConfiguration = () => {
  const form = Form.useFormInstance();
  const { t } = useTranslate('knowledgeConfiguration');

  const handleGenerate = () => {
    form.setFieldValue(
      ['parser_config', 'raptor', 'random_seed'],
      random(10000),
    );
  };

  return (
    <>
      <Form.Item
        name={['parser_config', 'raptor', 'use_raptor']}
        label={t('useRaptor')}
        initialValue={false}
        valuePropName="checked"
        tooltip={t('useRaptorTip')}
      >
        <Switch />
      </Form.Item>
      <Form.Item
        shouldUpdate={(prevValues, curValues) =>
          prevValues.parser_config.raptor.use_raptor !==
          curValues.parser_config.raptor.use_raptor
        }
      >
        {({ getFieldValue }) => {
          const useRaptor = getFieldValue([
            'parser_config',
            'raptor',
            'use_raptor',
          ]);

          return (
            useRaptor && (
              <>
                <Form.Item
                  name={['parser_config', 'raptor', 'prompt']}
                  label={t('prompt')}
                  initialValue={t('promptText')}
                  tooltip={t('promptTip')}
                  rules={[
                    {
                      required: true,
                      message: t('promptMessage'),
                    },
                  ]}
                >
                  <Input.TextArea rows={8} />
                </Form.Item>
                <Form.Item label={t('maxToken')} tooltip={t('maxTokenTip')}>
                  <Flex gap={20} align="center">
                    <Flex flex={1}>
                      <Form.Item
                        name={['parser_config', 'raptor', 'max_token']}
                        noStyle
                        initialValue={256}
                        rules={[
                          {
                            required: true,
                            message: t('maxTokenMessage'),
                          },
                        ]}
                      >
                        <Slider max={2048} style={{ width: '100%' }} />
                      </Form.Item>
                    </Flex>
                    <Form.Item
                      name={['parser_config', 'raptor', 'max_token']}
                      noStyle
                      rules={[
                        {
                          required: true,
                          message: t('maxTokenMessage'),
                        },
                      ]}
                    >
                      <InputNumber max={2048} min={0} />
                    </Form.Item>
                  </Flex>
                </Form.Item>
                <Form.Item label={t('threshold')} tooltip={t('thresholdTip')}>
                  <Flex gap={20} align="center">
                    <Flex flex={1}>
                      <Form.Item
                        name={['parser_config', 'raptor', 'threshold']}
                        noStyle
                        initialValue={0.1}
                        rules={[
                          {
                            required: true,
                            message: t('thresholdMessage'),
                          },
                        ]}
                      >
                        <Slider
                          min={0}
                          max={1}
                          style={{ width: '100%' }}
                          step={0.01}
                        />
                      </Form.Item>
                    </Flex>
                    <Form.Item
                      name={['parser_config', 'raptor', 'threshold']}
                      noStyle
                      rules={[
                        {
                          required: true,
                          message: t('thresholdMessage'),
                        },
                      ]}
                    >
                      <InputNumber max={1} min={0} step={0.01} />
                    </Form.Item>
                  </Flex>
                </Form.Item>
                <Form.Item label={t('maxCluster')} tooltip={t('maxClusterTip')}>
                  <Flex gap={20} align="center">
                    <Flex flex={1}>
                      <Form.Item
                        name={['parser_config', 'raptor', 'max_cluster']}
                        noStyle
                        initialValue={64}
                        rules={[
                          {
                            required: true,
                            message: t('maxClusterMessage'),
                          },
                        ]}
                      >
                        <Slider min={1} max={1024} style={{ width: '100%' }} />
                      </Form.Item>
                    </Flex>
                    <Form.Item
                      name={['parser_config', 'raptor', 'max_cluster']}
                      noStyle
                      rules={[
                        {
                          required: true,
                          message: t('maxClusterMessage'),
                        },
                      ]}
                    >
                      <InputNumber max={1024} min={1} />
                    </Form.Item>
                  </Flex>
                </Form.Item>
                <Form.Item label={t('randomSeed')}>
                  <Flex gap={20} align="center">
                    <Flex flex={1}>
                      <Form.Item
                        name={['parser_config', 'raptor', 'random_seed']}
                        noStyle
                        initialValue={0}
                        rules={[
                          {
                            required: true,
                            message: t('randomSeedMessage'),
                          },
                        ]}
                      >
                        <InputNumber style={{ width: '100%' }} />
                      </Form.Item>
                    </Flex>
                    <Form.Item noStyle>
                      <Button type="primary" onClick={handleGenerate}>
                        <PlusOutlined />
                      </Button>
                    </Form.Item>
                  </Flex>
                </Form.Item>
              </>
            )
          );
        }}
      </Form.Item>
    </>
  );
};

export default ParseConfiguration;

```

## High-Level Overview

// The three types "table", "resume" and "one" do not display this configuration.

## Detailed Walkthrough

### Exports (4)

- `excludedParseMethods`: Exported entity
- `showRaptorParseConfiguration`: Exported entity
- `excludedTagParseMethods`: Exported entity
- `showTagItems`: Exported entity

### Functions (4)

- `showRaptorParseConfiguration()`: Function definition
- `showTagItems()`: Function definition
- `ParseConfiguration()`: Function definition
- `handleGenerate()`: Function definition

### Imports (5)

- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { PlusOutlined } from '@ant-design/icons';`
- `import { Button, Flex, Form, Input, InputNumber, Slider, Switch } from 'antd';`
- `import random from 'lodash/random';`

## Code Structure Analysis

- Total lines: 218
- Blank lines: 10 (4.6%)
- Comment lines: ~1 (0.5%)
- Code lines: ~207


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@ant-design/icons`
- `antd`
- `lodash/random`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/parse-configuration`.

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

- Other files in `web/src/components/parse-configuration/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/constants/knowledge, @/hooks/common-hooks, @ant-design/icons, Button, DocumentParserType, Flex, Form, Input, InputNumber, Item, KnowledgeGraph, One, ParseConfiguration, Picture, PlusOutlined, Resume, Slider, Switch, Table, Tag, TextArea, The, TypeScript, ant, antd, excludedParseMethods, excludedTagParseMethods, form, handleGenerate, lodash/random, showRaptorParseConfiguration, showTagItems, useRaptor

---
*Generated by RAGFlow Repository Documentation Generator*
