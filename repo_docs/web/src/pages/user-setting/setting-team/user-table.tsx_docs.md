# File Documentation: web/src/pages/user-setting/setting-team/user-table.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-team/user-table.tsx`
- **Extension**: `.tsx`
- **Lines**: 162
- **Characters**: 5,840
- **Size**: 5,840 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { Badge } from '@/components/ui/badge';
import { Button } from '@/components/ui/button';
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table';
import { useListTenantUser } from '@/hooks/user-setting-hooks';
import { formatDate } from '@/utils/date';
import { upperFirst } from 'lodash';
import { ArrowDown, ArrowUp, ArrowUpDown, Trash2 } from 'lucide-react';
import { useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { TenantRole } from '../constants';
import { useHandleDeleteUser } from './hooks';

const ColorMap: Record<string, string> = {
  [TenantRole.Normal]: 'bg-transparent text-text-primary',
  [TenantRole.Invite]: 'bg-accent-primary-5 bg-accent-primary rounded-sm',
  [TenantRole.Owner]: 'bg-red-100 text-red-800',
};

const UserTable = ({ searchUser }: { searchUser: string }) => {
  const { data, loading } = useListTenantUser();
  const { handleDeleteTenantUser } = useHandleDeleteUser();
  const [sortOrder, setSortOrder] = useState<'asc' | 'desc' | null>(null);
  const { t } = useTranslation();
  const sortedData = useMemo(() => {
    console.log('sortedData', data, searchUser);
    if (!data || data.length === 0) return data;
    let filtered = data;
    if (searchUser) {
      filtered = filtered.filter(
        (tenant) =>
          tenant.nickname.toLowerCase().includes(searchUser.toLowerCase()) ||
          tenant.email.toLowerCase().includes(searchUser.toLowerCase()),
      );
    }
    if (sortOrder) {
      filtered = [...filtered].sort((a, b) => {
        const dateA = new Date(a.update_date).getTime();
        const dateB = new Date(b.update_date).getTime();

        if (sortOrder === 'asc') {
          return dateA - dateB;
        } else {
          return dateB - dateA;
        }
      });
    }

    return filtered;
  }, [data, sortOrder, searchUser]);
  const toggleSortOrder = () => {
    if (sortOrder === 'asc') {
      setSortOrder('desc');
    } else if (sortOrder === 'desc') {
      setSortOrder(null);
    } else {
      setSortOrder('asc');
    }
  };

  const renderSortIcon = () => {
    if (sortOrder === 'asc') {
      return <ArrowUp className="ml-1 h-4 w-4 " />;
    } else if (sortOrder === 'desc') {
      return <ArrowDown className="ml-1 h-4 w-4" />;
    } else {
      return <ArrowUpDown className="ml-1 h-4 w-4" />;
    }
  };
  return (
    <div className="rounded-lg bg-bg-input scrollbar-auto overflow-hidden border border-border-default">
      <Table rootClassName="rounded-lg">
        <TableHeader className="bg-bg-title">
          <TableRow>
            <TableHead className="h-12 px-4">{t('common.name')}</TableHead>
            <TableHead
              className="h-12 px-4 cursor-pointer"
              onClick={toggleSortOrder}
            >
              <div className="flex items-center">
                {t('setting.updateDate')}
                {renderSortIcon()}
              </div>
            </TableHead>
            <TableHead className="h-12 px-4">{t('setting.email')}</TableHead>
            <TableHead className="h-12 px-4">{t('setting.role')}</TableHead>
            <TableHead className="h-12 px-4">{t('common.action')}</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody className="bg-bg-base">
          {loading ? (
            <TableRow>
              <TableCell colSpan={5} className="h-24 text-center">
                <div className="flex items-center justify-center">
                  <div className="h-4 w-4 animate-spin rounded-full border-2 border-solid border-current border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]"></div>
                </div>
              </TableCell>
            </TableRow>
          ) : sortedData && sortedData.length > 0 ? (
            sortedData.map((record) => (
              <TableRow key={record.user_id} className="hover:bg-bg-card">
                <TableCell className="p-4 ">
                  <div className="flex gap-1 items-center">
                    <RAGFlowAvatar
                      isPerson
                      className="size-4"
                      avatar={record.avatar}
                      name={record.nickname}
                    />
                    {record.nickname}
                  </div>
                </TableCell>
                <TableCell className="p-4">
                  {formatDate(record.update_date)}
                </TableCell>
                <TableCell className="p-4">{record.email}</TableCell>
                <TableCell className="p-4">
                  {record.role === TenantRole.Normal && (
                    <Badge className={ColorMap[record.role]}>
                      {upperFirst('Member')}
                    </Badge>
                  )}
                  {record.role !== TenantRole.Normal && (
                    <Badge className={ColorMap[record.role]}>
                      {upperFirst(record.role)}
                    </Badge>
                  )}
                </TableCell>
                <TableCell className="p-4">
                  <Button
                    variant="ghost"
                    size="icon"
                    className="h-8 w-8 p-0"
                    onClick={handleDeleteTenantUser(record.user_id)}
                  >
                    <Trash2 className="h-4 w-4" />
                  </Button>
                </TableCell>
              </TableRow>
            ))
          ) : (
            <TableRow>
              <TableCell colSpan={5} className="h-24 text-center">
                {t('common.noData')}
              </TableCell>
            </TableRow>
          )}
        </TableBody>
      </Table>
    </div>
  );
};

export default UserTable;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-team/user-table.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 162 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `UserTable()`: Function definition
- `sortedData()`: Function definition
- `toggleSortOrder()`: Function definition
- `renderSortIcon()`: Function definition

### Imports (12)

- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { Badge } from '@/components/ui/badge';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { useListTenantUser } from '@/hooks/user-setting-hooks';`
- `import { formatDate } from '@/utils/date';`
- `import { upperFirst } from 'lodash';`
- `import { ArrowDown, ArrowUp, ArrowUpDown, Trash2 } from 'lucide-react';`
- `import { useMemo, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 162
- Blank lines: 7 (4.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~155


## Dependencies and Imports

- `@/components/ragflow-avatar`
- `@/components/ui/badge`
- `@/components/ui/button`
- `@/hooks/user-setting-hooks`
- `@/utils/date`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`
- `../constants`
- `./hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-team`.

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

- Other files in `web/src/pages/user-setting/setting-team/` directory
- Potential test file: `test_user-table.tsx`

## Keywords

../constants, ./hooks, @/components/ragflow-avatar, @/components/ui/badge, @/components/ui/button, @/hooks/user-setting-hooks, @/utils/date, ArrowDown, ArrowUp, ArrowUpDown, Badge, Button, ColorMap, Date, Invite, Member, Normal, Owner, RAGFlowAvatar, Record, Table, TableBody, TableCell, TableHead, TableHeader, TableRow, TenantRole, Trash2, TypeScript, UserTable, dateA, dateB, filtered, lodash, lucide-react, react, react-i18next, renderSortIcon, sortedData, toggleSortOrder

---
*Generated by RAGFlow Repository Documentation Generator*
