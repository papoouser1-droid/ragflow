# File Documentation: web/src/pages/dataset/sidebar/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/sidebar/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 112
- **Characters**: 3,633
- **Size**: 3,633 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IconFontFill } from '@/components/icon-font';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { Button } from '@/components/ui/button';
import { useSecondPathName } from '@/hooks/route-hook';
import {
  useFetchKnowledgeBaseConfiguration,
  useFetchKnowledgeGraph,
} from '@/hooks/use-knowledge-request';
import { cn, formatBytes } from '@/lib/utils';
import { Routes } from '@/routes';
import { formatPureDate } from '@/utils/date';
import { isEmpty } from 'lodash';
import { Banknote, FileSearch2, FolderOpen, Logs } from 'lucide-react';
import { useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { useHandleMenuClick } from './hooks';

type PropType = {
  refreshCount?: number;
};

export function SideBar({ refreshCount }: PropType) {
  const pathName = useSecondPathName();
  const { handleMenuClick } = useHandleMenuClick();
  // refreshCount: be for avatar img sync update on top left
  const { data } = useFetchKnowledgeBaseConfiguration({ refreshCount });
  const { data: routerData } = useFetchKnowledgeGraph();
  const { t } = useTranslation();

  const items = useMemo(() => {
    const list = [
      {
        icon: <FolderOpen className="size-4" />,
        label: t(`knowledgeDetails.subbarFiles`),
        key: Routes.DatasetBase,
      },
      {
        icon: <FileSearch2 className="size-4" />,
        label: t(`knowledgeDetails.testing`),
        key: Routes.DatasetTesting,
      },
      {
        icon: <Logs className="size-4" />,
        label: t(`knowledgeDetails.overview`),
        key: Routes.DataSetOverview,
      },
      {
        icon: <Banknote className="size-4" />,
        label: t(`knowledgeDetails.configuration`),
        key: Routes.DataSetSetting,
      },
    ];
    if (!isEmpty(routerData?.graph)) {
      list.push({
        icon: <IconFontFill name="knowledgegraph" className="size-4" />,
        label: t(`knowledgeDetails.knowledgeGraph`),
        key: Routes.KnowledgeGraph,
      });
    }
    return list;
  }, [t, routerData]);

  return (
    <aside className="relative p-5 space-y-8">
      <div className="flex gap-2.5 max-w-[200px] items-center">
        <RAGFlowAvatar
          avatar={data.avatar}
          name={data.name}
          className="size-16"
        ></RAGFlowAvatar>
        <div className=" text-text-secondary text-xs space-y-1 overflow-hidden">
          <h3 className="text-lg font-semibold line-clamp-1 text-text-primary text-ellipsis overflow-hidden">
            {data.name}
          </h3>
          <div className="flex justify-between">
            <span>
              {data.doc_num} {t('knowledgeDetails.files')}
            </span>
            <span>{formatBytes(data.size)}</span>
          </div>
          <div>
            {t('knowledgeDetails.created')} {formatPureDate(data.create_time)}
          </div>
        </div>
      </div>

      <div className="w-[200px] flex flex-col gap-5">
        {items.map((item, itemIdx) => {
          const active = '/' + pathName === item.key;
          return (
            <Button
              key={itemIdx}
              variant={active ? 'secondary' : 'ghost'}
              className={cn(
                'w-full justify-start gap-2.5 px-3 relative h-10 text-text-sub-title-invert',
                {
                  'bg-bg-card': active,
                  'text-text-primary': active,
                },
              )}
              onClick={handleMenuClick(item.key)}
            >
              {item.icon}
              <span>{item.label}</span>
            </Button>
          );
        })}
      </div>
    </aside>
  );
}

```

## High-Level Overview

  // refreshCount: be for avatar img sync update on top left

## Detailed Walkthrough

### Exports (1)

- `SideBar`: Exported entity

### Functions (2)

- `SideBar()`: Function definition
- `items()`: Function definition

### Imports (13)

- `import { IconFontFill } from '@/components/icon-font';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { Button } from '@/components/ui/button';`
- `import { useSecondPathName } from '@/hooks/route-hook';`
- `import {`
- `import { cn, formatBytes } from '@/lib/utils';`
- `import { Routes } from '@/routes';`
- `import { formatPureDate } from '@/utils/date';`
- `import { isEmpty } from 'lodash';`
- `import { Banknote, FileSearch2, FolderOpen, Logs } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 112
- Blank lines: 6 (5.4%)
- Comment lines: ~1 (0.9%)
- Code lines: ~105


## Dependencies and Imports

- `@/components/icon-font`
- `@/components/ragflow-avatar`
- `@/components/ui/button`
- `@/hooks/route-hook`
- `@/lib/utils`
- `@/routes`
- `@/utils/date`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`
- `./hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/sidebar`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/sidebar/` directory
- Potential test file: `test_index.tsx`

## Keywords

./hooks, @/components/icon-font, @/components/ragflow-avatar, @/components/ui/button, @/hooks/route-hook, @/lib/utils, @/routes, @/utils/date, Banknote, Button, DataSetOverview, DataSetSetting, DatasetBase, DatasetTesting, FileSearch2, FolderOpen, IconFontFill, KnowledgeGraph, Logs, PropType, RAGFlowAvatar, Routes, SideBar, TypeScript, active, items, list, lodash, lucide-react, pathName, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
