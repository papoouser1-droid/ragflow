# File Documentation: web/src/pages/dataset/dataset-setting/hooks.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 122
- **Characters**: 3,610
- **Size**: 3,610 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

// The value that does not need to be displayed in the analysis method Select

## Detailed Walkthrough

### Exports (7)

- `useSelectChunkMethodList`: Exported entity
- `useSelectEmbeddingModelOptions`: Exported entity
- `useHasParsedDocument`: Exported entity
- `useFetchKnowledgeConfigurationOnMount`: Exported entity
- `useSelectKnowledgeDetailsLoading`: Exported entity
- `useRenameKnowledgeTag`: Exported entity
- `useHandleKbEmbedding`: Exported entity

### Functions (10)

- `useSelectChunkMethodList()`: Function definition
- `parserList()`: Function definition
- `useSelectEmbeddingModelOptions()`: Function definition
- `useHasParsedDocument()`: Function definition
- `useFetchKnowledgeConfigurationOnMount()`: Function definition
- `useSelectKnowledgeDetailsLoading()`: Function definition
- `useRenameKnowledgeTag()`: Function definition
- `handleShowTagRenameModal()`: Function definition
- `useHandleKbEmbedding()`: Function definition
- `handleChange()`: Function definition

### Imports (13)

- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';`
- `import { useFetchKnowledgeBaseConfiguration } from '@/hooks/use-knowledge-request';`
- `import { useSelectParserList } from '@/hooks/user-setting-hooks';`
- `import kbService from '@/services/knowledge-service';`
- `import { useIsFetching } from '@tanstack/react-query';`
- `import { pick } from 'lodash';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { UseFormReturn } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 15 (12.3%)
- Comment lines: ~1 (0.8%)
- Code lines: ~106


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/hooks/llm-hooks`
- `@/hooks/use-knowledge-request`
- `@/hooks/user-setting-hooks`
- `@/services/knowledge-service`
- `@tanstack/react-query`
- `lodash`
- `react`
- `react-hook-form`
- `umi`
- `zod`
- `./form-schema`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
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

- Other files in `web/src/pages/dataset/dataset-setting/` directory
- Potential test file: `test_hooks.ts`

## Keywords

./form-schema, @/constants/knowledge, @/hooks/common-hooks, @/hooks/llm-hooks, @/hooks/use-knowledge-request, @/hooks/user-setting-hooks, @/services/knowledge-service, @tanstack/react-query, Embedding, HiddenFields, LlmModelType, Select, The, TypeScript, UseFormReturn, allOptions, formValues, handleChange, handleShowTagRenameModal, knowledgeBaseId, lodash, parserList, parser_config, react, react-hook-form, res, tanstack, umi, useFetchKnowledgeConfigurationOnMount, useHandleKbEmbedding, useHasParsedDocument, useRenameKnowledgeTag, useSelectChunkMethodList, useSelectEmbeddingModelOptions, useSelectKnowledgeDetailsLoading, zod

---
*Generated by RAGFlow Repository Documentation Generator*
