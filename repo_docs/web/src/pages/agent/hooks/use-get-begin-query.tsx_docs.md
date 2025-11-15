# File Documentation: web/src/pages/agent/hooks/use-get-begin-query.tsx

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-get-begin-query.tsx`
- **Extension**: `.tsx`
- **Lines**: 364
- **Characters**: 10,463
- **Size**: 10,463 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { AgentGlobals } from '@/constants/agent';
import { useFetchAgent } from '@/hooks/use-agent-request';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { buildNodeOutputOptions } from '@/utils/canvas-util';
import { DefaultOptionType } from 'antd/es/select';
import { t } from 'i18next';
import { isEmpty, toLower } from 'lodash';
import get from 'lodash/get';
import { MessageSquareCode } from 'lucide-react';
import { useCallback, useContext, useEffect, useMemo, useState } from 'react';
import {
  AgentDialogueMode,
  BeginId,
  BeginQueryType,
  JsonSchemaDataType,
  Operator,
  VariableType,
} from '../constant';
import { AgentFormContext } from '../context';
import { buildBeginInputListFromObject } from '../form/begin-form/utils';
import { BeginQuery } from '../interface';
import OperatorIcon from '../operator-icon';
import useGraphStore from '../store';
import {
  useFindAgentStructuredOutputLabelByValue,
  useFindAgentStructuredOutputTypeByValue,
} from './use-build-structured-output';

export function useSelectBeginNodeDataInputs() {
  const getNode = useGraphStore((state) => state.getNode);

  return buildBeginInputListFromObject(
    getNode(BeginId)?.data?.form?.inputs ?? {},
  );
}

export function useIsTaskMode(isTask?: boolean) {
  const getNode = useGraphStore((state) => state.getNode);

  return useMemo(() => {
    if (typeof isTask === 'boolean') {
      return isTask;
    }
    const node = getNode(BeginId);
    return node?.data?.form?.mode === AgentDialogueMode.Task;
  }, [getNode, isTask]);
}

export const useGetBeginNodeDataQuery = () => {
  const getNode = useGraphStore((state) => state.getNode);

  const getBeginNodeDataQuery = useCallback(() => {
    return buildBeginInputListFromObject(
      get(getNode(BeginId), 'data.form.inputs', {}),
    );
  }, [getNode]);

  return getBeginNodeDataQuery;
};

export const useGetBeginNodeDataInputs = () => {
  const getNode = useGraphStore((state) => state.getNode);

  const inputs = get(getNode(BeginId), 'data.form.inputs', {});

  const beginNodeDataInputs = useMemo(() => {
    return buildBeginInputListFromObject(inputs);
  }, [inputs]);

  return beginNodeDataInputs;
};

export const useGetBeginNodeDataQueryIsSafe = () => {
  const [isBeginNodeDataQuerySafe, setIsBeginNodeDataQuerySafe] =
    useState(false);
  const inputs = useSelectBeginNodeDataInputs();
  const nodes = useGraphStore((state) => state.nodes);

  useEffect(() => {
    const query: BeginQuery[] = inputs;
    const isSafe = !query.some((q) => !q.optional && q.type === 'file');
    setIsBeginNodeDataQuerySafe(isSafe);
  }, [inputs, nodes]);

  return isBeginNodeDataQuerySafe;
};

export function useBuildNodeOutputOptions(nodeId?: string) {
  const nodes = useGraphStore((state) => state.nodes);
  const edges = useGraphStore((state) => state.edges);

  return useMemo(() => {
    return buildNodeOutputOptions({
      nodes,
      edges,
      nodeId,
      Icon: ({ name }) => <OperatorIcon name={name as Operator}></OperatorIcon>,
    });
  }, [edges, nodeId, nodes]);
}

// exclude nodes with branches
const ExcludedNodes = [
  Operator.Categorize,
  Operator.Relevant,
  Operator.Begin,
  Operator.Note,
];

const StringList = [
  BeginQueryType.Line,
  BeginQueryType.Paragraph,
  BeginQueryType.Options,
];

function transferToVariableType(type: string) {
  if (StringList.some((x) => x === type)) {
    return VariableType.String;
  }
  return type;
}

export function useBuildBeginVariableOptions() {
  const inputs = useSelectBeginNodeDataInputs();

  const options = useMemo(() => {
    return [
      {
        label: <span>{t('flow.beginInput')}</span>,
        title: t('flow.beginInput'),
        options: inputs.map((x) => ({
          label: x.name,
          parentLabel: <span>{t('flow.beginInput')}</span>,
          icon: <OperatorIcon name={Operator.Begin} className="block" />,
          value: `begin@${x.key}`,
          type: transferToVariableType(x.type),
        })),
      },
    ];
  }, [inputs]);

  return options;
}

const Env = 'env.';

export function useBuildConversationVariableOptions() {
  const { data } = useFetchAgent();

  const conversationVariables = useMemo(
    () => data?.dsl?.variables ?? {},
    [data?.dsl?.variables],
  );

  const options = useMemo(() => {
    return [
      {
        label: <span>{t('flow.conversationVariable')}</span>,
        title: t('flow.conversationVariable'),
        options: Object.entries(conversationVariables).map(([key, value]) => {
          const keyWithPrefix = `${Env}${key}`;
          return {
            label: keyWithPrefix,
            parentLabel: <span>{t('flow.conversationVariable')}</span>,
            icon: <MessageSquareCode className="size-3" />,
            value: keyWithPrefix,
            type: value.type,
          };
        }),
      },
    ];
  }, [conversationVariables]);

  return options;
}

export const useBuildVariableOptions = (nodeId?: string, parentId?: string) => {
  const nodeOutputOptions = useBuildNodeOutputOptions(nodeId);
  const parentNodeOutputOptions = useBuildNodeOutputOptions(parentId);
  const beginOptions = useBuildBeginVariableOptions();

  const options = useMemo(() => {
    return [...beginOptions, ...nodeOutputOptions, ...parentNodeOutputOptions];
  }, [beginOptions, nodeOutputOptions, parentNodeOutputOptions]);

  return options;
};

export function useBuildQueryVariableOptions(n?: RAGFlowNodeType) {
  const { data } = useFetchAgent();
  const node = useContext(AgentFormContext) || n;
  const options = useBuildVariableOptions(node?.id, node?.parentId);

  const conversationOptions = useBuildConversationVariableOptions();

  const nextOptions = useMemo(() => {
    const globals = data?.dsl?.globals ?? {};
    const globalOptions = Object.entries(globals)
      .filter(([key]) => !key.startsWith(Env))
      .map(([key, value]) => ({
        label: key,
        value: key,
        icon: <OperatorIcon name={Operator.Begin} className="block" />,
        parentLabel: <span>{t('flow.beginInput')}</span>,
        type: Array.isArray(value)
          ? `${VariableType.Array}${key === AgentGlobals.SysFiles ? '<file>' : ''}`
          : typeof value,
      }));

    return [
      {
        ...options[0],
        options: [...options[0]?.options, ...globalOptions],
      },
      ...options.slice(1),
      ...conversationOptions,
    ];
  }, [conversationOptions, data?.dsl?.globals, options]);

  return nextOptions;
}

export function useFilterQueryVariableOptionsByTypes(
  types?: JsonSchemaDataType[],
) {
  const nextOptions = useBuildQueryVariableOptions();

  const filteredOptions = useMemo(() => {
    return !isEmpty(types)
      ? nextOptions.map((x) => {
          return {
            ...x,
            options: x.options.filter(
              (y) =>
                types?.some((x) => toLower(y.type).includes(x)) ||
                y.type === undefined, // agent structured output
            ),
          };
        })
      : nextOptions;
  }, [nextOptions, types]);

  return filteredOptions;
}

export function useBuildComponentIdOptions(nodeId?: string, parentId?: string) {
  const nodes = useGraphStore((state) => state.nodes);

  // Limit the nodes inside iteration to only reference peer nodes with the same parentId and other external nodes other than their parent nodes
  const filterChildNodesToSameParentOrExternal = useCallback(
    (node: RAGFlowNodeType) => {
      // Node inside iteration
      if (parentId) {
        return (
          (node.parentId === parentId || node.parentId === undefined) &&
          node.id !== parentId
        );
      }

      return node.parentId === undefined; // The outermost node
    },
    [parentId],
  );

  const componentIdOptions = useMemo(() => {
    return nodes
      .filter(
        (x) =>
          x.id !== nodeId &&
          !ExcludedNodes.some((y) => y === x.data.label) &&
          filterChildNodesToSameParentOrExternal(x),
      )
      .map((x) => ({ label: x.data.name, value: x.id }));
  }, [nodes, nodeId, filterChildNodesToSameParentOrExternal]);

  return [
    {
      label: <span>Component Output</span>,
      title: 'Component Output',
      options: componentIdOptions,
    },
  ];
}

export function useBuildComponentIdAndBeginOptions(
  nodeId?: string,
  parentId?: string,
) {
  const componentIdOptions = useBuildComponentIdOptions(nodeId, parentId);
  const beginOptions = useBuildBeginVariableOptions();

  return [...beginOptions, ...componentIdOptions];
}

export const useGetComponentLabelByValue = (nodeId: string) => {
  const options = useBuildComponentIdAndBeginOptions(nodeId);

  const flattenOptions = useMemo(() => {
    return options.reduce<DefaultOptionType[]>((pre, cur) => {
      return [...pre, ...cur.options];
    }, []);
  }, [options]);

  const getLabel = useCallback(
    (val?: string) => {
      return flattenOptions.find((x) => x.value === val)?.label;
    },
    [flattenOptions],
  );
  return getLabel;
};

export function useFlattenQueryVariableOptions(nodeId?: string) {
  const { getNode } = useGraphStore((state) => state);
  const nextOptions = useBuildQueryVariableOptions(getNode(nodeId));

  const flattenOptions = useMemo(() => {
    return nextOptions.reduce<DefaultOptionType[]>((pre, cur) => {
      return [...pre, ...cur.options];
    }, []);
  }, [nextOptions]);

  return flattenOptions;
}

export function useGetVariableLabelOrTypeByValue(nodeId?: string) {
  const flattenOptions = useFlattenQueryVariableOptions(nodeId);
  const findAgentStructuredOutputTypeByValue =
    useFindAgentStructuredOutputTypeByValue();
  const findAgentStructuredOutputLabel =
    useFindAgentStructuredOutputLabelByValue();

  const getItem = useCallback(
    (val?: string) => {
      return flattenOptions.find((x) => x.value === val);
    },
    [flattenOptions],
  );

  const getLabel = useCallback(
    (val?: string) => {
      const item = getItem(val);
      if (item) {
        return (
          <div>
            {item.parentLabel} / {item.label}
          </div>
        );
      }
      return getItem(val)?.label || findAgentStructuredOutputLabel(val);
    },
    [findAgentStructuredOutputLabel, getItem],
  );

  const getType = useCallback(
    (val?: string) => {
      return getItem(val)?.type || findAgentStructuredOutputTypeByValue(val);
    },
    [findAgentStructuredOutputTypeByValue, getItem],
  );

  return { getLabel, getType };
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/hooks/use-get-begin-query.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 364 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (16)

- `useSelectBeginNodeDataInputs`: Exported entity
- `useIsTaskMode`: Exported entity
- `useGetBeginNodeDataQuery`: Exported entity
- `useGetBeginNodeDataInputs`: Exported entity
- `useGetBeginNodeDataQueryIsSafe`: Exported entity
- `useBuildNodeOutputOptions`: Exported entity
- `useBuildBeginVariableOptions`: Exported entity
- `useBuildConversationVariableOptions`: Exported entity
- `useBuildVariableOptions`: Exported entity
- `useBuildQueryVariableOptions`: Exported entity
- `useFilterQueryVariableOptionsByTypes`: Exported entity
- `useBuildComponentIdOptions`: Exported entity
- `useBuildComponentIdAndBeginOptions`: Exported entity
- `useGetComponentLabelByValue`: Exported entity
- `useFlattenQueryVariableOptions`: Exported entity
- `useGetVariableLabelOrTypeByValue`: Exported entity

### Functions (43)

- `useSelectBeginNodeDataInputs()`: Function definition
- `getNode()`: Function definition
- `useIsTaskMode()`: Function definition
- `getNode()`: Function definition
- `useGetBeginNodeDataQuery()`: Function definition
- `getNode()`: Function definition
- `getBeginNodeDataQuery()`: Function definition
- `useGetBeginNodeDataInputs()`: Function definition
- `getNode()`: Function definition
- `beginNodeDataInputs()`: Function definition
- `useGetBeginNodeDataQueryIsSafe()`: Function definition
- `nodes()`: Function definition
- `isSafe()`: Function definition
- `useBuildNodeOutputOptions()`: Function definition
- `nodes()`: Function definition
- `edges()`: Function definition
- `StringList()`: Function definition
- `useBuildBeginVariableOptions()`: Function definition
- `options()`: Function definition
- `useBuildConversationVariableOptions()`: Function definition

### Imports (17)

- `import { AgentGlobals } from '@/constants/agent';`
- `import { useFetchAgent } from '@/hooks/use-agent-request';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { buildNodeOutputOptions } from '@/utils/canvas-util';`
- `import { DefaultOptionType } from 'antd/es/select';`
- `import { t } from 'i18next';`
- `import { isEmpty, toLower } from 'lodash';`
- `import get from 'lodash/get';`
- `import { MessageSquareCode } from 'lucide-react';`
- `import { useCallback, useContext, useEffect, useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 364
- Blank lines: 57 (15.7%)
- Comment lines: ~3 (0.8%)
- Code lines: ~304


## Dependencies and Imports

- `@/constants/agent`
- `@/hooks/use-agent-request`
- `@/interfaces/database/flow`
- `@/utils/canvas-util`
- `antd/es/select`
- `i18next`
- `lodash`
- `lodash/get`
- `lucide-react`
- `react`
- `../context`
- `../form/begin-form/utils`
- `../interface`
- `../operator-icon`
- `../store`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-get-begin-query.tsx`

## Keywords

../context, ../form/begin-form/utils, ../interface, ../operator-icon, ../store, @/constants/agent, @/hooks/use-agent-request, @/interfaces/database/flow, @/utils/canvas-util, AgentDialogueMode, AgentFormContext, AgentGlobals, Array, Begin, BeginId, BeginQuery, BeginQueryType, Categorize, Component, DefaultOptionType, Env, ExcludedNodes, Icon, JsonSchemaDataType, Limit, Line, MessageSquareCode, Node, Note, Object, Operator, OperatorIcon, Options, Output, Paragraph, RAGFlowNodeType, Relevant, String, StringList, SysFiles, Task, The, TypeScript, VariableType, antd/es/select, beginNodeDataInputs, beginOptions, componentIdOptions, conversationOptions, conversationVariables...

---
*Generated by RAGFlow Repository Documentation Generator*
