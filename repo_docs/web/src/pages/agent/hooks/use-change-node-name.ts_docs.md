# File Documentation: web/src/pages/agent/hooks/use-change-node-name.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-change-node-name.ts`
- **Extension**: `.ts`
- **Lines**: 121
- **Characters**: 2,984
- **Size**: 2,984 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { trim } from 'lodash';
import {
  ChangeEvent,
  Dispatch,
  SetStateAction,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from 'react';
import { Operator } from '../constant';
import useGraphStore from '../store';
import { getAgentNodeTools } from '../utils';

export function useHandleTooNodeNameChange({
  id,
  name,
  setName,
}: {
  id?: string;
  name?: string;
  setName: Dispatch<SetStateAction<string>>;
}) {
  const { clickedToolId, findUpstreamNodeById, updateNodeForm } = useGraphStore(
    (state) => state,
  );
  const agentNode = findUpstreamNodeById(id);
  const tools = getAgentNodeTools(agentNode);

  const previousName = useMemo(() => {
    const tool = tools.find((x) => x.component_name === clickedToolId);
    return tool?.name || tool?.component_name;
  }, [clickedToolId, tools]);

  const handleToolNameBlur = useCallback(() => {
    const trimmedName = trim(name);
    const existsSameName = tools.some((x) => x.name === trimmedName);
    if (trimmedName === '' || existsSameName) {
      if (existsSameName && previousName !== name) {
        message.error('The name cannot be repeated');
      }
      setName(previousName || '');
      return;
    }

    if (agentNode?.id) {
      const nextTools = tools.map((x) => {
        if (x.component_name === clickedToolId) {
          return {
            ...x,
            name,
          };
        }
        return x;
      });
      updateNodeForm(agentNode?.id, nextTools, ['tools']);
    }
  }, [
    agentNode?.id,
    clickedToolId,
    name,
    previousName,
    setName,
    tools,
    updateNodeForm,
  ]);

  return { handleToolNameBlur, previousToolName: previousName };
}

export const useHandleNodeNameChange = ({
  id,
  data,
}: {
  id?: string;
  data: any;
}) => {
  const [name, setName] = useState<string>('');
  const { updateNodeName, nodes, getOperatorTypeFromId } = useGraphStore(
    (state) => state,
  );
  const previousName = data?.name;
  const isToolNode = getOperatorTypeFromId(id) === Operator.Tool;

  const { handleToolNameBlur, previousToolName } = useHandleTooNodeNameChange({
    id,
    name,
    setName,
  });

  const handleNameBlur = useCallback(() => {
    const existsSameName = nodes.some((x) => x.data.name === name);
    if (trim(name) === '' || existsSameName) {
      if (existsSameName && previousName !== name) {
        message.error('The name cannot be repeated');
      }
      setName(previousName);
      return;
    }

    if (id) {
      updateNodeName(id, name);
    }
  }, [name, id, updateNodeName, previousName, nodes]);

  const handleNameChange = useCallback((e: ChangeEvent<any>) => {
    setName(e.target.value);
  }, []);

  useEffect(() => {
    setName(isToolNode ? previousToolName : previousName);
  }, [isToolNode, previousName, previousToolName]);

  return {
    name,
    handleNameBlur: isToolNode ? handleToolNameBlur : handleNameBlur,
    handleNameChange,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/hooks/use-change-node-name.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 121 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `useHandleTooNodeNameChange`: Exported entity
- `useHandleNodeNameChange`: Exported entity

### Functions (10)

- `useHandleTooNodeNameChange()`: Function definition
- `previousName()`: Function definition
- `tool()`: Function definition
- `handleToolNameBlur()`: Function definition
- `existsSameName()`: Function definition
- `nextTools()`: Function definition
- `useHandleNodeNameChange()`: Function definition
- `handleNameBlur()`: Function definition
- `existsSameName()`: Function definition
- `handleNameChange()`: Function definition

### Imports (6)

- `import message from '@/components/ui/message';`
- `import { trim } from 'lodash';`
- `import {`
- `import { Operator } from '../constant';`
- `import useGraphStore from '../store';`
- `import { getAgentNodeTools } from '../utils';`

## Code Structure Analysis

- Total lines: 121
- Blank lines: 13 (10.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~108


## Dependencies and Imports

- `@/components/ui/message`
- `lodash`
- `../constant`
- `../store`
- `../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-change-node-name.ts`

## Keywords

../constant, ../store, ../utils, @/components/ui/message, ChangeEvent, Dispatch, Operator, SetStateAction, The, Tool, TypeScript, agentNode, existsSameName, handleNameBlur, handleNameChange, handleToolNameBlur, isToolNode, lodash, nextTools, previousName, tool, tools, trimmedName, useHandleNodeNameChange, useHandleTooNodeNameChange

---
*Generated by RAGFlow Repository Documentation Generator*
