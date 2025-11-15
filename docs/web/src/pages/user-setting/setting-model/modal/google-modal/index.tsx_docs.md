# Documentation: web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx`
- **Size**: 3731 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx`.

## Original Source Code

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Form, Input, InputNumber, Modal, Select } from 'antd';

type FieldType = IAddLlmRequestBody & {
  google_project_id: string;
  google_region: string;
  google_service_account_key: string;
};

const { Option } = Select;

const GoogleModal = ({
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

    const data = {
      ...values,
      llm_factory: llmFactory,
      max_tokens: values.max_tokens,
    };

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
    >
      <Form form={form}>
        <Form.Item<FieldType>
          label={t('modelType')}
          name="model_type"
          initialValue={'chat'}
          rules={[{ required: true, message: t('modelTypeMessage') }]}
        >
          <Select placeholder={t('modelTypeMessage')}>
            <Option value="chat">chat</Option>
            <Option value="image2text">image2text</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelID')}
          name="llm_name"
          rules={[{ required: true, message: t('GoogleModelIDMessage') }]}
        >
          <Input
            placeholder={t('GoogleModelIDMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addGoogleProjectID')}
          name="google_project_id"
          rules={[{ required: true, message: t('GoogleProjectIDMessage') }]}
        >
          <Input
            placeholder={t('GoogleProjectIDMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addGoogleRegion')}
          name="google_region"
          rules={[{ required: true, message: t('GoogleRegionMessage') }]}
        >
          <Input
            placeholder={t('GoogleRegionMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addGoogleServiceAccountKey')}
          name="google_service_account_key"
          rules={[
            { required: true, message: t('GoogleServiceAccountKeyMessage') },
          ]}
        >
          <Input
            placeholder={t('GoogleServiceAccountKeyMessage')}
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
      </Form>
    </Modal>
  );
};

export default GoogleModal;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx` is located in the `web/src/pages/user-setting/setting-model/modal/google-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to google-modal.

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
