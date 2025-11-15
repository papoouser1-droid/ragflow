# Documentation: web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx`
- **Size**: 3449 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal/index.tsx` is located in the `web/src/pages/user-setting/setting-model/modal/system-model-setting-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to system-model-setting-modal.

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
