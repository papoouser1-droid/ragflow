# Documentation: web/src/pages/agents/template-sidebar.tsx

## File Metadata

- **Path**: `web/src/pages/agents/template-sidebar.tsx`
- **Size**: 3324 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agents/template-sidebar.tsx`.

## Original Source Code

```tsx
import { Button } from '@/components/ui/button';
import { cn } from '@/lib/utils';
import { t } from 'i18next';
import { lowerFirst } from 'lodash';
import {
  Box,
  ChartPie,
  Component,
  MessageCircleCode,
  PencilRuler,
  Route,
  Sparkle,
} from 'lucide-react';
export enum MenuItemKey {
  Recommended = 'Recommended',
  Agent = 'Agent',
  CustomerSupport = 'Customer Support',
  Marketing = 'Marketing',
  ConsumerApp = 'Consumer App',
  Pipeline = 'Ingestion Pipeline',
  Other = 'Other',
}
const menuItems = [
  {
    // section: 'All Templates',
    section: '',
    items: [
      {
        icon: Sparkle,
        label: t('flow.' + lowerFirst(MenuItemKey.Recommended)),
        key: MenuItemKey.Recommended,
      },
      {
        icon: Box,
        label: t('flow.' + lowerFirst(MenuItemKey.Agent)),
        key: MenuItemKey.Agent,
      },
      {
        icon: MessageCircleCode,
        label: t(
          'flow.' + lowerFirst(MenuItemKey.CustomerSupport).replace(' ', ''),
        ),
        key: MenuItemKey.CustomerSupport,
      },
      {
        icon: ChartPie,
        label: t('flow.' + lowerFirst(MenuItemKey.Marketing)),
        key: MenuItemKey.Marketing,
      },
      {
        icon: Component,
        label: t(
          'flow.' + lowerFirst(MenuItemKey.ConsumerApp.replace(' ', '')),
        ),
        key: MenuItemKey.ConsumerApp,
      },
      {
        icon: Route,
        label: t('flow.' + lowerFirst(MenuItemKey.Pipeline.replace(' ', ''))),
        key: MenuItemKey.Pipeline,
      },
      {
        icon: PencilRuler,
        label: t('flow.' + lowerFirst(MenuItemKey.Other)),
        key: MenuItemKey.Other,
      },
    ],
  },
];

export function SideBar({
  change,
  selected = MenuItemKey.Recommended,
}: {
  change: (keyword: string) => void;
  selected?: string;
}) {
  const handleMenuClick = (key: string) => {
    change(key);
  };

  return (
    <aside className="w-[303px] bg-text-title-invert border-r flex flex-col">
      <div className="flex-1 overflow-auto">
        {menuItems.map((section, idx) => (
          <div key={idx}>
            {section.section && (
              <h2
                className="p-6 text-sm font-semibold hover:bg-muted/50 cursor-pointer"
                onClick={() => handleMenuClick('')}
              >
                {section.section}
              </h2>
            )}
            {section.items.map((item, itemIdx) => {
              const active = selected === item.key;
              return (
                <Button
                  key={itemIdx}
                  variant={active ? 'secondary' : 'ghost'}
                  className={cn(
                    'w-full justify-start gap-4 px-6 py-8 relative rounded-none',
                  )}
                  onClick={() => handleMenuClick(item.key)}
                >
                  <item.icon className="w-6 h-6" />
                  <span>{item.label}</span>
                  {active && (
                    <div className="absolute right-0 w-[5px] h-[66px] bg-primary rounded-l-xl shadow-[0_0_5.94px_#7561ff,0_0_11.88px_#7561ff,0_0_41.58px_#7561ff,0_0_83.16px_#7561ff,0_0_142.56px_#7561ff,0_0_249.48px_#7561ff]" />
                  )}
                </Button>
              );
            })}
          </div>
        ))}
      </div>
    </aside>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agents/template-sidebar.tsx` is located in the `web/src/pages/agents` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to agents.

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

- [agent-card.tsx](agent-card.tsx_docs.md)
- [agent-dropdown.tsx](agent-dropdown.tsx_docs.md)
- [agent-log-detail-modal.tsx](agent-log-detail-modal.tsx_docs.md)
- [agent-log-page.tsx](agent-log-page.tsx_docs.md)
- [agent-templates.tsx](agent-templates.tsx_docs.md)
- [constant.ts](constant.ts_docs.md)
- [create-agent-dialog.tsx](create-agent-dialog.tsx_docs.md)
- [create-agent-form.tsx](create-agent-form.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [name-form-field.tsx](name-form-field.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
