# File Documentation: web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 134
- **Characters**: 3,731
- **Size**: 3,731 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/google-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 134 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `GoogleModal()`: Function definition
- `handleOk()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (4)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Form, Input, InputNumber, Modal, Select } from 'antd';`

## Code Structure Analysis

- Total lines: 134
- Blank lines: 10 (7.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~124


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/google-modal`.

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

- Other files in `web/src/pages/user-setting/setting-model/modal/google-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, Enter, Error, FieldType, Form, GoogleModal, GoogleModelIDMessage, GoogleProjectIDMessage, GoogleRegionMessage, GoogleServiceAccountKeyMessage, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, KeyboardEvent, Modal, Option, Promise, React, Select, TypeScript, antd, data, handleKeyDown, handleOk, values

---
*Generated by RAGFlow Repository Documentation Generator*
