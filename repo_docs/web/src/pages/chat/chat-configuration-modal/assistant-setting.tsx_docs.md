# File Documentation: web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx`
- **Extension**: `.tsx`
- **Lines**: 194
- **Characters**: 5,623
- **Size**: 5,623 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import KnowledgeBaseItem from '@/components/knowledge-base-item';
import { TavilyItem } from '@/components/tavily-item';
import { useTranslate } from '@/hooks/common-hooks';
import { useFetchTenantInfo } from '@/hooks/user-setting-hooks';
import { PlusOutlined } from '@ant-design/icons';
import { Form, Input, message, Select, Switch, Upload } from 'antd';
import classNames from 'classnames';
import { useCallback } from 'react';
import { ISegmentedContentProps } from '../interface';

import { DatasetMetadata } from '@/constants/chat';
import styles from './index.less';
import { MetadataFilterConditions } from './metadata-filter-conditions';

const emptyResponseField = ['prompt_config', 'empty_response'];

const AssistantSetting = ({
  show,
  form,
  setHasError,
}: ISegmentedContentProps) => {
  const { t } = useTranslate('chat');
  const { data } = useFetchTenantInfo(true);

  const MetadataOptions = Object.values(DatasetMetadata).map((x) => {
    return {
      value: x,
      label: t(`meta.${x}`),
    };
  });

  const metadata = Form.useWatch(['meta_data_filter', 'method'], form);
  const kbIds = Form.useWatch(['kb_ids'], form);

  const hasKnowledge = Array.isArray(kbIds) && kbIds.length > 0;

  const handleChange = useCallback(() => {
    const kbIds = form.getFieldValue('kb_ids');
    const emptyResponse = form.getFieldValue(emptyResponseField);

    const required =
      emptyResponse && ((Array.isArray(kbIds) && kbIds.length === 0) || !kbIds);

    setHasError(required);
    form.setFields([
      {
        name: emptyResponseField,
        errors: required ? [t('emptyResponseMessage')] : [],
      },
    ]);
  }, [form, setHasError, t]);

  const normFile = (e: any) => {
    if (Array.isArray(e)) {
      return e;
    }
    return e?.fileList;
  };

  const handleTtsChange = useCallback(
    (checked: boolean) => {
      if (checked && !data.tts_id) {
        message.error(`Please set TTS model firstly. 
        Setting >> Model providers >> System model settings`);
        form.setFieldValue(['prompt_config', 'tts'], false);
      }
    },
    [data, form],
  );

  const uploadButton = (
    <button style={{ border: 0, background: 'none' }} type="button">
      <PlusOutlined />
      <div style={{ marginTop: 8 }}>{t('upload', { keyPrefix: 'common' })}</div>
    </button>
  );

  return (
    <section
      className={classNames({
        [styles.segmentedHidden]: !show,
      })}
    >
      <Form.Item
        name={'name'}
        label={t('assistantName')}
        rules={[{ required: true, message: t('assistantNameMessage') }]}
      >
        <Input placeholder={t('namePlaceholder')} />
      </Form.Item>
      <Form.Item name={'description'} label={t('description')}>
        <Input placeholder={t('descriptionPlaceholder')} />
      </Form.Item>
      <Form.Item
        name="icon"
        label={t('assistantAvatar')}
        valuePropName="fileList"
        getValueFromEvent={normFile}
      >
        <Upload
          listType="picture-card"
          maxCount={1}
          beforeUpload={() => false}
          showUploadList={{ showPreviewIcon: false, showRemoveIcon: false }}
        >
          {show ? uploadButton : null}
        </Upload>
      </Form.Item>
      <Form.Item
        name={'language'}
        label={t('language')}
        initialValue={'English'}
        tooltip="coming soon"
        style={{ display: 'none' }}
      >
        <Select
          options={[
            { value: 'Chinese', label: t('chinese', { keyPrefix: 'common' }) },
            { value: 'English', label: t('english', { keyPrefix: 'common' }) },
          ]}
        />
      </Form.Item>
      <Form.Item
        name={emptyResponseField}
        label={t('emptyResponse')}
        tooltip={t('emptyResponseTip')}
      >
        <Input placeholder="" onChange={handleChange} />
      </Form.Item>
      <Form.Item
        name={['prompt_config', 'prologue']}
        label={t('setAnOpener')}
        tooltip={t('setAnOpenerTip')}
        initialValue={t('setAnOpenerInitial')}
      >
        <Input.TextArea autoSize={{ minRows: 5 }} />
      </Form.Item>
      <Form.Item
        label={t('quote')}
        valuePropName="checked"
        name={['prompt_config', 'quote']}
        tooltip={t('quoteTip')}
        initialValue={true}
      >
        <Switch />
      </Form.Item>
      <Form.Item
        label={t('keyword')}
        valuePropName="checked"
        name={['prompt_config', 'keyword']}
        tooltip={t('keywordTip')}
        initialValue={false}
      >
        <Switch />
      </Form.Item>
      <Form.Item
        label={t('tts')}
        valuePropName="checked"
        name={['prompt_config', 'tts']}
        tooltip={t('ttsTip')}
        initialValue={false}
      >
        <Switch onChange={handleTtsChange} />
      </Form.Item>
      <TavilyItem></TavilyItem>
      <KnowledgeBaseItem
        required={false}
        onChange={handleChange}
      ></KnowledgeBaseItem>
      {hasKnowledge && (
        <Form.Item
          label={t('metadata')}
          name={['meta_data_filter', 'method']}
          tooltip={t('metadataTip')}
          initialValue={DatasetMetadata.Disabled}
        >
          <Select options={MetadataOptions} />
        </Form.Item>
      )}
      {hasKnowledge && metadata === DatasetMetadata.Manual && (
        <Form.Item
          label={t('conditions')}
          tooltip={t('ttsTip')}
          initialValue={false}
        >
          <MetadataFilterConditions kbIds={kbIds}></MetadataFilterConditions>
        </Form.Item>
      )}
    </section>
  );
};

export default AssistantSetting;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 194 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `AssistantSetting()`: Function definition
- `MetadataOptions()`: Function definition
- `handleChange()`: Function definition
- `normFile()`: Function definition
- `handleTtsChange()`: Function definition

### Imports (12)

- `import KnowledgeBaseItem from '@/components/knowledge-base-item';`
- `import { TavilyItem } from '@/components/tavily-item';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useFetchTenantInfo } from '@/hooks/user-setting-hooks';`
- `import { PlusOutlined } from '@ant-design/icons';`
- `import { Form, Input, message, Select, Switch, Upload } from 'antd';`
- `import classNames from 'classnames';`
- `import { useCallback } from 'react';`
- `import { ISegmentedContentProps } from '../interface';`
- `import { DatasetMetadata } from '@/constants/chat';`

## Code Structure Analysis

- Total lines: 194
- Blank lines: 15 (7.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~179


## Dependencies and Imports

- `@/components/knowledge-base-item`
- `@/components/tavily-item`
- `@/hooks/common-hooks`
- `@/hooks/user-setting-hooks`
- `@ant-design/icons`
- `antd`
- `classnames`
- `react`
- `../interface`
- `@/constants/chat`
- `./index.less`
- `./metadata-filter-conditions`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/chat-configuration-modal`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/chat/chat-configuration-modal/` directory
- Potential test file: `test_assistant-setting.tsx`

## Keywords

../interface, ./index.less, ./metadata-filter-conditions, @/components/knowledge-base-item, @/components/tavily-item, @/constants/chat, @/hooks/common-hooks, @/hooks/user-setting-hooks, @ant-design/icons, Array, AssistantSetting, Chinese, DatasetMetadata, Disabled, English, Form, ISegmentedContentProps, Input, Item, KnowledgeBaseItem, Manual, MetadataFilterConditions, MetadataOptions, Model, Object, Please, PlusOutlined, Select, Setting, Switch, System, TTS, TavilyItem, TextArea, TypeScript, Upload, ant, antd, classnames, emptyResponse, emptyResponseField, handleChange, handleTtsChange, hasKnowledge, kbIds, metadata, normFile, react, required, uploadButton

---
*Generated by RAGFlow Repository Documentation Generator*
