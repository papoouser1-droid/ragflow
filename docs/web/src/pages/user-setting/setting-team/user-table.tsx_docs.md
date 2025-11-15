# Documentation: web/src/pages/user-setting/setting-team/user-table.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-team/user-table.tsx`
- **Size**: 5840 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-team/user-table.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-team/user-table.tsx` is located in the `web/src/pages/user-setting/setting-team` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to setting-team.

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

- [add-user-modal.tsx](add-user-modal.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [tenant-table.tsx](tenant-table.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
