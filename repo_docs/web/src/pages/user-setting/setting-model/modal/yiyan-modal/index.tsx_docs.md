# File Documentation: web/src/pages/user-setting/setting-model/modal/yiyan-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/yiyan-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 132
- **Characters**: 3,619
- **Size**: 3,619 bytes
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
  yiyan_ak: string;
  yiyan_sk: string;
};

const { Option } = Select;

const YiyanModal = ({
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
            <Option value="rerank">rerank</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('yiyanModelNameMessage') }]}
        >
          <Input
            placeholder={t('yiyanModelNameMessage')}
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addyiyanAK')}
          name="yiyan_ak"
          rules={[{ required: true, message: t('yiyanAKMessage') }]}
        >
          <Input placeholder={t('yiyanAKMessage')} onKeyDown={handleKeyDown} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addyiyanSK')}
          name="yiyan_sk"
          rules={[{ required: true, message: t('yiyanSKMessage') }]}
        >
          <Input placeholder={t('yiyanSKMessage')} onKeyDown={handleKeyDown} />
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

export default YiyanModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/yiyan-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 132 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `YiyanModal()`: Function definition
- `handleOk()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (5)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Form, Input, InputNumber, Modal, Select } from 'antd';`
- `import omit from 'lodash/omit';`

## Code Structure Analysis

- Total lines: 132
- Blank lines: 11 (8.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~121


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `lodash/omit`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/yiyan-modal`.

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

- Other files in `web/src/pages/user-setting/setting-model/modal/yiyan-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, Enter, Error, FieldType, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, KeyboardEvent, Modal, Option, Promise, React, Select, TypeScript, YiyanModal, antd, data, handleKeyDown, handleOk, lodash/omit, modelType, values

---
*Generated by RAGFlow Repository Documentation Generator*
