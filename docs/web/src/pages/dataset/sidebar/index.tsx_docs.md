# Documentation: web/src/pages/dataset/sidebar/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/sidebar/index.tsx`
- **Size**: 3633 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/sidebar/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/sidebar/index.tsx` is located in the `web/src/pages/dataset/sidebar` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to sidebar.

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

- [hooks.tsx](hooks.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
