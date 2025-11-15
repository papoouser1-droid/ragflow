# Documentation: web/src/components/ui/async-tree-select.tsx

## File Metadata

- **Path**: `web/src/components/ui/async-tree-select.tsx`
- **Size**: 4512 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/async-tree-select.tsx`.

## Original Source Code

```tsx
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import { cn } from '@/lib/utils';
import { isEmpty } from 'lodash';
import { ChevronDown, ChevronRight, Loader2 } from 'lucide-react';
import { ReactNode, useCallback, useEffect, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { Button } from './button';

type TreeId = number | string;

export type TreeNodeType = {
  id: TreeId;
  title: ReactNode;
  parentId: TreeId;
  isLeaf?: boolean;
};

type AsyncTreeSelectProps = {
  treeData: TreeNodeType[];
  value?: TreeId;
  onChange?(value: TreeId): void;
  loadData?(node: TreeNodeType): Promise<any>;
};

export function AsyncTreeSelect({
  treeData,
  value,
  loadData,
  onChange,
}: AsyncTreeSelectProps) {
  const [open, setOpen] = useState(false);
  const { t } = useTranslation();

  const [expandedKeys, setExpandedKeys] = useState<TreeId[]>([]);
  const [loadingId, setLoadingId] = useState<TreeId>('');

  const selectedTitle = useMemo(() => {
    return treeData.find((x) => x.id === value)?.title;
  }, [treeData, value]);

  const isExpanded = useCallback(
    (id: TreeId | undefined) => {
      if (id === undefined) {
        return true;
      }
      return expandedKeys.indexOf(id) !== -1;
    },
    [expandedKeys],
  );

  const handleNodeClick = useCallback(
    (id: TreeId) => (e: React.MouseEvent<HTMLLIElement>) => {
      e.stopPropagation();
      onChange?.(id);
      setOpen(false);
    },
    [onChange],
  );

  const handleArrowClick = useCallback(
    (node: TreeNodeType) => async (e: React.MouseEvent<HTMLButtonElement>) => {
      e.stopPropagation();
      const { id } = node;
      if (isExpanded(id)) {
        setExpandedKeys((keys) => {
          return keys.filter((x) => x !== id);
        });
      } else {
        const hasChild = treeData.some((x) => x.parentId === id);
        setExpandedKeys((keys) => {
          return [...keys, id];
        });

        if (!hasChild) {
          setLoadingId(id);
          await loadData?.(node);
          setLoadingId('');
        }
      }
    },
    [isExpanded, loadData, treeData],
  );

  const renderNodes = useCallback(
    (parentId?: TreeId) => {
      const currentLevelList = parentId
        ? treeData.filter((x) => x.parentId === parentId)
        : treeData.filter((x) => treeData.every((y) => x.parentId !== y.id));

      if (currentLevelList.length === 0) return null;

      return (
        <ul className={cn('pl-2', { hidden: !isExpanded(parentId) })}>
          {currentLevelList.map((x) => (
            <li
              key={x.id}
              onClick={handleNodeClick(x.id)}
              className="cursor-pointer  "
            >
              <div
                className={cn(
                  'flex justify-between items-center hover:bg-accent py-0.5 px-1 rounded-md ',
                  { 'bg-cyan-50': value === x.id },
                )}
              >
                <span className={cn('flex-1 ')}>{x.title}</span>
                {x.isLeaf || (
                  <Button
                    variant={'ghost'}
                    className="size-7"
                    onClick={handleArrowClick(x)}
                    disabled={loadingId === x.id}
                  >
                    {loadingId === x.id ? (
                      <Loader2 className="animate-spin" />
                    ) : isExpanded(x.id) ? (
                      <ChevronDown />
                    ) : (
                      <ChevronRight />
                    )}
                  </Button>
                )}
              </div>
              {renderNodes(x.id)}
            </li>
          ))}
        </ul>
      );
    },
    [handleArrowClick, handleNodeClick, isExpanded, loadingId, treeData, value],
  );

  useEffect(() => {
    if (isEmpty(treeData)) {
      loadData?.({ id: '', parentId: '', title: '' });
    }
  }, [loadData, treeData]);

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger asChild>
        <div className="flex justify-between border px-2 py-1.5 rounded-md gap-2 items-center w-full">
          {selectedTitle || (
            <span className="text-slate-400">{t('common.pleaseSelect')}</span>
          )}
          <ChevronDown className="size-5 " />
        </div>
      </PopoverTrigger>
      <PopoverContent className="p-1 min-w-[var(--radix-popover-trigger-width)]">
        <ul>{renderNodes()}</ul>
      </PopoverContent>
    </Popover>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/async-tree-select.tsx` is located in the `web/src/components/ui` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to ui.

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

- [accordion.tsx](accordion.tsx_docs.md)
- [alert-dialog.tsx](alert-dialog.tsx_docs.md)
- [aspect-ratio.tsx](aspect-ratio.tsx_docs.md)
- [avatar.tsx](avatar.tsx_docs.md)
- [badge.tsx](badge.tsx_docs.md)
- [breadcrumb.tsx](breadcrumb.tsx_docs.md)
- [button.tsx](button.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [checkbox.tsx](checkbox.tsx_docs.md)
- [collapsible.tsx](collapsible.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
