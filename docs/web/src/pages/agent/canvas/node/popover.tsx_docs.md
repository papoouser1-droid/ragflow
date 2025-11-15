# Documentation: web/src/pages/agent/canvas/node/popover.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/popover.tsx`
- **Size**: 3734 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/canvas/node/popover.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/canvas/node/popover.tsx` is located in the `web/src/pages/agent/canvas/node` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to node.

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

- [agent-node.tsx](agent-node.tsx_docs.md)
- [begin-node.tsx](begin-node.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [categorize-node.tsx](categorize-node.tsx_docs.md)
- [data-operations-node.tsx](data-operations-node.tsx_docs.md)
- [dropdown.tsx](dropdown.tsx_docs.md)
- [email-node.tsx](email-node.tsx_docs.md)
- [extractor-node.tsx](extractor-node.tsx_docs.md)
- [file-node.tsx](file-node.tsx_docs.md)
- [handle-icon.tsx](handle-icon.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
