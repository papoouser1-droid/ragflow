# File Documentation: web/src/pages/agents/hooks/use-create-agent.ts

## File Metadata

- **Path**: `web/src/pages/agents/hooks/use-create-agent.ts`
- **Extension**: `.ts`
- **Lines**: 110
- **Characters**: 2,752
- **Size**: 2,752 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { AgentCategory, Operator } from '@/constants/agent';
import { useSetModalState } from '@/hooks/common-hooks';
import { EmptyDsl, useSetAgent } from '@/hooks/use-agent-request';
import { DSL } from '@/interfaces/database/agent';

import { FileId, initialParserValues } from '@/pages/agent/constant';
import { useCallback } from 'react';
import { FlowType } from '../constant';
import { FormSchemaType } from '../create-agent-form';

export const DataflowEmptyDsl = {
  graph: {
    nodes: [
      {
        id: FileId,
        type: 'beginNode',
        position: {
          x: 50,
          y: 200,
        },
        data: {
          label: Operator.File,
          name: Operator.File,
        },
        sourcePosition: 'left',
        targetPosition: 'right',
      },
      {
        data: {
          form: initialParserValues,
          label: 'Parser',
          name: 'Parser_0',
        },
        dragging: false,
        id: 'Parser:HipSignsRhyme',
        measured: {
          height: 57,
          width: 200,
        },
        position: {
          x: 316.99524094206413,
          y: 195.39629819663406,
        },
        selected: true,
        sourcePosition: 'right',
        targetPosition: 'left',
        type: 'parserNode',
      },
    ],
    edges: [
      {
        id: 'xy-edge__Filestart-Parser:HipSignsRhymeend',
        source: FileId,
        sourceHandle: 'start',
        target: 'Parser:HipSignsRhyme',
        targetHandle: 'end',
      },
    ],
  },
  components: {
    [Operator.File]: {
      obj: {
        component_name: Operator.File,
        params: {},
      },
      downstream: [], // other edge target is downstream, edge source is current node id
      upstream: [], // edge source is upstream, edge target is current node id
    },
  },
  retrieval: [], // reference
  history: [],
  path: [],
  globals: {},
};

export function useCreateAgentOrPipeline() {
  const { loading, setAgent } = useSetAgent();
  const {
    visible: creatingVisible,
    hideModal: hideCreatingModal,
    showModal: showCreatingModal,
  } = useSetModalState();

  const handleCreateAgentOrPipeline = useCallback(
    async (data: FormSchemaType) => {
      const isAgent = data.type === FlowType.Agent;
      const ret = await setAgent({
        title: data.name,
        dsl: isAgent ? (EmptyDsl as DSL) : (DataflowEmptyDsl as DSL),
        canvas_category: isAgent
          ? AgentCategory.AgentCanvas
          : AgentCategory.DataflowCanvas,
      });

      if (ret.code === 0) {
        hideCreatingModal();
      }
    },
    [hideCreatingModal, setAgent],
  );

  return {
    loading: loading,
    creatingVisible,
    hideCreatingModal,
    showCreatingModal,
    handleCreateAgentOrPipeline,
  };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agents/hooks/use-create-agent.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 110 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `DataflowEmptyDsl`: Exported entity
- `useCreateAgentOrPipeline`: Exported entity

### Functions (2)

- `useCreateAgentOrPipeline()`: Function definition
- `handleCreateAgentOrPipeline()`: Function definition

### Imports (8)

- `import { AgentCategory, Operator } from '@/constants/agent';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { EmptyDsl, useSetAgent } from '@/hooks/use-agent-request';`
- `import { DSL } from '@/interfaces/database/agent';`
- `import { FileId, initialParserValues } from '@/pages/agent/constant';`
- `import { useCallback } from 'react';`
- `import { FlowType } from '../constant';`
- `import { FormSchemaType } from '../create-agent-form';`

## Code Structure Analysis

- Total lines: 110
- Blank lines: 7 (6.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~103


## Dependencies and Imports

- `@/constants/agent`
- `@/hooks/common-hooks`
- `@/hooks/use-agent-request`
- `@/interfaces/database/agent`
- `@/pages/agent/constant`
- `react`
- `../constant`
- `../create-agent-form`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agents/hooks`.

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

- Other files in `web/src/pages/agents/hooks/` directory
- Potential test file: `test_use-create-agent.ts`

## Keywords

../constant, ../create-agent-form, @/constants/agent, @/hooks/common-hooks, @/hooks/use-agent-request, @/interfaces/database/agent, @/pages/agent/constant, Agent, AgentCanvas, AgentCategory, DSL, DataflowCanvas, DataflowEmptyDsl, EmptyDsl, File, FileId, FlowType, FormSchemaType, HipSignsRhyme, HipSignsRhymeend, Operator, Parser, Parser_0, TypeScript, handleCreateAgentOrPipeline, isAgent, react, ret, useCreateAgentOrPipeline

---
*Generated by RAGFlow Repository Documentation Generator*
