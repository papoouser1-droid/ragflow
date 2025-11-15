# Documentation: web/src/pages/agent/hooks.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks.tsx`
- **Size**: 8120 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/hooks.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/hooks.tsx` is located in the `web/src/pages/agent` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to agent.

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

- [context.ts](context.ts_docs.md)
- [flow-tooltip.tsx](flow-tooltip.tsx_docs.md)
- [form-hooks.ts](form-hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [interface.ts](interface.ts_docs.md)
- [operator-icon.tsx](operator-icon.tsx_docs.md)
- [options.ts](options.ts_docs.md)
- [store.ts](store.ts_docs.md)
- [use-agent-history-manager.ts](use-agent-history-manager.ts_docs.md)
- [utils.ts](utils.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
