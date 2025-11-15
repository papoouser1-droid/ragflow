# File Documentation: web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 170
- **Characters**: 4,702
- **Size**: 4,702 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Form, Input, InputNumber, Modal, Select, Switch } from 'antd';
import omit from 'lodash/omit';

type FieldType = IAddLlmRequestBody & {
  api_version: string;
  vision: boolean;
};

const { Option } = Select;

const AzureOpenAIModal = ({
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
  const optionsMap = {
    Default: [
      { value: 'chat', label: 'chat' },
      { value: 'embedding', label: 'embedding' },
      { value: 'image2text', label: 'image2text' },
    ],
  };
  const getOptions = () => {
    return optionsMap.Default;
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
          initialValue={'embedding'}
          rules={[{ required: true, message: t('modelTypeMessage') }]}
        >
          <Select placeholder={t('modelTypeMessage')}>
            {getOptions(llmFactory).map((option) => (
              <Option key={option.value} value={option.value}>
                {option.label}
              </Option>
            ))}
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addLlmBaseUrl')}
          name="api_base"
          rules={[{ required: true, message: t('baseUrlNameMessage') }]}
        >
          <Input
            placeholder={t('baseUrlNameMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('apiKey')}
          name="api_key"
          rules={[{ required: false, message: t('apiKeyMessage') }]}
        >
          <Input placeholder={t('apiKeyMessage')} onKeyDown={handleKeyDown} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          initialValue="gpt-3.5-turbo"
          rules={[{ required: true, message: t('modelNameMessage') }]}
        >
          <Input
            placeholder={t('modelNameMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('apiVersion')}
          name="api_version"
          initialValue="2024-02-01"
          rules={[{ required: false, message: t('apiVersionMessage') }]}
        >
          <Input
            placeholder={t('apiVersionMessage')}
            onKeyDown={handleKeyDown}
          />
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

        <Form.Item noStyle dependencies={['model_type']}>
          {({ getFieldValue }) =>
            getFieldValue('model_type') === 'chat' && (
              <Form.Item
                label={t('vision')}
                valuePropName="checked"
                name={'vision'}
              >
                <Switch />
              </Form.Item>
            )
          }
        </Form.Item>
      </Form>
    </Modal>
  );
};

export default AzureOpenAIModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 170 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `AzureOpenAIModal()`: Function definition
- `handleOk()`: Function definition
- `getOptions()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (5)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Form, Input, InputNumber, Modal, Select, Switch } from 'antd';`
- `import omit from 'lodash/omit';`

## Code Structure Analysis

- Total lines: 170
- Blank lines: 11 (6.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~159


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `lodash/omit`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/azure-openai-modal`.

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

- Other files in `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, AzureOpenAIModal, Default, Enter, Error, FieldType, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, KeyboardEvent, Modal, Option, Promise, React, Select, Switch, TypeScript, antd, data, getOptions, handleKeyDown, handleOk, lodash/omit, modelType, optionsMap, values

---
*Generated by RAGFlow Repository Documentation Generator*
