# Documentation: web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx`
- **Size**: 4070 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-model/modal/bedrock-modal/index.tsx` is located in the `web/src/pages/user-setting/setting-model/modal/bedrock-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to bedrock-modal.

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
