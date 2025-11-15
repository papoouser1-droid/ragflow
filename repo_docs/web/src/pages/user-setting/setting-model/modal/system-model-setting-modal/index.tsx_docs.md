# File Documentation: web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 133
- **Characters**: 3,449
- **Size**: 3,449 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IModalManagerChildrenProps } from '@/components/modal-manager';
import { LlmModelType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import {
  ISystemModelSettingSavingParams,
  useComposeLlmOptionsByModelTypes,
} from '@/hooks/llm-hooks';
import { Form, Modal, Select } from 'antd';
import { useEffect } from 'react';
import { useFetchSystemModelSettingOnMount } from '../../hooks';

interface IProps extends Omit<IModalManagerChildrenProps, 'showModal'> {
  loading: boolean;
  onOk: (
    payload: Omit<ISystemModelSettingSavingParams, 'tenant_id' | 'name'>,
  ) => void;
}

const SystemModelSettingModal = ({
  visible,
  hideModal,
  onOk,
  loading,
}: IProps) => {
  const [form] = Form.useForm();
  const { systemSetting: initialValues, allOptions } =
    useFetchSystemModelSettingOnMount();
  const { t } = useTranslate('setting');

  const handleOk = async () => {
    const values = await form.validateFields();
    onOk({
      ...values,
      asr_id: values.asr_id ?? '',
      embd_id: values.embd_id ?? '',
      img2txt_id: values.img2txt_id ?? '',
      llm_id: values.llm_id ?? '',
    });
  };

  useEffect(() => {
    if (visible) {
      form.setFieldsValue(initialValues);
    }
  }, [form, initialValues, visible]);

  const onFormLayoutChange = () => {};

  const modelOptions = useComposeLlmOptionsByModelTypes([
    LlmModelType.Chat,
    LlmModelType.Image2text,
  ]);

  return (
    <Modal
      title={t('systemModelSettings')}
      open={visible}
      onOk={handleOk}
      onCancel={hideModal}
      okButtonProps={{ loading }}
      confirmLoading={loading}
    >
      <Form form={form} onValuesChange={onFormLayoutChange} layout={'vertical'}>
        <Form.Item
          label={t('chatModel')}
          name="llm_id"
          tooltip={t('chatModelTip')}
        >
          <Select options={modelOptions} allowClear showSearch />
        </Form.Item>
        <Form.Item
          label={t('embeddingModel')}
          name="embd_id"
          tooltip={t('embeddingModelTip')}
        >
          <Select
            options={allOptions[LlmModelType.Embedding]}
            allowClear
            showSearch
          />
        </Form.Item>
        <Form.Item
          label={t('img2txtModel')}
          name="img2txt_id"
          tooltip={t('img2txtModelTip')}
        >
          <Select
            options={allOptions[LlmModelType.Image2text]}
            allowClear
            showSearch
          />
        </Form.Item>

        <Form.Item
          label={t('sequence2txtModel')}
          name="asr_id"
          tooltip={t('sequence2txtModelTip')}
        >
          <Select
            options={allOptions[LlmModelType.Speech2text]}
            allowClear
            showSearch
          />
        </Form.Item>
        <Form.Item
          label={t('rerankModel')}
          name="rerank_id"
          tooltip={t('rerankModelTip')}
        >
          <Select
            options={allOptions[LlmModelType.Rerank]}
            allowClear
            showSearch
          />
        </Form.Item>
        <Form.Item
          label={t('ttsModel')}
          name="tts_id"
          tooltip={t('ttsModelTip')}
        >
          <Select
            options={allOptions[LlmModelType.TTS]}
            allowClear
            showSearch
          />
        </Form.Item>
      </Form>
    </Modal>
  );
};

export default SystemModelSettingModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 133 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `SystemModelSettingModal()`: Function definition
- `handleOk()`: Function definition
- `values()`: Function definition
- `onFormLayoutChange()`: Function definition

### Imports (7)

- `import { IModalManagerChildrenProps } from '@/components/modal-manager';`
- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import { Form, Modal, Select } from 'antd';`
- `import { useEffect } from 'react';`
- `import { useFetchSystemModelSettingOnMount } from '../../hooks';`

## Code Structure Analysis

- Total lines: 133
- Blank lines: 10 (7.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~123


## Dependencies and Imports

- `@/components/modal-manager`
- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `antd`
- `react`
- `../../hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks, @/components/modal-manager, @/constants/knowledge, @/hooks/common-hooks, Chat, Embedding, Form, IModalManagerChildrenProps, IProps, ISystemModelSettingSavingParams, Image2text, Item, LlmModelType, Modal, Omit, Rerank, Select, Speech2text, SystemModelSettingModal, TTS, TypeScript, antd, handleOk, modelOptions, onFormLayoutChange, react, values

---
*Generated by RAGFlow Repository Documentation Generator*
