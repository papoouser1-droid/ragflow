# File Documentation: web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 145
- **Characters**: 4,070
- **Size**: 4,070 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';
import { useMemo } from 'react';
import { BedrockRegionList } from '../../constant';

type FieldType = IAddLlmRequestBody & {
  bedrock_ak: string;
  bedrock_sk: string;
  bedrock_region: string;
};

const { Option } = Select;

const BedrockModal = ({
  visible,
  hideModal,
  onOk,
  loading,
  llmFactory,
}: IModalProps<IAddLlmRequestBody> & { llmFactory: string }) => {
  const [form] = Form.useForm<FieldType>();

  const { t } = useTranslate('setting');
  const options = useMemo(
    () => BedrockRegionList.map((x) => ({ value: x, label: t(x) })),
    [t],
  );

  const handleOk = async () => {
    const values = await form.validateFields();

    const data = {
      ...values,
      llm_factory: llmFactory,
      max_tokens: values.max_tokens,
    };

    onOk?.(data);
  };

  return (
    <Modal
      title={t('addLlmTitle', { name: llmFactory })}
      open={visible}
      onOk={handleOk}
      onCancel={hideModal}
      okButtonProps={{ loading }}
      footer={(originNode: React.ReactNode) => {
        return (
          <Flex justify={'space-between'}>
            <a
              href="https://console.aws.amazon.com/"
              target="_blank"
              rel="noreferrer"
            >
              {t('ollamaLink', { name: llmFactory })}
            </a>
            <Space>{originNode}</Space>
          </Flex>
        );
      }}
    >
      <Form
        name="basic"
        style={{ maxWidth: 600 }}
        autoComplete="off"
        layout={'vertical'}
        form={form}
      >
        <Form.Item<FieldType>
          label={t('modelType')}
          name="model_type"
          initialValue={'chat'}
          rules={[{ required: true, message: t('modelTypeMessage') }]}
        >
          <Select placeholder={t('modelTypeMessage')}>
            <Option value="chat">chat</Option>
            <Option value="embedding">embedding</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('bedrockModelNameMessage') }]}
        >
          <Input placeholder={t('bedrockModelNameMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addBedrockEngineAK')}
          name="bedrock_ak"
          rules={[{ required: true, message: t('bedrockAKMessage') }]}
        >
          <Input placeholder={t('bedrockAKMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addBedrockSK')}
          name="bedrock_sk"
          rules={[{ required: true, message: t('bedrockSKMessage') }]}
        >
          <Input placeholder={t('bedrockSKMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('bedrockRegion')}
          name="bedrock_region"
          rules={[{ required: true, message: t('bedrockRegionMessage') }]}
        >
          <Select
            placeholder={t('bedrockRegionMessage')}
            options={options}
            allowClear
          ></Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('maxTokens')}
          name="max_tokens"
          rules={[
            { required: true, message: t('maxTokensMessage') },
            {
              type: 'number',
              message: t('maxTokensInvalidMessage'),
            },
            ({}) => ({
              validator(_, value) {
                if (value < 0) {
                  return Promise.reject(new Error(t('maxTokensMinMessage')));
                }
                return Promise.resolve();
              },
            }),
          ]}
        >
          <InputNumber
            placeholder={t('maxTokensTip')}
            style={{ width: '100%' }}
          />
        </Form.Item>
      </Form>
    </Modal>
  );
};

export default BedrockModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 145 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `BedrockModal()`: Function definition
- `options()`: Function definition
- `handleOk()`: Function definition

### Imports (6)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';`
- `import { useMemo } from 'react';`
- `import { BedrockRegionList } from '../../constant';`

## Code Structure Analysis

- Total lines: 145
- Blank lines: 10 (6.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~135


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `react`
- `../../constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/bedrock-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/setting-model/modal/bedrock-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, @/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, BedrockModal, BedrockRegionList, Error, FieldType, Flex, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, Modal, Option, Promise, React, ReactNode, Select, Space, TypeScript, antd, data, handleOk, options, react, values

---
*Generated by RAGFlow Repository Documentation Generator*
