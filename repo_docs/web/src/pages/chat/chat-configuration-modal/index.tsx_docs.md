# File Documentation: web/src/pages/chat/chat-configuration-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 208
- **Characters**: 5,743
- **Size**: 5,743 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chat/chat-configuration-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 208 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (6)

- `segmentedMap()`: Function definition
- `ChatConfigurationModal()`: Function definition
- `handleOk()`: Function definition
- `handleSegmentedChange()`: Function definition
- `handleModalAfterClose()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (17)

- `import { ReactComponent as ChatConfigurationAtom } from '@/assets/svg/chat-configuration-atom.svg';`
- `import { IModalManagerChildrenProps } from '@/components/modal-manager';`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useFetchModelId } from '@/hooks/logic-hooks';`
- `import { IDialog } from '@/interfaces/database/chat';`
- `import { getBase64FromUploadFileList } from '@/utils/file-util';`
- `import { removeUselessFieldsFromValues } from '@/utils/form';`
- `import { Divider, Flex, Form, Modal, Segmented, UploadFile } from 'antd';`
- `import { SegmentedValue } from 'antd/es/segmented';`

## Code Structure Analysis

- Total lines: 208
- Blank lines: 21 (10.1%)
- Comment lines: ~1 (0.5%)
- Code lines: ~186


## Dependencies and Imports

- `@/assets/svg/chat-configuration-atom.svg`
- `@/components/modal-manager`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `@/interfaces/database/chat`
- `@/utils/file-util`
- `@/utils/form`
- `antd`
- `antd/es/segmented`
- `lodash/camelCase`
- `react`
- `../interface`
- `./assistant-setting`
- `./model-setting`
- `./prompt-engine`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/chat-configuration-modal`.

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

- Other files in `web/src/pages/chat/chat-configuration-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../interface, ./assistant-setting, ./index.less, ./model-setting, ./prompt-engine, @/assets/svg/chat-configuration-atom.svg, @/components/modal-manager, @/hooks/common-hooks, @/hooks/logic-hooks, @/interfaces/database/chat, @/utils/file-util, @/utils/form, Allow, Array, Assistant, AssistantSetting, ChatConfigurationAtom, ChatConfigurationModal, ConfigurationSegmented, Divider, Element, Engine, Enter, Flex, Form, HTMLTextAreaElement, IDialog, IModalManagerChildrenProps, IPromptConfigParameters, IProps, KeyboardEvent, Modal, Model, ModelSetting, ModelVariableType, Object, Precise, Prompt, PromptEngine, React, ReactComponent, Segmented, SegmentedValue, Setting, TypeScript, UploadFile, antd, antd/es/segmented, emptyResponse, fileList...

---
*Generated by RAGFlow Repository Documentation Generator*
