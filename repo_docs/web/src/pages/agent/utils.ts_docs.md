# File Documentation: web/src/pages/agent/utils.ts

## File Metadata

- **Path**: `web/src/pages/agent/utils.ts`
- **Extension**: `.ts`
- **Lines**: 735
- **Characters**: 20,415
- **Size**: 20,415 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  DSL,
  GlobalVariableType,
  IAgentForm,
  ICategorizeForm,
  ICategorizeItem,
  ICategorizeItemResult,
} from '@/interfaces/database/agent';
import { DSLComponents, RAGFlowNodeType } from '@/interfaces/database/flow';
import { removeUselessFieldsFromValues } from '@/utils/form';
import { Edge, Node, XYPosition } from '@xyflow/react';
import { FormInstance, FormListFieldData } from 'antd';
import { humanId } from 'human-id';
import {
  curry,
  get,
  intersectionWith,
  isEmpty,
  isEqual,
  omit,
  sample,
} from 'lodash';
import pipe from 'lodash/fp/pipe';
import isObject from 'lodash/isObject';
import {
  CategorizeAnchorPointPositions,
  FileType,
  FileTypeSuffixMap,
  NoCopyOperatorsList,
  NoDebugOperatorsList,
  NodeHandleId,
  Operator,
} from './constant';
import { DataOperationsFormSchemaType } from './form/data-operations-form';
import { ExtractorFormSchemaType } from './form/extractor-form';
import { HierarchicalMergerFormSchemaType } from './form/hierarchical-merger-form';
import { ParserFormSchemaType } from './form/parser-form';
import { SplitterFormSchemaType } from './form/splitter-form';
import { BeginQuery, IPosition } from './interface';

function buildAgentExceptionGoto(edges: Edge[], nodeId: string) {
  const exceptionEdges = edges.filter(
    (x) =>
      x.source === nodeId && x.sourceHandle === NodeHandleId.AgentException,
  );

  return exceptionEdges.map((x) => x.target);
}

const buildComponentDownstreamOrUpstream = (
  edges: Edge[],
  nodeId: string,
  isBuildDownstream = true,
  nodes: Node[],
) => {
  return edges
    .filter((y) => {
      const node = nodes.find((x) => x.id === nodeId);
      let isNotUpstreamTool = true;
      let isNotUpstreamAgent = true;
      let isNotExceptionGoto = true;
      if (isBuildDownstream && node?.data.label === Operator.Agent) {
        isNotExceptionGoto = y.sourceHandle !== NodeHandleId.AgentException;
        // Exclude the tool operator downstream of the agent operator
        isNotUpstreamTool = !y.target.startsWith(Operator.Tool);
        // Exclude the agent operator downstream of the agent operator
        isNotUpstreamAgent = !(
          y.target.startsWith(Operator.Agent) &&
          y.targetHandle === NodeHandleId.AgentTop
        );
      }
      return (
        y[isBuildDownstream ? 'source' : 'target'] === nodeId &&
        isNotUpstreamTool &&
        isNotUpstreamAgent &&
        isNotExceptionGoto
      );
    })
    .map((y) => y[isBuildDownstream ? 'target' : 'source']);
};

const removeUselessDataInTheOperator = curry(
  (operatorName: string, params: Record<string, unknown>) => {
    if (operatorName === Operator.Categorize) {
      return removeUselessFieldsFromValues(params, '');
    }
    return params;
  },
);
// initialize data for operators without parameters
// const initializeOperatorParams = curry((operatorName: string, values: any) => {
//   if (isEmpty(values)) {
//     return initialFormValuesMap[operatorName as Operator];
//   }
//   return values;
// });

function buildAgentTools(edges: Edge[], nodes: Node[], nodeId: string) {
  const node = nodes.find((x) => x.id === nodeId);
  const params = { ...(node?.data.form ?? {}) };
  if (node && node.data.label === Operator.Agent) {
    const bottomSubAgentEdges = edges.filter(
      (x) => x.source === nodeId && x.sourceHandle === NodeHandleId.AgentBottom,
    );

    (params as IAgentForm).tools = (params as IAgentForm).tools.concat(
      bottomSubAgentEdges.map((x) => {
        const {
          params: formData,
          id,
          name,
        } = buildAgentTools(edges, nodes, x.target);

        return {
          component_name: Operator.Agent,
          id,
          name: name as string, // Cast name to string and provide fallback
          params: { ...formData },
        };
      }),
    );
  }
  return { params, name: node?.data.name, id: node?.id };
}

function filterTargetsBySourceHandleId(edges: Edge[], handleId: string) {
  return edges.filter((x) => x.sourceHandle === handleId).map((x) => x.target);
}

function buildCategorize(edges: Edge[], nodes: Node[], nodeId: string) {
  const node = nodes.find((x) => x.id === nodeId);
  const params = { ...(node?.data.form ?? {}) } as ICategorizeForm;
  if (node && node.data.label === Operator.Categorize) {
    const subEdges = edges.filter((x) => x.source === nodeId);

    const items = params.items || [];

    const nextCategoryDescription = items.reduce<
      ICategorizeForm['category_description']
    >((pre, val) => {
      const key = val.name;
      pre[key] = {
        ...omit(val, 'name', 'uuid'),
        examples: val.examples?.map((x) => x.value) || [],
        to: filterTargetsBySourceHandleId(subEdges, val.uuid),
      };
      return pre;
    }, {});

    params.category_description = nextCategoryDescription;
  }
  return omit(params, 'items');
}

const buildOperatorParams = (operatorName: string) =>
  pipe(
    removeUselessDataInTheOperator(operatorName),
    // initializeOperatorParams(operatorName), // Final processing, for guarantee
  );

const ExcludeOperators = [Operator.Note, Operator.Tool, Operator.Placeholder];

export function isBottomSubAgent(edges: Edge[], nodeId?: string) {
  const edge = edges.find(
    (x) => x.target === nodeId && x.targetHandle === NodeHandleId.AgentTop,
  );
  return !!edge;
}

export function hasSubAgentOrTool(edges: Edge[], nodeId?: string) {
  const edge = edges.find(
    (x) =>
      x.source === nodeId &&
      (x.sourceHandle === NodeHandleId.Tool ||
        x.sourceHandle === NodeHandleId.AgentBottom),
  );
  return !!edge;
}

export function hasSubAgent(edges: Edge[], nodeId?: string) {
  const edge = edges.find(
    (x) => x.source === nodeId && x.sourceHandle === NodeHandleId.AgentBottom,
  );
  return !!edge;
}

// Because the array of react-hook-form must be object data,
// it needs to be converted into a simple data type array required by the backend
function transformObjectArrayToPureArray(
  list: Array<Record<string, any>>,
  field: string,
) {
  return Array.isArray(list)
    ? list.filter((x) => !isEmpty(x[field])).map((y) => y[field])
    : [];
}

function transformParserParams(params: ParserFormSchemaType) {
  const setups = params.setups.reduce<
    Record<string, ParserFormSchemaType['setups'][0]>
  >((pre, cur) => {
    if (cur.fileFormat) {
      let filteredSetup: Partial<
        ParserFormSchemaType['setups'][0] & { suffix: string[] }
      > = {
        output_format: cur.output_format,
        suffix: FileTypeSuffixMap[cur.fileFormat as FileType],
      };

      switch (cur.fileFormat) {
        case FileType.PDF:
          filteredSetup = {
            ...filteredSetup,
            parse_method: cur.parse_method,
            lang: cur.lang,
          };
          break;
        case FileType.Image:
          filteredSetup = {
            ...filteredSetup,
            parse_method: cur.parse_method,
            lang: cur.lang,
            system_prompt: cur.system_prompt,
          };
          break;
        case FileType.Email:
          filteredSetup = {
            ...filteredSetup,
            fields: cur.fields,
          };
          break;
        case FileType.Video:
        case FileType.Audio:
          filteredSetup = {
            ...filteredSetup,
            llm_id: cur.llm_id,
          };
          break;
        default:
          break;
      }

      pre[cur.fileFormat] = filteredSetup;
    }
    return pre;
  }, {});

  return { ...params, setups };
}

function transformSplitterParams(params: SplitterFormSchemaType) {
  return {
    ...params,
    overlapped_percent: Number(params.overlapped_percent) / 100,
    delimiters: transformObjectArrayToPureArray(params.delimiters, 'value'),
  };
}

function transformHierarchicalMergerParams(
  params: HierarchicalMergerFormSchemaType,
) {
  const levels = params.levels.map((x) =>
    transformObjectArrayToPureArray(x.expressions, 'expression'),
  );

  return { ...params, hierarchy: Number(params.hierarchy), levels };
}

function transformExtractorParams(params: ExtractorFormSchemaType) {
  return { ...params, prompts: [{ content: params.prompts, role: 'user' }] };
}

function transformDataOperationsParams(params: DataOperationsFormSchemaType) {
  return {
    ...params,
    select_keys: params?.select_keys?.map((x) => x.name),
    remove_keys: params?.remove_keys?.map((x) => x.name),
    query: params.query.map((x) => x.input),
  };
}

// construct a dsl based on the node information of the graph
export const buildDslComponentsByGraph = (
  nodes: RAGFlowNodeType[],
  edges: Edge[],
  oldDslComponents: DSLComponents,
): DSLComponents => {
  const components: DSLComponents = {};

  nodes
    ?.filter(
      (x) =>
        !ExcludeOperators.some((y) => y === x.data.label) &&
        !isBottomSubAgent(edges, x.id),
    )
    .forEach((x) => {
      const id = x.id;
      const operatorName = x.data.label;
      let params = x?.data.form ?? {};

      switch (operatorName) {
        case Operator.Agent: {
          const { params: formData } = buildAgentTools(edges, nodes, id);
          params = {
            ...formData,
            exception_goto: buildAgentExceptionGoto(edges, id),
          };
          break;
        }
        case Operator.Categorize:
          params = buildCategorize(edges, nodes, id);
          break;

        case Operator.Parser:
          params = transformParserParams(params);
          break;

        case Operator.Splitter:
          params = transformSplitterParams(params);
          break;

        case Operator.HierarchicalMerger:
          params = transformHierarchicalMergerParams(params);
          break;
        case Operator.Extractor:
          params = transformExtractorParams(params);
          break;
        case Operator.DataOperations:
          params = transformDataOperationsParams(params);
          break;
        default:
          break;
      }

      components[id] = {
        obj: {
          ...(oldDslComponents[id]?.obj ?? {}),
          component_name: operatorName,
          params: buildOperatorParams(operatorName)(params) ?? {},
        },
        downstream: buildComponentDownstreamOrUpstream(edges, id, true, nodes),
        upstream: buildComponentDownstreamOrUpstream(edges, id, false, nodes),
        parent_id: x?.parentId,
      };
    });

  return components;
};

export const buildDslGlobalVariables = (
  dsl: DSL,
  globalVariables?: Record<string, GlobalVariableType>,
) => {
  if (!globalVariables) {
    return { globals: dsl.globals, variables: dsl.variables || {} };
  }

  let globalVariablesTemp: Record<string, any> = {};
  let globalSystem: Record<string, any> = {};
  Object.keys(dsl.globals)?.forEach((key) => {
    if (key.indexOf('sys') > -1) {
      globalSystem[key] = dsl.globals[key];
    }
  });
  Object.keys(globalVariables).forEach((key) => {
    globalVariablesTemp['env.' + key] = globalVariables[key].value;
  });

  const globalVariablesResult = {
    ...globalSystem,
    ...globalVariablesTemp,
  };
  return { globals: globalVariablesResult, variables: globalVariables };
};

export const receiveMessageError = (res: any) =>
  res && (res?.response.status !== 200 || res?.data?.code !== 0);

// Replace the id in the object with text
export const replaceIdWithText = (
  obj: Record<string, unknown> | unknown[] | unknown,
  getNameById: (id?: string) => string | undefined,
) => {
  if (isObject(obj)) {
    const ret: Record<string, unknown> | unknown[] = Array.isArray(obj)
      ? []
      : {};
    Object.keys(obj).forEach((key) => {
      const val = (obj as Record<string, unknown>)[key];
      const text = typeof val === 'string' ? getNameById(val) : undefined;
      (ret as Record<string, unknown>)[key] = text
        ? text
        : replaceIdWithText(val, getNameById);
    });

    return ret;
  }

  return obj;
};

export const isEdgeEqual = (previous: Edge, current: Edge) =>
  previous.source === current.source &&
  previous.target === current.target &&
  previous.sourceHandle === current.sourceHandle;

export const buildNewPositionMap = (
  currentKeys: string[],
  previousPositionMap: Record<string, IPosition>,
) => {
  // index in use
  const indexesInUse = Object.values(previousPositionMap).map((x) => x.idx);
  const previousKeys = Object.keys(previousPositionMap);
  const intersectionKeys = intersectionWith(
    previousKeys,
    currentKeys,
    (categoryDataKey: string, positionMapKey: string) =>
      categoryDataKey === positionMapKey,
  );
  // difference set
  const currentDifferenceKeys = currentKeys.filter(
    (x) => !intersectionKeys.some((y: string) => y === x),
  );
  const newPositionMap = currentDifferenceKeys.reduce<
    Record<string, IPosition>
  >((pre, cur) => {
    // take a coordinate
    const effectiveIdxes = CategorizeAnchorPointPositions.map(
      (x, idx) => idx,
    ).filter((x) => !indexesInUse.some((y) => y === x));
    const idx = sample(effectiveIdxes);
    if (idx !== undefined) {
      indexesInUse.push(idx);
      pre[cur] = { ...CategorizeAnchorPointPositions[idx], idx };
    }

    return pre;
  }, {});

  return { intersectionKeys, newPositionMap };
};

export const isKeysEqual = (currentKeys: string[], previousKeys: string[]) => {
  return isEqual(currentKeys.sort(), previousKeys.sort());
};

export const getOperatorIndex = (handleTitle: string) => {
  return handleTitle.split(' ').at(-1);
};

// Get the value of other forms except itself
export const getOtherFieldValues = (
  form: FormInstance,
  formListName: string = 'items',
  field: FormListFieldData,
  latestField: string,
) =>
  (form.getFieldValue([formListName]) ?? [])
    .map((x: any) => {
      return get(x, latestField);
    })
    .filter(
      (x: string) =>
        x !== form.getFieldValue([formListName, field.name, latestField]),
    );

export const generateSwitchHandleText = (idx: number) => {
  return `Case ${idx + 1}`;
};

export const getNodeDragHandle = (nodeType?: string) => {
  return nodeType === Operator.Note ? '.note-drag-handle' : undefined;
};

const splitName = (name: string) => {
  const names = name.split('_');
  const type = names.at(0);
  const index = Number(names.at(-1));

  return { type, index };
};

export const generateNodeNamesWithIncreasingIndex = (
  name: string,
  nodes: RAGFlowNodeType[],
) => {
  const templateNameList = nodes
    .filter((x) => {
      const temporaryName = x.data.name;

      const { type, index } = splitName(temporaryName);

      return (
        temporaryName.match(/_/g)?.length === 1 &&
        type === name &&
        !isNaN(index)
      );
    })
    .map((x) => {
      const temporaryName = x.data.name;
      const { index } = splitName(temporaryName);

      return {
        idx: index,
        name: temporaryName,
      };
    })
    .sort((a, b) => a.idx - b.idx);

  let index: number = 0;
  for (let i = 0; i < templateNameList.length; i++) {
    const idx = templateNameList[i]?.idx;
    const nextIdx = templateNameList[i + 1]?.idx;
    if (idx + 1 !== nextIdx) {
      index = idx + 1;
      break;
    }
  }

  return `${name}_${index}`;
};

export const duplicateNodeForm = (nodeData?: RAGFlowNodeType['data']) => {
  const form: Record<string, any> = { ...(nodeData?.form ?? {}) };

  // Delete the downstream node corresponding to the to field of the Categorize operator
  if (nodeData?.label === Operator.Categorize) {
    form.category_description = Object.keys(form.category_description).reduce<
      Record<string, Record<string, any>>
    >((pre, cur) => {
      pre[cur] = {
        ...form.category_description[cur],
        to: undefined,
      };
      return pre;
    }, {});
  }

  // Delete the downstream nodes corresponding to the yes and no fields of the Relevant operator
  if (nodeData?.label === Operator.Relevant) {
    form.yes = undefined;
    form.no = undefined;
  }

  return {
    ...(nodeData ?? { label: '' }),
    form,
  };
};

export const getDrawerWidth = () => {
  return window.innerWidth > 1278 ? '40%' : 470;
};

export const needsSingleStepDebugging = (label: string) => {
  return !NoDebugOperatorsList.some((x) => (label as Operator) === x);
};

export function showCopyIcon(label: string) {
  return !NoCopyOperatorsList.some((x) => (label as Operator) === x);
}

// Get the coordinates of the node relative to the Iteration node
export function getRelativePositionToIterationNode(
  nodes: RAGFlowNodeType[],
  position?: XYPosition, // relative position
) {
  if (!position) {
    return;
  }

  const iterationNodes = nodes.filter(
    (node) => node.data.label === Operator.Iteration,
  );

  for (const iterationNode of iterationNodes) {
    const {
      position: { x, y },
      width,
      height,
    } = iterationNode;
    const halfWidth = (width || 0) / 2;
    if (
      position.x >= x - halfWidth &&
      position.x <= x + halfWidth &&
      position.y >= y &&
      position.y <= y + (height || 0)
    ) {
      return {
        parentId: iterationNode.id,
        position: { x: position.x - x + halfWidth, y: position.y - y },
      };
    }
  }
}

export const generateDuplicateNode = (
  position?: XYPosition,
  label?: string,
) => {
  const nextPosition = {
    x: (position?.x || 0) + 50,
    y: (position?.y || 0) + 50,
  };

  return {
    selected: false,
    dragging: false,
    id: `${label}:${humanId()}`,
    position: nextPosition,
    dragHandle: getNodeDragHandle(label),
  };
};

export function convertToStringArray(
  list?: Array<{ value: string | number | boolean }>,
) {
  if (!Array.isArray(list)) {
    return [];
  }
  return list.map((x) => x.value);
}

export function convertToObjectArray<T extends string | number | boolean>(
  list: Array<T>,
) {
  if (!Array.isArray(list)) {
    return [];
  }
  return list.map((x) => ({ value: x }));
}

/**
   * convert the following object into a list
   * 
   * {
      "product_related": {
      "description": "The question is about product usage, appearance and how it works.",
      "examples": "Why it always beaming?\nHow to install it onto the wall?\nIt leaks, what to do?",
      "to": "generate:0"
      }
      }
*/
export const buildCategorizeListFromObject = (
  categorizeItem: ICategorizeItemResult,
) => {
  // Categorize's to field has two data sources, with edges as the data source.
  // Changes in the edge or to field need to be synchronized to the form field.
  return Object.keys(categorizeItem)
    .reduce<Array<Omit<ICategorizeItem, 'uuid'>>>((pre, cur) => {
      // synchronize edge data to the to field

      pre.push({
        name: cur,
        ...categorizeItem[cur],
        examples: convertToObjectArray(categorizeItem[cur].examples),
      });
      return pre;
    }, [])
    .sort((a, b) => a.index - b.index);
};

/**
   * Convert the list in the following form into an object
   * {
    "items": [
      {
        "name": "Categorize 1",
        "description": "111",
        "examples": ["ddd"],
        "to": "Retrieval:LazyEelsStick"
      }
     ]
    }
*/
export const buildCategorizeObjectFromList = (list: Array<ICategorizeItem>) => {
  return list.reduce<ICategorizeItemResult>((pre, cur) => {
    if (cur?.name) {
      pre[cur.name] = {
        ...omit(cur, 'name', 'examples'),
        examples: convertToStringArray(cur.examples) as string[],
      };
    }
    return pre;
  }, {});
};

export function getAgentNodeTools(agentNode?: RAGFlowNodeType) {
  const tools: IAgentForm['tools'] = get(agentNode, 'data.form.tools', []);
  return tools;
}

export function getAgentNodeMCP(agentNode?: RAGFlowNodeType) {
  const tools: IAgentForm['mcp'] = get(agentNode, 'data.form.mcp', []);
  return tools;
}

export function mapEdgeMouseEvent(
  edges: Edge[],
  edgeId: string,
  isHovered: boolean,
) {
  const nextEdges = edges.map((element) =>
    element.id === edgeId
      ? {
          ...element,
          data: {
            ...element.data,
            isHovered,
          },
        }
      : element,
  );

  return nextEdges;
}

export function buildBeginQueryWithObject(
  inputs: Record<string, BeginQuery>,
  values: BeginQuery[],
) {
  const nextInputs = Object.keys(inputs).reduce<Record<string, BeginQuery>>(
    (pre, key) => {
      const item = values.find((x) => x.key === key);
      if (item) {
        pre[key] = { ...item };
      }
      return pre;
    },
    {},
  );

  return nextInputs;
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/utils.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 735 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (29)

- `isBottomSubAgent`: Exported entity
- `hasSubAgentOrTool`: Exported entity
- `hasSubAgent`: Exported entity
- `buildDslComponentsByGraph`: Exported entity
- `buildDslGlobalVariables`: Exported entity
- `receiveMessageError`: Exported entity
- `replaceIdWithText`: Exported entity
- `isEdgeEqual`: Exported entity
- `buildNewPositionMap`: Exported entity
- `isKeysEqual`: Exported entity
- `getOperatorIndex`: Exported entity
- `getOtherFieldValues`: Exported entity
- `generateSwitchHandleText`: Exported entity
- `getNodeDragHandle`: Exported entity
- `generateNodeNamesWithIncreasingIndex`: Exported entity
- `duplicateNodeForm`: Exported entity
- `getDrawerWidth`: Exported entity
- `needsSingleStepDebugging`: Exported entity
- `showCopyIcon`: Exported entity
- `getRelativePositionToIterationNode`: Exported entity

### Functions (66)

- `buildAgentExceptionGoto()`: Function definition
- `exceptionEdges()`: Function definition
- `buildComponentDownstreamOrUpstream()`: Function definition
- `node()`: Function definition
- `removeUselessDataInTheOperator()`: Function definition
- `initializeOperatorParams()`: Function definition
- `buildAgentTools()`: Function definition
- `node()`: Function definition
- `bottomSubAgentEdges()`: Function definition
- `filterTargetsBySourceHandleId()`: Function definition
- `buildCategorize()`: Function definition
- `node()`: Function definition
- `subEdges()`: Function definition
- `nextCategoryDescription()`: Function definition
- `buildOperatorParams()`: Function definition
- `isBottomSubAgent()`: Function definition
- `edge()`: Function definition
- `hasSubAgentOrTool()`: Function definition
- `edge()`: Function definition
- `hasSubAgent()`: Function definition

### Imports (16)

- `import {`
- `import { DSLComponents, RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { removeUselessFieldsFromValues } from '@/utils/form';`
- `import { Edge, Node, XYPosition } from '@xyflow/react';`
- `import { FormInstance, FormListFieldData } from 'antd';`
- `import { humanId } from 'human-id';`
- `import {`
- `import pipe from 'lodash/fp/pipe';`
- `import isObject from 'lodash/isObject';`
- `import {`

## Code Structure Analysis

- Total lines: 735
- Blank lines: 83 (11.3%)
- Comment lines: ~33 (4.5%)
- Code lines: ~619


## Dependencies and Imports

- `@/interfaces/database/flow`
- `@/utils/form`
- `@xyflow/react`
- `antd`
- `human-id`
- `lodash/fp/pipe`
- `lodash/isObject`
- `./form/data-operations-form`
- `./form/extractor-form`
- `./form/hierarchical-merger-form`
- `./form/parser-form`
- `./form/splitter-form`
- `./interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity
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

- Other files in `web/src/pages/agent/` directory
- Potential test file: `test_utils.ts`

## Keywords

./form/data-operations-form, ./form/extractor-form, ./form/hierarchical-merger-form, ./form/parser-form, ./form/splitter-form, ./interface, @/interfaces/database/flow, @/utils/form, @xyflow/react, Agent, AgentBottom, AgentException, AgentTop, Array, Audio, Because, BeginQuery, Case, Cast, Categorize, CategorizeAnchorPointPositions, Changes, Convert, DSL, DSLComponents, DataOperations, DataOperationsFormSchemaType, Delete, Edge, Email, Exclude, ExcludeOperators, Extractor, ExtractorFormSchemaType, FileType, FileTypeSuffixMap, Final, FormInstance, FormListFieldData, Get, GlobalVariableType, HierarchicalMerger, HierarchicalMergerFormSchemaType, IAgentForm, ICategorizeForm, ICategorizeItem, ICategorizeItemResult, IPosition, Image, Iteration...

---
*Generated by RAGFlow Repository Documentation Generator*
