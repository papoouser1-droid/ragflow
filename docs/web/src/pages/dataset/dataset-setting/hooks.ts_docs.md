# Documentation: web/src/pages/dataset/dataset-setting/hooks.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/hooks.ts`
- **Size**: 3610 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-setting/hooks.ts`.

## Original Source Code

```ts
import { LlmModelType } from '@/constants/knowledge';
import { useSetModalState } from '@/hooks/common-hooks';

import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';
import { useFetchKnowledgeBaseConfiguration } from '@/hooks/use-knowledge-request';
import { useSelectParserList } from '@/hooks/user-setting-hooks';
import kbService from '@/services/knowledge-service';
import { useIsFetching } from '@tanstack/react-query';
import { pick } from 'lodash';
import { useCallback, useEffect, useState } from 'react';
import { UseFormReturn } from 'react-hook-form';
import { useParams, useSearchParams } from 'umi';
import { z } from 'zod';
import { formSchema } from './form-schema';

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

export function useHasParsedDocument(isEdit?: boolean) {
  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration({
    isEdit,
  });
  return knowledgeDetails.chunk_num > 0;
}

export const useFetchKnowledgeConfigurationOnMount = (
  form: UseFormReturn<z.infer<typeof formSchema>, any, undefined>,
) => {
  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration();

  useEffect(() => {
    const parser_config = {
      ...form.formState?.defaultValues?.parser_config,
      ...knowledgeDetails.parser_config,
      raptor: {
        ...form.formState?.defaultValues?.parser_config?.raptor,
        ...knowledgeDetails.parser_config?.raptor,
        use_raptor: true,
      },
      graphrag: {
        ...form.formState?.defaultValues?.parser_config?.graphrag,
        ...knowledgeDetails.parser_config?.graphrag,
        use_graphrag: true,
      },
    };
    const formValues = {
      ...pick({ ...knowledgeDetails, parser_config: parser_config }, [
        'description',
        'name',
        'permission',
        'embd_id',
        'parser_id',
        'language',
        'parser_config',
        'connectors',
        'pagerank',
        'avatar',
      ]),
    } as z.infer<typeof formSchema>;
    form.reset(formValues);
  }, [form, knowledgeDetails]);

  return knowledgeDetails;
};

export const useSelectKnowledgeDetailsLoading = () =>
  useIsFetching({ queryKey: ['fetchKnowledgeDetail'] }) > 0;

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

export const useHandleKbEmbedding = () => {
  const { id } = useParams();
  const [searchParams] = useSearchParams();
  const knowledgeBaseId = searchParams.get('id') || id;
  const handleChange = useCallback(
    async ({ embed_id }: { embed_id: string }) => {
      const res = await kbService.checkEmbedding({
        kb_id: knowledgeBaseId,
        embd_id: embed_id,
      });
      return res.data;
    },
    [knowledgeBaseId],
  );
  return {
    handleChange,
  };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-setting/hooks.ts` is located in the `web/src/pages/dataset/dataset-setting` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset-setting.

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

- [category-panel.tsx](category-panel.tsx_docs.md)
- [chunk-method-form.tsx](chunk-method-form.tsx_docs.md)
- [chunk-method-learn-more.tsx](chunk-method-learn-more.tsx_docs.md)
- [configuration-form-container.tsx](configuration-form-container.tsx_docs.md)
- [form-schema.ts](form-schema.ts_docs.md)
- [general-form.tsx](general-form.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [permission-form-field.tsx](permission-form-field.tsx_docs.md)
- [saving-button.tsx](saving-button.tsx_docs.md)
- [tag-tabs.tsx](tag-tabs.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
