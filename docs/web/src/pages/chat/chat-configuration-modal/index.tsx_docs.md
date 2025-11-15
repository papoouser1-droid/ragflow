# Documentation: web/src/pages/chat/chat-configuration-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/index.tsx`
- **Size**: 5743 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chat/chat-configuration-modal/index.tsx`.

## Original Source Code

```tsx
import { ReactComponent as ChatConfigurationAtom } from '@/assets/svg/chat-configuration-atom.svg';
import { IModalManagerChildrenProps } from '@/components/modal-manager';
import {
  ModelVariableType,
  settledModelVariableMap,
} from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useFetchModelId } from '@/hooks/logic-hooks';
import { IDialog } from '@/interfaces/database/chat';
import { getBase64FromUploadFileList } from '@/utils/file-util';
import { removeUselessFieldsFromValues } from '@/utils/form';
import { Divider, Flex, Form, Modal, Segmented, UploadFile } from 'antd';
import { SegmentedValue } from 'antd/es/segmented';
import camelCase from 'lodash/camelCase';
import { useEffect, useRef, useState } from 'react';
import { IPromptConfigParameters } from '../interface';
import AssistantSetting from './assistant-setting';
import ModelSetting from './model-setting';
import PromptEngine from './prompt-engine';

import styles from './index.less';

const layout = {
  labelCol: { span: 9 },
  wrapperCol: { span: 15 },
};

const validateMessages = {
  required: '${label} is required!',
  types: {
    email: '${label} is not a valid email!',
    number: '${label} is not a valid number!',
  },
  number: {
    range: '${label} must be between ${min} and ${max}',
  },
};

enum ConfigurationSegmented {
  AssistantSetting = 'Assistant Setting',
  PromptEngine = 'Prompt Engine',
  ModelSetting = 'Model Setting',
}

const segmentedMap = {
  [ConfigurationSegmented.AssistantSetting]: AssistantSetting,
  [ConfigurationSegmented.ModelSetting]: ModelSetting,
  [ConfigurationSegmented.PromptEngine]: PromptEngine,
};

interface IProps extends IModalManagerChildrenProps {
  initialDialog: IDialog;
  loading: boolean;
  onOk: (dialog: IDialog) => void;
  clearDialog: () => void;
}

const ChatConfigurationModal = ({
  visible,
  hideModal,
  initialDialog,
  loading,
  onOk,
  clearDialog,
}: IProps) => {
  const [form] = Form.useForm();
  const [hasError, setHasError] = useState(false);

  const [value, setValue] = useState<ConfigurationSegmented>(
    ConfigurationSegmented.AssistantSetting,
  );
  const promptEngineRef = useRef<Array<IPromptConfigParameters>>([]);
  const modelId = useFetchModelId();
  const { t } = useTranslate('chat');

  const handleOk = async () => {
    const values = await form.validateFields();
    if (hasError) {
      return;
    }
    const nextValues: any = removeUselessFieldsFromValues(
      values,
      'llm_setting.',
    );
    const emptyResponse = nextValues.prompt_config?.empty_response ?? '';

    const icon = await getBase64FromUploadFileList(values.icon);

    const finalValues = {
      dialog_id: initialDialog.id,
      ...nextValues,
      vector_similarity_weight: 1 - nextValues.vector_similarity_weight,
      prompt_config: {
        ...nextValues.prompt_config,
        parameters: promptEngineRef.current,
        empty_response: emptyResponse,
      },
      icon,
    };
    onOk(finalValues);
  };

  const handleSegmentedChange = (val: SegmentedValue) => {
    setValue(val as ConfigurationSegmented);
  };

  const handleModalAfterClose = () => {
    clearDialog();
    form.resetFields();
  };

  const title = (
    <Flex gap={16}>
      <ChatConfigurationAtom></ChatConfigurationAtom>
      <div>
        <b>{t('chatConfiguration')}</b>
        <div className={styles.chatConfigurationDescription}>
          {t('chatConfigurationDescription')}
        </div>
      </div>
    </Flex>
  );

  useEffect(() => {
    if (visible) {
      const icon = initialDialog.icon;
      let fileList: UploadFile[] = [];

      if (icon) {
        fileList = [{ uid: '1', name: 'file', thumbUrl: icon, status: 'done' }];
      }
      form.setFieldsValue({
        ...initialDialog,
        llm_setting:
          initialDialog.llm_setting ??
          settledModelVariableMap[ModelVariableType.Precise],
        icon: fileList,
        llm_id: initialDialog.llm_id ?? modelId,
        vector_similarity_weight:
          1 - (initialDialog.vector_similarity_weight ?? 0.3),
      });
    }
  }, [initialDialog, form, visible, modelId]);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    // Allow Enter in textareas
    if (e.target instanceof HTMLTextAreaElement) {
      return;
    }

    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleOk();
    }
  };

  return (
    <Modal
      title={title}
      width={688}
      open={visible}
      onOk={handleOk}
      onCancel={hideModal}
      confirmLoading={loading}
      destroyOnClose
      afterClose={handleModalAfterClose}
    >
      <Segmented
        size={'large'}
        value={value}
        onChange={handleSegmentedChange}
        options={Object.values(ConfigurationSegmented).map((x) => ({
          label: t(camelCase(x)),
          value: x,
        }))}
        block
      />
      <Divider></Divider>
      <Form
        {...layout}
        name="nest-messages"
        form={form}
        style={{ maxWidth: 600 }}
        validateMessages={validateMessages}
        colon={false}
        onKeyDown={handleKeyDown}
      >
        {Object.entries(segmentedMap).map(([key, Element]) => (
          <Element
            key={key}
            show={key === value}
            form={form}
            setHasError={setHasError}
            {...(key === ConfigurationSegmented.ModelSetting
              ? { initialLlmSetting: initialDialog.llm_setting, visible }
              : {})}
            {...(key === ConfigurationSegmented.PromptEngine
              ? { ref: promptEngineRef }
              : {})}
          ></Element>
        ))}
      </Form>
    </Modal>
  );
};

export default ChatConfigurationModal;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chat/chat-configuration-modal/index.tsx` is located in the `web/src/pages/chat/chat-configuration-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chat-configuration-modal.

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

- [assistant-setting.tsx](assistant-setting.tsx_docs.md)
- [editable-cell.tsx](editable-cell.tsx_docs.md)
- [index.less](index.less_docs.md)
- [metadata-filter-conditions.tsx](metadata-filter-conditions.tsx_docs.md)
- [model-setting.tsx](model-setting.tsx_docs.md)
- [prompt-engine.tsx](prompt-engine.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
