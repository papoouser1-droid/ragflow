# Documentation: web/src/pages/agent/hooks/use-build-structured-output.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-build-structured-output.ts`
- **Size**: 4602 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/hooks/use-build-structured-output.ts`.

## Original Source Code

```ts
import { get, isPlainObject } from 'lodash';
import { ReactNode, useCallback } from 'react';
import {
  AgentStructuredOutputField,
  JsonSchemaDataType,
  Operator,
} from '../constant';
import useGraphStore from '../store';

function getNodeId(value: string) {
  return value.split('@').at(0);
}

export function useShowSecondaryMenu() {
  const { getOperatorTypeFromId } = useGraphStore((state) => state);

  const showSecondaryMenu = useCallback(
    (value: string, outputLabel: string) => {
      const nodeId = getNodeId(value);
      return (
        getOperatorTypeFromId(nodeId) === Operator.Agent &&
        outputLabel === AgentStructuredOutputField
      );
    },
    [getOperatorTypeFromId],
  );

  return showSecondaryMenu;
}

export function useGetStructuredOutputByValue() {
  const { getNode } = useGraphStore((state) => state);

  const getStructuredOutput = useCallback(
    (value: string) => {
      const node = getNode(getNodeId(value));
      const structuredOutput = get(
        node,
        `data.form.outputs.${AgentStructuredOutputField}`,
      );

      return structuredOutput;
    },
    [getNode],
  );

  return getStructuredOutput;
}

export function useFindAgentStructuredOutputLabel() {
  const getOperatorTypeFromId = useGraphStore(
    (state) => state.getOperatorTypeFromId,
  );

  const findAgentStructuredOutputLabel = useCallback(
    (
      value: string,
      options: Array<{
        label: string;
        value: string;
        parentLabel?: string | ReactNode;
        icon?: ReactNode;
      }>,
    ) => {
      // agent structured output
      const fields = value.split('@');
      if (
        getOperatorTypeFromId(fields.at(0)) === Operator.Agent &&
        fields.at(1)?.startsWith(AgentStructuredOutputField)
      ) {
        // is agent structured output
        const agentOption = options.find((x) => value.includes(x.value));
        const jsonSchemaFields = fields
          .at(1)
          ?.slice(AgentStructuredOutputField.length);

        return {
          ...agentOption,
          label: (agentOption?.label ?? '') + jsonSchemaFields,
          value: value,
        };
      }
    },
    [getOperatorTypeFromId],
  );

  return findAgentStructuredOutputLabel;
}

export function useFindAgentStructuredOutputTypeByValue() {
  const { getOperatorTypeFromId } = useGraphStore((state) => state);
  const filterStructuredOutput = useGetStructuredOutputByValue();

  const findTypeByValue = useCallback(
    (
      values: unknown,
      target: string,
      path: string = '',
    ): string | undefined => {
      const properties =
        get(values, 'properties') || get(values, 'items.properties');

      if (isPlainObject(values) && properties) {
        for (const [key, value] of Object.entries(properties)) {
          const nextPath = path ? `${path}.${key}` : key;
          const dataType = get(value, 'type');

          if (nextPath === target) {
            return dataType;
          }

          if (
            [JsonSchemaDataType.Object, JsonSchemaDataType.Array].some(
              (x) => x === dataType,
            )
          ) {
            const type = findTypeByValue(value, target, nextPath);
            if (type) {
              return type;
            }
          }
        }
      }
    },
    [],
  );

  const findAgentStructuredOutputTypeByValue = useCallback(
    (value?: string) => {
      if (!value) {
        return;
      }
      const fields = value.split('@');
      const nodeId = fields.at(0);
      const jsonSchema = filterStructuredOutput(value);

      if (
        getOperatorTypeFromId(nodeId) === Operator.Agent &&
        fields.at(1)?.startsWith(AgentStructuredOutputField)
      ) {
        const jsonSchemaFields = fields
          .at(1)
          ?.slice(AgentStructuredOutputField.length + 1);

        if (jsonSchemaFields) {
          const type = findTypeByValue(jsonSchema, jsonSchemaFields);
          return type;
        }
      }
    },
    [filterStructuredOutput, findTypeByValue, getOperatorTypeFromId],
  );

  return findAgentStructuredOutputTypeByValue;
}

export function useFindAgentStructuredOutputLabelByValue() {
  const { getNode } = useGraphStore((state) => state);

  const findAgentStructuredOutputLabel = useCallback(
    (value?: string) => {
      if (value) {
        const operatorName = getNode(getNodeId(value ?? ''))?.data.name;

        if (operatorName) {
          return operatorName + ' / ' + value?.split('@').at(1);
        }
      }

      return '';
    },
    [getNode],
  );

  return findAgentStructuredOutputLabel;
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/hooks/use-build-structured-output.ts` is located in the `web/src/pages/agent/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [use-add-node.ts](use-add-node.ts_docs.md)
- [use-agent-tool-initial-values.ts](use-agent-tool-initial-values.ts_docs.md)
- [use-before-delete.tsx](use-before-delete.tsx_docs.md)
- [use-build-dsl.ts](use-build-dsl.ts_docs.md)
- [use-build-options.tsx](use-build-options.tsx_docs.md)
- [use-cache-chat-log.ts](use-cache-chat-log.ts_docs.md)
- [use-calculate-sheet-right.ts](use-calculate-sheet-right.ts_docs.md)
- [use-cancel-dataflow.ts](use-cancel-dataflow.ts_docs.md)
- [use-change-node-name.ts](use-change-node-name.ts_docs.md)
- [use-chat-logic.ts](use-chat-logic.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
