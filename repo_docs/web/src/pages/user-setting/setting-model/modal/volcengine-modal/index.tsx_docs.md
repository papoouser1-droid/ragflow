# File Documentation: web/src/pages/user-setting/setting-model/modal/volcengine-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/volcengine-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 138
- **Characters**: 3,860
- **Size**: 3,860 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';
import omit from 'lodash/omit';

type FieldType = IAddLlmRequestBody & {
  vision: boolean;
  volc_ak: string;
  volc_sk: string;
  endpoint_id: string;
  ark_api_key: string;
};

const { Option } = Select;

const VolcEngineModal = ({
  visible,
  hideModal,
  onOk,
  loading,
  llmFactory,
}: IModalProps<IAddLlmRequestBody> & { llmFactory: string }) => {
  const [form] = Form.useForm<FieldType>();

  const { t } = useTranslate('setting');

  const handleOk = async () => {
    const values = await form.validateFields();
    const modelType =
      values.model_type === 'chat' && values.vision
        ? 'image2text'
        : values.model_type;

    const data = {
      ...omit(values, ['vision']),
      model_type: modelType,
      llm_factory: llmFactory,
      max_tokens: values.max_tokens,
    };
    console.info(data);

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
              href="https://www.volcengine.com/docs/82379/1302008"
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
            <Option value="image2text">image2text</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('volcModelNameMessage') }]}
        >
          <Input placeholder={t('volcModelNameMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addEndpointID')}
          name="endpoint_id"
          rules={[{ required: true, message: t('endpointIDMessage') }]}
        >
          <Input placeholder={t('endpointIDMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addArkApiKey')}
          name="ark_api_key"
          rules={[{ required: true, message: t('ArkApiKeyMessage') }]}
        >
          <Input placeholder={t('ArkApiKeyMessage')} />
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

export default VolcEngineModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/volcengine-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 138 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `VolcEngineModal()`: Function definition
- `handleOk()`: Function definition

### Imports (5)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';`
- `import omit from 'lodash/omit';`

## Code Structure Analysis

- Total lines: 138
- Blank lines: 10 (7.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~128


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `lodash/omit`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/volcengine-modal`.

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

- Other files in `web/src/pages/user-setting/setting-model/modal/volcengine-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, ArkApiKeyMessage, Error, FieldType, Flex, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, Modal, Option, Promise, React, ReactNode, Select, Space, TypeScript, VolcEngineModal, antd, data, handleOk, lodash/omit, modelType, values

---
*Generated by RAGFlow Repository Documentation Generator*
