# Documentation: web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx`
- **Size**: 5623 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chat/chat-configuration-modal/assistant-setting.tsx` is located in the `web/src/pages/chat/chat-configuration-modal` directory.

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

- [editable-cell.tsx](editable-cell.tsx_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [metadata-filter-conditions.tsx](metadata-filter-conditions.tsx_docs.md)
- [model-setting.tsx](model-setting.tsx_docs.md)
- [prompt-engine.tsx](prompt-engine.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
