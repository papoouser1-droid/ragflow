# File Documentation: web/src/pages/user-setting/setting-model/modal/fish-audio-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/fish-audio-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 127
- **Characters**: 3,559
- **Size**: 3,559 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';
import omit from 'lodash/omit';

type FieldType = IAddLlmRequestBody & {
  fish_audio_ak: string;
  fish_audio_refid: string;
};

const { Option } = Select;

const FishAudioModal = ({
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
    const modelType = values.model_type;

    const data = {
      ...omit(values),
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
            <a href={`https://fish.audio`} target="_blank" rel="noreferrer">
              {t('FishAudioLink')}
            </a>
            <Space>{originNode}</Space>
          </Flex>
        );
      }}
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
          initialValue={'tts'}
          rules={[{ required: true, message: t('modelTypeMessage') }]}
        >
          <Select placeholder={t('modelTypeMessage')}>
            <Option value="tts">tts</Option>
          </Select>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('FishAudioModelNameMessage') }]}
        >
          <Input placeholder={t('FishAudioModelNameMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addFishAudioAK')}
          name="fish_audio_ak"
          rules={[{ required: true, message: t('FishAudioAKMessage') }]}
        >
          <Input placeholder={t('FishAudioAKMessage')} />
        </Form.Item>
        <Form.Item<FieldType>
          label={t('addFishAudioRefID')}
          name="fish_audio_refid"
          rules={[{ required: true, message: t('FishAudioRefIDMessage') }]}
        >
          <Input placeholder={t('FishAudioRefIDMessage')} />
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

export default FishAudioModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/fish-audio-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 127 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `FishAudioModal()`: Function definition
- `handleOk()`: Function definition

### Imports (5)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import { Flex, Form, Input, InputNumber, Modal, Select, Space } from 'antd';`
- `import omit from 'lodash/omit';`

## Code Structure Analysis

- Total lines: 127
- Blank lines: 10 (7.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~117


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `antd`
- `lodash/omit`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/fish-audio-modal`.

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

- Other files in `web/src/pages/user-setting/setting-model/modal/fish-audio-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, Error, FieldType, FishAudioAKMessage, FishAudioLink, FishAudioModal, FishAudioModelNameMessage, FishAudioRefIDMessage, Flex, Form, IAddLlmRequestBody, IModalProps, Input, InputNumber, Item, Modal, Option, Promise, React, ReactNode, Select, Space, TypeScript, antd, data, handleOk, lodash/omit, modelType, values

---
*Generated by RAGFlow Repository Documentation Generator*
