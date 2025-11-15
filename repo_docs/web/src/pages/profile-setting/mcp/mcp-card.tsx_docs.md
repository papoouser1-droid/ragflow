# File Documentation: web/src/pages/profile-setting/mcp/mcp-card.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/mcp/mcp-card.tsx`
- **Extension**: `.tsx`
- **Lines**: 75
- **Characters**: 2,371
- **Size**: 2,371 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Card, CardContent } from '@/components/ui/card';
import { Checkbox } from '@/components/ui/checkbox';
import { IMcpServer } from '@/interfaces/database/mcp';
import { formatDate } from '@/utils/date';
import { isPlainObject } from 'lodash';
import { useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { McpOperation } from './mcp-operation';
import { UseBulkOperateMCPReturnType } from './use-bulk-operate-mcp';
import { UseEditMcpReturnType } from './use-edit-mcp';

export type DatasetCardProps = {
  data: IMcpServer;
  isSelectionMode: boolean;
} & Pick<UseBulkOperateMCPReturnType, 'handleSelectChange' | 'selectedList'> &
  Pick<UseEditMcpReturnType, 'showEditModal'>;

export function McpCard({
  data,
  selectedList,
  handleSelectChange,
  showEditModal,
  isSelectionMode,
}: DatasetCardProps) {
  const { t } = useTranslation();
  const toolLength = useMemo(() => {
    const tools = data.variables?.tools;
    if (isPlainObject(tools)) {
      return Object.keys(tools || {}).length;
    }
    return 0;
  }, [data.variables?.tools]);
  const onCheckedChange = (checked: boolean) => {
    if (typeof checked === 'boolean') {
      handleSelectChange(data.id, checked);
    }
  };

  return (
    <Card key={data.id}>
      <CardContent className="p-2.5 pt-2 group">
        <section className="flex justify-between pb-2">
          <h3 className="text-base font-normal truncate flex-1 text-text-primary">
            {data.name}
          </h3>
          <div className="space-x-4">
            {isSelectionMode ? (
              <Checkbox
                checked={selectedList.includes(data.id)}
                onCheckedChange={onCheckedChange}
                onClick={(e) => {
                  e.stopPropagation();
                }}
              />
            ) : (
              <McpOperation
                mcpId={data.id}
                showEditModal={showEditModal}
              ></McpOperation>
            )}
          </div>
        </section>
        <div className="flex justify-between items-end text-xs text-text-secondary">
          <div className="w-full">
            <div className="line-clamp-1 pb-1">
              {toolLength} {t('mcp.cachedTools')}
            </div>
            <p>{formatDate(data.update_date)}</p>
          </div>
        </div>
      </CardContent>
    </Card>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/profile-setting/mcp/mcp-card.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 75 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `McpCard`: Exported entity

### Functions (3)

- `McpCard()`: Function definition
- `toolLength()`: Function definition
- `onCheckedChange()`: Function definition

### Imports (10)

- `import { Card, CardContent } from '@/components/ui/card';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import { IMcpServer } from '@/interfaces/database/mcp';`
- `import { formatDate } from '@/utils/date';`
- `import { isPlainObject } from 'lodash';`
- `import { useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { McpOperation } from './mcp-operation';`
- `import { UseBulkOperateMCPReturnType } from './use-bulk-operate-mcp';`
- `import { UseEditMcpReturnType } from './use-edit-mcp';`

## Code Structure Analysis

- Total lines: 75
- Blank lines: 4 (5.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~71


## Dependencies and Imports

- `@/components/ui/card`
- `@/components/ui/checkbox`
- `@/interfaces/database/mcp`
- `@/utils/date`
- `lodash`
- `react`
- `react-i18next`
- `./mcp-operation`
- `./use-bulk-operate-mcp`
- `./use-edit-mcp`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/profile-setting/mcp`.

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

- Other files in `web/src/pages/profile-setting/mcp/` directory
- Potential test file: `test_mcp-card.tsx`

## Keywords

./mcp-operation, ./use-bulk-operate-mcp, ./use-edit-mcp, @/components/ui/card, @/components/ui/checkbox, @/interfaces/database/mcp, @/utils/date, Card, CardContent, Checkbox, DatasetCardProps, IMcpServer, McpCard, McpOperation, Object, Pick, TypeScript, UseBulkOperateMCPReturnType, UseEditMcpReturnType, lodash, onCheckedChange, react, react-i18next, toolLength, tools

---
*Generated by RAGFlow Repository Documentation Generator*
