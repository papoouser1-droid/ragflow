# File Documentation: web/src/pages/add-knowledge/components/knowledge-setting/hooks.ts

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-setting/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 122
- **Characters**: 3,547
- **Size**: 3,550 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { LlmModelType } from '@/constants/knowledge';
import { useSetModalState } from '@/hooks/common-hooks';
import {
  useFetchKnowledgeBaseConfiguration,
  useUpdateKnowledge,
} from '@/hooks/knowledge-hooks';
import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';
import { useNavigateToDataset } from '@/hooks/route-hook';
import { useSelectParserList } from '@/hooks/user-setting-hooks';
import {
  getBase64FromUploadFileList,
  getUploadFileListFromBase64,
} from '@/utils/file-util';
import { useIsFetching } from '@tanstack/react-query';
import { Form, UploadFile } from 'antd';
import { FormInstance } from 'antd/lib';
import pick from 'lodash/pick';
import { useCallback, useEffect, useState } from 'react';

export const useSubmitKnowledgeConfiguration = (form: FormInstance) => {
  const { saveKnowledgeConfiguration, loading } = useUpdateKnowledge();
  const navigateToDataset = useNavigateToDataset();

  const submitKnowledgeConfiguration = useCallback(async () => {
    const values = await form.validateFields();
    const avatar = await getBase64FromUploadFileList(values.avatar);
    saveKnowledgeConfiguration({
      ...values,
      avatar,
    });
    navigateToDataset();
  }, [saveKnowledgeConfiguration, form, navigateToDataset]);

  return {
    submitKnowledgeConfiguration,
    submitLoading: loading,
    navigateToDataset,
  };
};

// The value that does not need to be displayed in the analysis method Select
const HiddenFields = ['email', 'picture', 'audio'];

export function useSelectChunkMethodList() {
  const parserList = useSelectParserList();

  return parserList.filter((x) => !HiddenFields.some((y) => y === x.value));
}

export function useSelectEmbeddingModelOptions() {
  const allOptions = useSelectLlmOptionsByModelType();
  return allOptions[LlmModelType.Embedding];
}

export function useHasParsedDocument() {
  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration();
  return knowledgeDetails.chunk_num > 0;
}

export const useFetchKnowledgeConfigurationOnMount = (form: FormInstance) => {
  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration();

  useEffect(() => {
    const fileList: UploadFile[] = getUploadFileListFromBase64(
      knowledgeDetails.avatar,
    );
    form.setFieldsValue({
      ...pick(knowledgeDetails, [
        'description',
        'name',
        'permission',
        'embd_id',
        'parser_id',
        'language',
        'parser_config',
        'pagerank',
      ]),
      avatar: fileList,
    });
  }, [form, knowledgeDetails]);

  return knowledgeDetails;
};

export const useSelectKnowledgeDetailsLoading = () =>
  useIsFetching({ queryKey: ['fetchKnowledgeDetail'] }) > 0;

export const useHandleChunkMethodChange = () => {
  const [form] = Form.useForm();
  const chunkMethod = Form.useWatch('parser_id', form);

  useEffect(() => {
    console.log('🚀 ~ useHandleChunkMethodChange ~ chunkMethod:', chunkMethod);
  }, [chunkMethod]);

  return { form, chunkMethod };
};

export const useRenameKnowledgeTag = () => {
  const [tag, setTag] = useState<string>('');
  const {
    visible: tagRenameVisible,
    hideModal: hideTagRenameModal,
    showModal: showFileRenameModal,
  } = useSetModalState();

  const handleShowTagRenameModal = useCallback(
    (record: string) => {
      setTag(record);
      showFileRenameModal();
    },
    [showFileRenameModal],
  );

  return {
    initialName: tag,
    tagRenameVisible,
    hideTagRenameModal,
    showTagRenameModal: handleShowTagRenameModal,
  };
};

```

## High-Level Overview

// The value that does not need to be displayed in the analysis method Select

## Detailed Walkthrough

### Exports (8)

- `useSubmitKnowledgeConfiguration`: Exported entity
- `useSelectChunkMethodList`: Exported entity
- `useSelectEmbeddingModelOptions`: Exported entity
- `useHasParsedDocument`: Exported entity
- `useFetchKnowledgeConfigurationOnMount`: Exported entity
- `useSelectKnowledgeDetailsLoading`: Exported entity
- `useHandleChunkMethodChange`: Exported entity
- `useRenameKnowledgeTag`: Exported entity

### Functions (12)

- `useSubmitKnowledgeConfiguration()`: Function definition
- `submitKnowledgeConfiguration()`: Function definition
- `useSelectChunkMethodList()`: Function definition
- `parserList()`: Function definition
- `useSelectEmbeddingModelOptions()`: Function definition
- `useHasParsedDocument()`: Function definition
- `useFetchKnowledgeConfigurationOnMount()`: Function definition
- `useSelectKnowledgeDetailsLoading()`: Function definition
- `useHandleChunkMethodChange()`: Function definition
- `chunkMethod()`: Function definition
- `useRenameKnowledgeTag()`: Function definition
- `handleShowTagRenameModal()`: Function definition

### Imports (12)

- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import {`
- `import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';`
- `import { useNavigateToDataset } from '@/hooks/route-hook';`
- `import { useSelectParserList } from '@/hooks/user-setting-hooks';`
- `import {`
- `import { useIsFetching } from '@tanstack/react-query';`
- `import { Form, UploadFile } from 'antd';`
- `import { FormInstance } from 'antd/lib';`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 19 (15.6%)
- Comment lines: ~1 (0.8%)
- Code lines: ~102


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/hooks/llm-hooks`
- `@/hooks/route-hook`
- `@/hooks/user-setting-hooks`
- `@tanstack/react-query`
- `antd`
- `antd/lib`
- `lodash/pick`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-setting`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
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

- Other files in `web/src/pages/add-knowledge/components/knowledge-setting/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/constants/knowledge, @/hooks/common-hooks, @/hooks/llm-hooks, @/hooks/route-hook, @/hooks/user-setting-hooks, @tanstack/react-query, Embedding, Form, FormInstance, HiddenFields, LlmModelType, Select, The, TypeScript, UploadFile, allOptions, antd, antd/lib, avatar, chunkMethod, fileList, handleShowTagRenameModal, lodash/pick, navigateToDataset, parserList, react, submitKnowledgeConfiguration, tanstack, useFetchKnowledgeConfigurationOnMount, useHandleChunkMethodChange, useHasParsedDocument, useRenameKnowledgeTag, useSelectChunkMethodList, useSelectEmbeddingModelOptions, useSelectKnowledgeDetailsLoading, useSubmitKnowledgeConfiguration, values

---
*Generated by RAGFlow Repository Documentation Generator*
