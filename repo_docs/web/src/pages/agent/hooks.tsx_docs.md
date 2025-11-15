# File Documentation: web/src/pages/agent/hooks.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks.tsx`
- **Extension**: `.tsx`
- **Lines**: 275
- **Characters**: 8,117
- **Size**: 8,120 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Connection, Edge, getOutgoers } from '@xyflow/react';
import React, { useCallback, useEffect } from 'react';
// import { shallow } from 'zustand/shallow';
import { settledModelVariableMap } from '@/constants/knowledge';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { get, lowerFirst, omit } from 'lodash';
import { UseFormReturn } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { Operator, RestrictedUpstreamMap } from './constant';
import useGraphStore, { RFState } from './store';
import { buildCategorizeObjectFromList, replaceIdWithText } from './utils';

const selector = (state: RFState) => ({
  nodes: state.nodes,
  edges: state.edges,
  onNodesChange: state.onNodesChange,
  onEdgesChange: state.onEdgesChange,
  onConnect: state.onConnect,
  setNodes: state.setNodes,
  onSelectionChange: state.onSelectionChange,
  onEdgeMouseEnter: state.onEdgeMouseEnter,
  onEdgeMouseLeave: state.onEdgeMouseLeave,
});

export const useSelectCanvasData = () => {
  // return useStore(useShallow(selector)); // throw error
  // return useStore(selector, shallow);
  return useGraphStore(selector);
};

export const useHandleDrag = () => {
  const handleDragStart = useCallback(
    (operatorId: string) => (ev: React.DragEvent<HTMLDivElement>) => {
      ev.dataTransfer.setData('application/@xyflow/react', operatorId);
      ev.dataTransfer.effectAllowed = 'move';
    },
    [],
  );

  return { handleDragStart };
};

export const useGetNodeName = () => {
  const { t } = useTranslation();

  return (type: string) => {
    const name = t(`flow.${lowerFirst(type)}`);
    return name;
  };
};

export const useHandleFormValuesChange = (
  operatorName: Operator,
  id?: string,
  form?: UseFormReturn,
) => {
  const updateNodeForm = useGraphStore((state) => state.updateNodeForm);
  const handleValuesChange = useCallback(
    (changedValues: any, values: any) => {
      let nextValues: any = values;
      // Fixed the issue that the related form value does not change after selecting the freedom field of the model
      if (
        Object.keys(changedValues).length === 1 &&
        'parameter' in changedValues &&
        changedValues['parameter'] in settledModelVariableMap
      ) {
        nextValues = {
          ...values,
          ...settledModelVariableMap[
            changedValues['parameter'] as keyof typeof settledModelVariableMap
          ],
        };
      }
      if (id) {
        updateNodeForm(id, nextValues);
      }
    },
    [updateNodeForm, id],
  );

  useEffect(() => {
    const subscription = form?.watch((value, { name, type, values }) => {
      if (id && name) {
        console.log(
          '🚀 ~ useEffect ~ value:',
          name,
          type,
          values,
          operatorName,
        );
        let nextValues: any = value;

        // Fixed the issue that the related form value does not change after selecting the freedom field of the model
        if (
          name === 'parameter' &&
          value['parameter'] in settledModelVariableMap
        ) {
          nextValues = {
            ...value,
            ...settledModelVariableMap[
              value['parameter'] as keyof typeof settledModelVariableMap
            ],
          };
        }

        const categoryDescriptionRegex = /items\.\d+\.name/g;
        if (
          operatorName === Operator.Categorize &&
          categoryDescriptionRegex.test(name)
        ) {
          nextValues = {
            ...omit(value, 'items'),
            category_description: buildCategorizeObjectFromList(value.items),
          };
        }
        // Manually triggered form updates are synchronized to the canvas
        if (type) {
          updateNodeForm(id, nextValues);
        }
      }
    });
    return () => subscription?.unsubscribe();
  }, [form, form?.watch, id, operatorName, updateNodeForm]);

  return { handleValuesChange };
};

export const useValidateConnection = () => {
  const { getOperatorTypeFromId, getParentIdById, edges, nodes } =
    useGraphStore((state) => state);

  const isSameNodeChild = useCallback(
    (connection: Connection | Edge) => {
      const sourceParentId = getParentIdById(connection.source);
      const targetParentId = getParentIdById(connection.target);
      if (sourceParentId || targetParentId) {
        return sourceParentId === targetParentId;
      }
      return true;
    },
    [getParentIdById],
  );

  const hasCanvasCycle = useCallback(
    (connection: Connection | Edge) => {
      const target = nodes.find((node) => node.id === connection.target);
      const hasCycle = (node: RAGFlowNodeType, visited = new Set()) => {
        if (visited.has(node.id)) return false;

        visited.add(node.id);

        for (const outgoer of getOutgoers(node, nodes, edges)) {
          if (outgoer.id === connection.source) return true;
          if (hasCycle(outgoer, visited)) return true;
        }
      };

      if (target?.id === connection.source) return false;

      return target ? !hasCycle(target) : false;
    },
    [edges, nodes],
  );

  // restricted lines cannot be connected successfully.
  const isValidConnection = useCallback(
    (connection: Connection | Edge) => {
      // node cannot connect to itself
      const isSelfConnected = connection.target === connection.source;

      // limit the connection between two nodes to only one connection line in one direction
      // const hasLine = edges.some(
      //   (x) => x.source === connection.source && x.target === connection.target,
      // );

      const ret =
        !isSelfConnected &&
        RestrictedUpstreamMap[
          getOperatorTypeFromId(connection.source) as Operator
        ]?.every((x) => x !== getOperatorTypeFromId(connection.target)) &&
        isSameNodeChild(connection) &&
        hasCanvasCycle(connection);
      return ret;
    },
    [getOperatorTypeFromId, hasCanvasCycle, isSameNodeChild],
  );

  return isValidConnection;
};

export const useReplaceIdWithName = () => {
  const getNode = useGraphStore((state) => state.getNode);

  const replaceIdWithName = useCallback(
    (id?: string) => {
      return getNode(id)?.data.name;
    },
    [getNode],
  );

  return replaceIdWithName;
};

export const useReplaceIdWithText = (output: unknown) => {
  const getNameById = useReplaceIdWithName();

  return {
    replacedOutput: replaceIdWithText(output, getNameById),
    getNameById,
  };
};

export const useDuplicateNode = () => {
  const duplicateNodeById = useGraphStore((store) => store.duplicateNode);
  const getNodeName = useGetNodeName();

  const duplicateNode = useCallback(
    (id: string, label: string) => {
      duplicateNodeById(id, getNodeName(label));
    },
    [duplicateNodeById, getNodeName],
  );

  return duplicateNode;
};

export const useCopyPaste = () => {
  const nodes = useGraphStore((state) => state.nodes);
  const duplicateNode = useDuplicateNode();

  const onCopyCapture = useCallback(
    (event: ClipboardEvent) => {
      if (get(event, 'srcElement.tagName') !== 'BODY') return;

      event.preventDefault();
      const nodesStr = JSON.stringify(
        nodes.filter((n) => n.selected && n.data.label !== Operator.Begin),
      );

      event.clipboardData?.setData('agent:nodes', nodesStr);
    },
    [nodes],
  );

  const onPasteCapture = useCallback(
    (event: ClipboardEvent) => {
      const nodes = JSON.parse(
        event.clipboardData?.getData('agent:nodes') || '[]',
      ) as RAGFlowNodeType[] | undefined;

      if (Array.isArray(nodes) && nodes.length) {
        event.preventDefault();
        nodes.forEach((n) => {
          duplicateNode(n.id, n.data.label);
        });
      }
    },
    [duplicateNode],
  );

  useEffect(() => {
    window.addEventListener('copy', onCopyCapture);
    return () => {
      window.removeEventListener('copy', onCopyCapture);
    };
  }, [onCopyCapture]);

  useEffect(() => {
    window.addEventListener('paste', onPasteCapture);
    return () => {
      window.removeEventListener('paste', onPasteCapture);
    };
  }, [onPasteCapture]);
};

```

## High-Level Overview

// import { shallow } from 'zustand/shallow';

## Detailed Walkthrough

### Exports (9)

- `useSelectCanvasData`: Exported entity
- `useHandleDrag`: Exported entity
- `useGetNodeName`: Exported entity
- `useHandleFormValuesChange`: Exported entity
- `useValidateConnection`: Exported entity
- `useReplaceIdWithName`: Exported entity
- `useReplaceIdWithText`: Exported entity
- `useDuplicateNode`: Exported entity
- `useCopyPaste`: Exported entity

### Functions (29)

- `selector()`: Function definition
- `useSelectCanvasData()`: Function definition
- `useHandleDrag()`: Function definition
- `handleDragStart()`: Function definition
- `useGetNodeName()`: Function definition
- `useHandleFormValuesChange()`: Function definition
- `updateNodeForm()`: Function definition
- `handleValuesChange()`: Function definition
- `subscription()`: Function definition
- `useValidateConnection()`: Function definition
- `isSameNodeChild()`: Function definition
- `hasCanvasCycle()`: Function definition
- `target()`: Function definition
- `isValidConnection()`: Function definition
- `hasLine()`: Function definition
- `ret()`: Function definition
- `useReplaceIdWithName()`: Function definition
- `getNode()`: Function definition
- `replaceIdWithName()`: Function definition
- `useReplaceIdWithText()`: Function definition

### Imports (10)

- `import { Connection, Edge, getOutgoers } from '@xyflow/react';`
- `import React, { useCallback, useEffect } from 'react';`
- `import { settledModelVariableMap } from '@/constants/knowledge';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { get, lowerFirst, omit } from 'lodash';`
- `import { UseFormReturn } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { Operator, RestrictedUpstreamMap } from './constant';`
- `import useGraphStore, { RFState } from './store';`
- `import { buildCategorizeObjectFromList, replaceIdWithText } from './utils';`

## Code Structure Analysis

- Total lines: 275
- Blank lines: 39 (14.2%)
- Comment lines: ~12 (4.4%)
- Code lines: ~224


## Dependencies and Imports

- `@xyflow/react`
- `react`
- `zustand/shallow`
- `@/constants/knowledge`
- `@/interfaces/database/flow`
- `lodash`
- `react-hook-form`
- `react-i18next`
- `./constant`
- `./store`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/` directory
- Potential test file: `test_hooks.tsx`

## Keywords

./constant, ./store, ./utils, @/constants/knowledge, @/interfaces/database/flow, @xyflow/react, Array, BODY, Begin, Categorize, ClipboardEvent, Connection, DragEvent, Edge, Fixed, HTMLDivElement, JSON, Manually, Object, Operator, RAGFlowNodeType, RFState, React, RestrictedUpstreamMap, Set, TypeScript, UseFormReturn, categoryDescriptionRegex, duplicateNode, duplicateNodeById, getNameById, getNode, getNodeName, handleDragStart, handleValuesChange, hasCanvasCycle, hasCycle, hasLine, isSameNodeChild, isSelfConnected, isValidConnection, lodash, name, nextValues, nodes, nodesStr, onCopyCapture, onPasteCapture, outgoer, react...

---
*Generated by RAGFlow Repository Documentation Generator*
