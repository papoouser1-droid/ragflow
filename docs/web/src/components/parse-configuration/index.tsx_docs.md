# Documentation: web/src/components/parse-configuration/index.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/index.tsx`
- **Size**: 7431 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/parse-configuration/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/parse-configuration/index.tsx` is located in the `web/src/components/parse-configuration` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parse-configuration.

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

- [graph-rag-form-fields.tsx](graph-rag-form-fields.tsx_docs.md)
- [graph-rag-items.tsx](graph-rag-items.tsx_docs.md)
- [raptor-form-fields-old.tsx](raptor-form-fields-old.tsx_docs.md)
- [raptor-form-fields.tsx](raptor-form-fields.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
