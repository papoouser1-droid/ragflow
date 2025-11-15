# File Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 141
- **Characters**: 3,954
- **Size**: 3,957 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import EditTag from '@/components/edit-tag';
import { useFetchChunk } from '@/hooks/chunk-hooks';
import { IModalProps } from '@/interfaces/common';
import { IChunk } from '@/interfaces/database/knowledge';
import { DeleteOutlined } from '@ant-design/icons';
import { Divider, Form, Input, Modal, Space, Switch } from 'antd';
import React, { useCallback, useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { useDeleteChunkByIds } from '../../hooks';
import {
  transformTagFeaturesArrayToObject,
  transformTagFeaturesObjectToArray,
} from '../../utils';
import { TagFeatureItem } from './tag-feature-item';

type FieldType = Pick<
  IChunk,
  'content_with_weight' | 'tag_kwd' | 'question_kwd' | 'important_kwd'
>;

interface kFProps {
  doc_id: string;
  chunkId: string | undefined;
  parserId: string;
}

const ChunkCreatingModal: React.FC<IModalProps<any> & kFProps> = ({
  doc_id,
  chunkId,
  hideModal,
  onOk,
  loading,
  parserId,
}) => {
  const [form] = Form.useForm();
  const [checked, setChecked] = useState(false);
  const { removeChunk } = useDeleteChunkByIds();
  const { data } = useFetchChunk(chunkId);
  const { t } = useTranslation();

  const isTagParser = parserId === 'tag';

  const handleOk = useCallback(async () => {
    try {
      const values = await form.validateFields();
      console.log('🚀 ~ handleOk ~ values:', values);

      onOk?.({
        ...values,
        tag_feas: transformTagFeaturesArrayToObject(values.tag_feas),
        available_int: checked ? 1 : 0, // available_int
      });
    } catch (errorInfo) {
      console.log('Failed:', errorInfo);
    }
  }, [checked, form, onOk]);

  const handleRemove = useCallback(() => {
    if (chunkId) {
      return removeChunk([chunkId], doc_id);
    }
  }, [chunkId, doc_id, removeChunk]);

  const handleCheck = useCallback(() => {
    setChecked(!checked);
  }, [checked]);

  useEffect(() => {
    if (data?.code === 0) {
      const { available_int, tag_feas } = data.data;
      form.setFieldsValue({
        ...(data.data || {}),
        tag_feas: transformTagFeaturesObjectToArray(tag_feas),
      });

      setChecked(available_int !== 0);
    }
  }, [data, form, chunkId]);

  return (
    <Modal
      title={`${chunkId ? t('common.edit') : t('common.create')} ${t('chunk.chunk')}`}
      open={true}
      onOk={handleOk}
      onCancel={hideModal}
      okButtonProps={{ loading }}
      destroyOnClose
    >
      <Form form={form} autoComplete="off" layout={'vertical'}>
        <Form.Item<FieldType>
          label={t('chunk.chunk')}
          name="content_with_weight"
          rules={[{ required: true, message: t('chunk.chunkMessage') }]}
        >
          <Input.TextArea autoSize={{ minRows: 4, maxRows: 10 }} />
        </Form.Item>

        <Form.Item<FieldType> label={t('chunk.keyword')} name="important_kwd">
          <EditTag></EditTag>
        </Form.Item>
        <Form.Item<FieldType>
          label={t('chunk.question')}
          name="question_kwd"
          tooltip={t('chunk.questionTip')}
        >
          <EditTag></EditTag>
        </Form.Item>
        {isTagParser && (
          <Form.Item<FieldType>
            label={t('knowledgeConfiguration.tagName')}
            name="tag_kwd"
          >
            <EditTag></EditTag>
          </Form.Item>
        )}

        {!isTagParser && <TagFeatureItem></TagFeatureItem>}
      </Form>

      {chunkId && (
        <section>
          <Divider></Divider>
          <Space size={'large'}>
            <Switch
              checkedChildren={t('chunk.enabled')}
              unCheckedChildren={t('chunk.disabled')}
              onChange={handleCheck}
              checked={checked}
            />

            <span onClick={handleRemove}>
              <DeleteOutlined /> {t('common.delete')}
            </span>
          </Space>
        </section>
      )}
    </Modal>
  );
};
export default ChunkCreatingModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 141 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `handleOk()`: Function definition
- `handleRemove()`: Function definition
- `handleCheck()`: Function definition

### Imports (11)

- `import EditTag from '@/components/edit-tag';`
- `import { useFetchChunk } from '@/hooks/chunk-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IChunk } from '@/interfaces/database/knowledge';`
- `import { DeleteOutlined } from '@ant-design/icons';`
- `import { Divider, Form, Input, Modal, Space, Switch } from 'antd';`
- `import React, { useCallback, useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { useDeleteChunkByIds } from '../../hooks';`
- `import {`

## Code Structure Analysis

- Total lines: 141
- Blank lines: 16 (11.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~125


## Dependencies and Imports

- `@/components/edit-tag`
- `@/hooks/chunk-hooks`
- `@/interfaces/common`
- `@/interfaces/database/knowledge`
- `@ant-design/icons`
- `antd`
- `react`
- `react-i18next`
- `../../hooks`
- `./tag-feature-item`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-creating-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../hooks, ./tag-feature-item, @/components/edit-tag, @/hooks/chunk-hooks, @/interfaces/common, @/interfaces/database/knowledge, @ant-design/icons, ChunkCreatingModal, DeleteOutlined, Divider, EditTag, Failed, FieldType, Form, IChunk, IModalProps, Input, Item, Modal, Pick, React, Space, Switch, TagFeatureItem, TextArea, TypeScript, ant, antd, handleCheck, handleOk, handleRemove, isTagParser, kFProps, react, react-i18next, values

---
*Generated by RAGFlow Repository Documentation Generator*
