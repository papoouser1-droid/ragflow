# File Documentation: web/src/pages/user-setting/setting-model/modal/spark-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/spark-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 164
- **Characters**: 4,818
- **Size**: 4,818 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Form, Input, InputNumber, Modal, Select } from 'antd';
import omit from 'lodash/omit';

type FieldType = IAddLlmRequestBody & {
  vision: boolean;
  spark_api_password: string;
  spark_app_id: string;
  spark_api_secret: string;
  spark_api_key: string;
};

const { Option } = Select;

const SparkModal = ({
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

  const handleKeyDown = async (e: React.KeyboardEvent) => {
    if (e.key === 'Enter') {
      await handleOk();
    }
  };

  return (
    <Modal
      title={t('addLlmTitle', { name: llmFactory })}
      open={visible}
      onOk={handleOk}
      onCancel={hideModal}
      okButtonProps={{ loading }}
      confirmLoading={loading}
    >
      <Form>
        <Form.Item<FieldType>
          label={t('modelType')}
          name="model_type"
          initialValue={'chat'}
          rules={[{ required: true, message: t('modelTypeMessage') }]}
        >
          <Select placeholder={t('modelTypeMessage')}>
            <Option value="chat">chat</Option>
            <Option value="tts">tts</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('SparkModelNameMessage') }]}
        >
          <Input
            placeholder={t('modelNameMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addSparkAPIPassword')}
          name="spark_api_password"
          rules={[{ required: true, message: t('SparkAPIPasswordMessage') }]}
        >
          <Input
            placeholder={t('SparkAPIPasswordMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item noStyle dependencies={['model_type']}>
          {({ getFieldValue }) =>
            getFieldValue('model_type') === 'tts' && (
              <Form.Item<FieldType>
                label={t('addSparkAPPID')}
                name="spark_app_id"
                rules={[{ required: true, message: t('SparkAPPIDMessage') }]}
              >
                <Input placeholder={t('SparkAPPIDMessage')} />
              </Form.Item>
            )
          }
        </Form.Item>
        <Form.Item noStyle dependencies={['model_type']}>
          {({ getFieldValue }) =>
            getFieldValue('model_type') === 'tts' && (
              <Form.Item<FieldType>
                label={t('addSparkAPISecret')}
                name="spark_api_secret"
                rules={[
                  { required: true, message: t('SparkAPISecretMessage') },
                ]}
              >
                <Input placeholder={t('SparkAPISecretMessage')} />
              </Form.Item>
            )
          }
        </Form.Item>
        <Form.Item noStyle dependencies={['model_type']}>
          {({ getFieldValue }) =>
            getFieldValue('model_type') === 'tts' && (
              <Form.Item<FieldType>
                label={t('addSparkAPIKey')}
                name="spark_api_key"
                rules={[{ required: true, message: t('SparkAPIKeyMessage') }]}
              >
                <Input placeholder={t('SparkAPIKeyMessage')} />
              </Form.Item>
            )
          }
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

export default SparkModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/spark-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 164 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `SparkModal()`: Function definition
- `handleOk()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (5)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Form, Input, InputNumber, Modal, Select } from 'antd';`
- `import omit from 'lodash/omit';`

## Code Structure Analysis

- Total lines: 164
- Blank lines: 11 (6.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~153


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `lodash/omit`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/spark-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/setting-model/modal/spark-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, Enter, Error, FieldType, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, KeyboardEvent, Modal, Option, Promise, React, Select, SparkAPIKeyMessage, SparkAPIPasswordMessage, SparkAPISecretMessage, SparkAPPIDMessage, SparkModal, SparkModelNameMessage, TypeScript, antd, data, handleKeyDown, handleOk, lodash/omit, modelType, values

---
*Generated by RAGFlow Repository Documentation Generator*
