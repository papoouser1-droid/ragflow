# File Documentation: web/src/pages/agent/canvas/node/popover.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/popover.tsx`
- **Extension**: `.tsx`
- **Lines**: 122
- **Characters**: 3,734
- **Size**: 3,734 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import get from 'lodash/get';
import React, { MouseEventHandler, useCallback, useMemo } from 'react';
import JsonView from 'react18-json-view';
import 'react18-json-view/src/style.css';
import { useReplaceIdWithText } from '../../hooks';

import { useTheme } from '@/components/theme-provider';
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import { useTranslate } from '@/hooks/common-hooks';
import { useFetchAgent } from '@/hooks/use-agent-request';
import { useGetComponentLabelByValue } from '../../hooks/use-get-begin-query';

interface IProps extends React.PropsWithChildren {
  nodeId: string;
  name?: string;
}

export function NextNodePopover({ children, nodeId, name }: IProps) {
  const { t } = useTranslate('flow');

  const { data } = useFetchAgent();
  const { theme } = useTheme();
  const component = useMemo(() => {
    return get(data, ['dsl', 'components', nodeId], {});
  }, [nodeId, data]);

  const inputs: Array<{ component_id: string; content: string }> = get(
    component,
    ['obj', 'inputs'],
    [],
  );
  const output = get(component, ['obj', 'output'], {});
  const { replacedOutput } = useReplaceIdWithText(output);
  const stopPropagation: MouseEventHandler = useCallback((e) => {
    e.stopPropagation();
  }, []);

  const getLabel = useGetComponentLabelByValue(nodeId);

  return (
    <Popover>
      <PopoverTrigger onClick={stopPropagation} asChild>
        {children}
      </PopoverTrigger>
      <PopoverContent
        align={'start'}
        side={'right'}
        sideOffset={20}
        onClick={stopPropagation}
        className="w-[400px]"
      >
        <div className="mb-3 font-semibold text-[16px]">
          {name} {t('operationResults')}
        </div>
        <div className="flex w-full gap-4 flex-col">
          <div className="flex flex-col space-y-1.5">
            <span className="font-semibold text-[14px]">{t('input')}</span>
            <div
              style={
                theme === 'dark'
                  ? {
                      backgroundColor: 'rgba(150, 150, 150, 0.2)',
                    }
                  : {}
              }
              className={`bg-gray-100 p-1 rounded`}
            >
              <Table>
                <TableHeader>
                  <TableRow>
                    <TableHead>{t('componentId')}</TableHead>
                    <TableHead className="w-[60px]">{t('content')}</TableHead>
                  </TableRow>
                </TableHeader>
                <TableBody>
                  {inputs.map((x, idx) => (
                    <TableRow key={idx}>
                      <TableCell>{getLabel(x.component_id)}</TableCell>
                      <TableCell className="truncate">{x.content}</TableCell>
                    </TableRow>
                  ))}
                </TableBody>
              </Table>
            </div>
          </div>
          <div className="flex flex-col space-y-1.5">
            <span className="font-semibold text-[14px]">{t('output')}</span>
            <div
              style={
                theme === 'dark'
                  ? {
                      backgroundColor: 'rgba(150, 150, 150, 0.2)',
                    }
                  : {}
              }
              className="bg-gray-100 p-1 rounded"
            >
              <JsonView
                src={replacedOutput}
                displaySize={30}
                className="w-full max-h-[300px] break-words overflow-auto"
              />
            </div>
          </div>
        </div>
      </PopoverContent>
    </Popover>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/popover.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 122 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `NextNodePopover`: Exported entity

### Functions (2)

- `NextNodePopover()`: Function definition
- `component()`: Function definition

### Imports (11)

- `import get from 'lodash/get';`
- `import React, { MouseEventHandler, useCallback, useMemo } from 'react';`
- `import JsonView from 'react18-json-view';`
- `import 'react18-json-view/src/style.css';`
- `import { useReplaceIdWithText } from '../../hooks';`
- `import { useTheme } from '@/components/theme-provider';`
- `import {`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useFetchAgent } from '@/hooks/use-agent-request';`

## Code Structure Analysis

- Total lines: 122
- Blank lines: 8 (6.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~114


## Dependencies and Imports

- `lodash/get`
- `react`
- `react18-json-view`
- `../../hooks`
- `@/components/theme-provider`
- `@/hooks/common-hooks`
- `@/hooks/use-agent-request`
- `../../hooks/use-get-begin-query`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node`.

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

- Other files in `web/src/pages/agent/canvas/node/` directory
- Potential test file: `test_popover.tsx`

## Keywords

../../hooks, ../../hooks/use-get-begin-query, @/components/theme-provider, @/hooks/common-hooks, @/hooks/use-agent-request, Array, IProps, JsonView, MouseEventHandler, NextNodePopover, Popover, PopoverContent, PopoverTrigger, PropsWithChildren, React, Table, TableBody, TableCell, TableHead, TableHeader, TableRow, TypeScript, component, getLabel, inputs, lodash/get, output, react, react18-json-view, stopPropagation

---
*Generated by RAGFlow Repository Documentation Generator*
