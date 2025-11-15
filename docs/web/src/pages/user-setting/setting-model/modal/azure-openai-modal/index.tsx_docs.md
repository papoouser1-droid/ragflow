# Documentation: web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx`
- **Size**: 4702 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-model/modal/azure-openai-modal/index.tsx` is located in the `web/src/pages/user-setting/setting-model/modal/azure-openai-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to azure-openai-modal.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
