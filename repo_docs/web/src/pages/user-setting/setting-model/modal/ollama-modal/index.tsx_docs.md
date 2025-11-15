# File Documentation: web/src/pages/user-setting/setting-model/modal/ollama-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/ollama-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 273
- **Characters**: 8,388
- **Size**: 8,388 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { LLMFactory } from '@/constants/llm';
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { IAddLlmRequestBody } from '@/interfaces/request/llm';
import {
  Flex,
  Form,
  Input,
  InputNumber,
  Modal,
  Select,
  Space,
  Switch,
} from 'antd';
import omit from 'lodash/omit';
import { useEffect } from 'react';

type FieldType = IAddLlmRequestBody & {
  vision: boolean;
  provider_order?: string;
};

const { Option } = Select;

const llmFactoryToUrlMap = {
  [LLMFactory.Ollama]:
    'https://github.com/infiniflow/ragflow/blob/main/docs/guides/models/deploy_local_llm.mdx',
  [LLMFactory.Xinference]:
    'https://inference.readthedocs.io/en/latest/user_guide',
  [LLMFactory.ModelScope]:
    'https://www.modelscope.cn/docs/model-service/API-Inference/intro',
  [LLMFactory.LocalAI]: 'https://localai.io/docs/getting-started/models/',
  [LLMFactory.LMStudio]: 'https://lmstudio.ai/docs/basics',
  [LLMFactory.OpenAiAPICompatible]:
    'https://platform.openai.com/docs/models/gpt-4',
  [LLMFactory.TogetherAI]: 'https://docs.together.ai/docs/deployment-options',
  [LLMFactory.Replicate]: 'https://replicate.com/docs/topics/deployments',
  [LLMFactory.OpenRouter]: 'https://openrouter.ai/docs',
  [LLMFactory.HuggingFace]:
    'https://huggingface.co/docs/text-embeddings-inference/quick_tour',
  [LLMFactory.GPUStack]: 'https://docs.gpustack.ai/latest/quickstart',
  [LLMFactory.VLLM]: 'https://docs.vllm.ai/en/latest/',
  [LLMFactory.TokenPony]: 'https://docs.tokenpony.cn/#/',
};
type LlmFactory = keyof typeof llmFactoryToUrlMap;

const OllamaModal = ({
  visible,
  hideModal,
  onOk,
  loading,
  llmFactory,
  editMode = false,
  initialValues,
}: IModalProps<IAddLlmRequestBody> & {
  llmFactory: string;
  editMode?: boolean;
  initialValues?: Partial<IAddLlmRequestBody>;
}) => {
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

  useEffect(() => {
    if (visible && editMode && initialValues) {
      const formValues = {
        llm_name: initialValues.llm_name,
        model_type: initialValues.model_type,
        api_base: initialValues.api_base,
        max_tokens: initialValues.max_tokens || 8192,
        api_key: '',
        ...initialValues,
      };
      form.setFieldsValue(formValues);
    } else if (visible && !editMode) {
      form.resetFields();
    }
  }, [visible, editMode, initialValues, form]);

  const url =
    llmFactoryToUrlMap[llmFactory as LlmFactory] ||
    'https://github.com/infiniflow/ragflow/blob/main/docs/guides/models/deploy_local_llm.mdx';
  const optionsMap = {
    [LLMFactory.HuggingFace]: [
      { value: 'embedding', label: 'embedding' },
      { value: 'chat', label: 'chat' },
      { value: 'rerank', label: 'rerank' },
    ],
    [LLMFactory.LMStudio]: [
      { value: 'chat', label: 'chat' },
      { value: 'embedding', label: 'embedding' },
      { value: 'image2text', label: 'image2text' },
    ],
    [LLMFactory.Xinference]: [
      { value: 'chat', label: 'chat' },
      { value: 'embedding', label: 'embedding' },
      { value: 'rerank', label: 'rerank' },
      { value: 'image2text', label: 'image2text' },
      { value: 'speech2text', label: 'sequence2text' },
      { value: 'tts', label: 'tts' },
    ],
    [LLMFactory.ModelScope]: [{ value: 'chat', label: 'chat' }],
    [LLMFactory.GPUStack]: [
      { value: 'chat', label: 'chat' },
      { value: 'embedding', label: 'embedding' },
      { value: 'rerank', label: 'rerank' },
      { value: 'speech2text', label: 'sequence2text' },
      { value: 'tts', label: 'tts' },
    ],
    [LLMFactory.OpenRouter]: [
      { value: 'chat', label: 'chat' },
      { value: 'image2text', label: 'image2text' },
    ],
    Default: [
      { value: 'chat', label: 'chat' },
      { value: 'embedding', label: 'embedding' },
      { value: 'rerank', label: 'rerank' },
      { value: 'image2text', label: 'image2text' },
    ],
  };
  const getOptions = (factory: string) => {
    return optionsMap[factory as keyof typeof optionsMap] || optionsMap.Default;
  };
  return (
    <Modal
      title={
        editMode
          ? t('editLlmTitle', { name: llmFactory })
          : t('addLlmTitle', { name: llmFactory })
      }
      open={visible}
      onOk={handleOk}
      onCancel={hideModal}
      okButtonProps={{ loading }}
      footer={(originNode: React.ReactNode) => {
        return (
          <Flex justify={'space-between'}>
            <a href={url} target="_blank" rel="noreferrer">
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
          label={t(llmFactory === 'Xinference' ? 'modelUid' : 'modelName')}
          name="llm_name"
          rules={[{ required: true, message: t('modelNameMessage') }]}
        >
          <Input
            placeholder={t('modelNameMessage')}
            onKeyDown={handleKeyDown}
          />
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
            onKeyDown={handleKeyDown}
          />
        </Form.Item>
        {llmFactory === LLMFactory.OpenRouter && (
          <Form.Item<FieldType>
            label="Provider Order"
            name="provider_order"
            tooltip="Comma-separated provider list, e.g. Groq,Fireworks"
            rules={[]}
          >
            <Input placeholder="Groq,Fireworks" onKeyDown={handleKeyDown} />
          </Form.Item>
        )}

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

export default OllamaModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/ollama-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 273 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `OllamaModal()`: Function definition
- `handleOk()`: Function definition
- `handleKeyDown()`: Function definition
- `getOptions()`: Function definition

### Imports (7)

- `import { LLMFactory } from '@/constants/llm';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IAddLlmRequestBody } from '@/interfaces/request/llm';`
- `import {`
- `import omit from 'lodash/omit';`
- `import { useEffect } from 'react';`

## Code Structure Analysis

- Total lines: 273
- Blank lines: 14 (5.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~259


## Dependencies and Imports

- `@/constants/llm`
- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/interfaces/request/llm`
- `lodash/omit`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/ollama-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/setting-model/modal/ollama-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/constants/llm, @/hooks/common-hooks, @/interfaces/common, @/interfaces/request/llm, API, Comma, Default, Enter, Error, FieldType, Fireworks, Flex, Form, GPUStack, Groq, HuggingFace, IAddLlmRequestBody, IModalProps, Inference, Input, InputNumber, Item, KeyboardEvent, LLMFactory, LMStudio, LlmFactory, LocalAI, Modal, ModelScope, Ollama, OllamaModal, OpenAiAPICompatible, OpenRouter, Option, Order, Partial, Promise, Provider, React, ReactNode, Replicate, Select, Space, Switch, TogetherAI, TokenPony, TypeScript, VLLM, Xinference, data...

---
*Generated by RAGFlow Repository Documentation Generator*
