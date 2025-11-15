# File Documentation: web/src/pages/agent/form/invoke-form/hooks.ts

## File Metadata

- **Path**: `web/src/pages/agent/form/invoke-form/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 98
- **Characters**: 2,495
- **Size**: 2,495 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import get from 'lodash/get';
import {
  ChangeEventHandler,
  MouseEventHandler,
  useCallback,
  useMemo,
} from 'react';
import { v4 as uuid } from 'uuid';
import { IGenerateParameter, IInvokeVariable } from '../../interface';
import useGraphStore from '../../store';

export const useHandleOperateParameters = (nodeId: string) => {
  const { getNode, updateNodeForm } = useGraphStore((state) => state);
  const node = getNode(nodeId);
  const dataSource: IGenerateParameter[] = useMemo(
    () => get(node, 'data.form.variables', []) as IGenerateParameter[],
    [node],
  );

  const changeValue = useCallback(
    (row: IInvokeVariable, field: string, value: string) => {
      const newData = [...dataSource];
      const index = newData.findIndex((item) => row.id === item.id);
      const item = newData[index];
      newData.splice(index, 1, {
        ...item,
        [field]: value,
      });

      updateNodeForm(nodeId, { variables: newData });
    },
    [dataSource, nodeId, updateNodeForm],
  );

  const handleComponentIdChange = useCallback(
    (row: IInvokeVariable) => (value: string) => {
      changeValue(row, 'component_id', value);
    },
    [changeValue],
  );

  const handleValueChange = useCallback(
    (row: IInvokeVariable): ChangeEventHandler<HTMLInputElement> =>
      (e) => {
        changeValue(row, 'value', e.target.value);
      },
    [changeValue],
  );

  const handleRemove = useCallback(
    (id?: string) => () => {
      const newData = dataSource.filter((item) => item.id !== id);
      updateNodeForm(nodeId, { variables: newData });
    },
    [updateNodeForm, nodeId, dataSource],
  );

  const handleAdd: MouseEventHandler = useCallback(
    (e) => {
      e.preventDefault();
      e.stopPropagation();
      updateNodeForm(nodeId, {
        variables: [
          ...dataSource,
          {
            id: uuid(),
            key: '',
            component_id: undefined,
            value: '',
          },
        ],
      });
    },
    [dataSource, nodeId, updateNodeForm],
  );

  const handleSave = (row: IGenerateParameter) => {
    const newData = [...dataSource];
    const index = newData.findIndex((item) => row.id === item.id);
    const item = newData[index];
    newData.splice(index, 1, {
      ...item,
      ...row,
    });

    updateNodeForm(nodeId, { variables: newData });
  };

  return {
    handleAdd,
    handleRemove,
    handleComponentIdChange,
    handleValueChange,
    handleSave,
    dataSource,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/invoke-form/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 98 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `useHandleOperateParameters`: Exported entity

### Functions (9)

- `useHandleOperateParameters()`: Function definition
- `changeValue()`: Function definition
- `index()`: Function definition
- `handleComponentIdChange()`: Function definition
- `handleValueChange()`: Function definition
- `handleRemove()`: Function definition
- `newData()`: Function definition
- `handleSave()`: Function definition
- `index()`: Function definition

### Imports (5)

- `import get from 'lodash/get';`
- `import {`
- `import { v4 as uuid } from 'uuid';`
- `import { IGenerateParameter, IInvokeVariable } from '../../interface';`
- `import useGraphStore from '../../store';`

## Code Structure Analysis

- Total lines: 98
- Blank lines: 11 (11.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~87


## Dependencies and Imports

- `lodash/get`
- `uuid`
- `../../interface`
- `../../store`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/invoke-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/invoke-form/` directory
- Potential test file: `test_hooks.ts`

## Keywords

../../interface, ../../store, ChangeEventHandler, HTMLInputElement, IGenerateParameter, IInvokeVariable, MouseEventHandler, TypeScript, changeValue, dataSource, handleAdd, handleComponentIdChange, handleRemove, handleSave, handleValueChange, index, item, lodash/get, newData, node, useHandleOperateParameters, uuid

---
*Generated by RAGFlow Repository Documentation Generator*
