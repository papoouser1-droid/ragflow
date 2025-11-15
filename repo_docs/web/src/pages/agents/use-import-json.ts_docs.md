# File Documentation: web/src/pages/agents/use-import-json.ts

## File Metadata

- **Path**: `web/src/pages/agents/use-import-json.ts`
- **Extension**: `.ts`
- **Lines**: 84
- **Characters**: 2,614
- **Size**: 2,614 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useToast } from '@/components/hooks/use-toast';
import message from '@/components/ui/message';
import { AgentCategory, DataflowOperator } from '@/constants/agent';
import { FileMimeType } from '@/constants/common';
import { useSetModalState } from '@/hooks/common-hooks';
import { EmptyDsl, useSetAgent } from '@/hooks/use-agent-request';
import { Node } from '@xyflow/react';
import isEmpty from 'lodash/isEmpty';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { DataflowEmptyDsl } from './hooks/use-create-agent';
import { FormSchemaType } from './upload-agent-dialog/upload-agent-form';

function hasNode(nodes: Node[], operator: DataflowOperator) {
  return nodes.some((x) => x.data.label === operator);
}

export const useHandleImportJsonFile = () => {
  const {
    visible: fileUploadVisible,
    hideModal: hideFileUploadModal,
    showModal: showFileUploadModal,
  } = useSetModalState();
  const { t } = useTranslation();
  const { toast } = useToast();
  const { loading, setAgent } = useSetAgent();

  const onFileUploadOk = useCallback(
    async ({ fileList, name }: FormSchemaType) => {
      if (fileList.length > 0) {
        const file = fileList[0];
        if (file.type !== FileMimeType.Json) {
          toast({ title: t('flow.jsonUploadTypeErrorMessage') });
          return;
        }

        const graphStr = await file.text();
        const errorMessage = t('flow.jsonUploadContentErrorMessage');
        try {
          const graph = JSON.parse(graphStr);
          if (graphStr && !isEmpty(graph) && Array.isArray(graph?.nodes)) {
            const nodes: Node[] = graph.nodes;

            let isAgent = true;

            if (
              hasNode(nodes, DataflowOperator.Begin) &&
              hasNode(nodes, DataflowOperator.Parser)
            ) {
              isAgent = false;
            }

            const dsl = isAgent
              ? { ...EmptyDsl, graph }
              : { ...DataflowEmptyDsl, graph };

            setAgent({
              title: name,
              dsl,
              canvas_category: isAgent
                ? AgentCategory.AgentCanvas
                : AgentCategory.DataflowCanvas,
            });
            hideFileUploadModal();
          } else {
            message.error(errorMessage);
          }
        } catch (error) {
          message.error(errorMessage);
        }
      }
    },
    [hideFileUploadModal, setAgent, t, toast],
  );

  return {
    fileUploadVisible,
    handleImportJson: showFileUploadModal,
    hideFileUploadModal,
    onFileUploadOk,
    loading,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agents/use-import-json.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 84 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useHandleImportJsonFile`: Exported entity

### Functions (3)

- `hasNode()`: Function definition
- `useHandleImportJsonFile()`: Function definition
- `onFileUploadOk()`: Function definition

### Imports (12)

- `import { useToast } from '@/components/hooks/use-toast';`
- `import message from '@/components/ui/message';`
- `import { AgentCategory, DataflowOperator } from '@/constants/agent';`
- `import { FileMimeType } from '@/constants/common';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { EmptyDsl, useSetAgent } from '@/hooks/use-agent-request';`
- `import { Node } from '@xyflow/react';`
- `import isEmpty from 'lodash/isEmpty';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 84
- Blank lines: 10 (11.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~74


## Dependencies and Imports

- `@/components/hooks/use-toast`
- `@/components/ui/message`
- `@/constants/agent`
- `@/constants/common`
- `@/hooks/common-hooks`
- `@/hooks/use-agent-request`
- `@xyflow/react`
- `lodash/isEmpty`
- `react`
- `react-i18next`
- `./hooks/use-create-agent`
- `./upload-agent-dialog/upload-agent-form`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agents`.

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

- Other files in `web/src/pages/agents/` directory
- Potential test file: `test_use-import-json.ts`

## Keywords

./hooks/use-create-agent, ./upload-agent-dialog/upload-agent-form, @/components/hooks/use-toast, @/components/ui/message, @/constants/agent, @/constants/common, @/hooks/common-hooks, @/hooks/use-agent-request, @xyflow/react, AgentCanvas, AgentCategory, Array, Begin, DataflowCanvas, DataflowEmptyDsl, DataflowOperator, EmptyDsl, FileMimeType, FormSchemaType, JSON, Json, Node, Parser, TypeScript, dsl, errorMessage, file, graph, graphStr, hasNode, isAgent, lodash/isEmpty, nodes, onFileUploadOk, react, react-i18next, useHandleImportJsonFile, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
