# File Documentation: web/src/pages/agents/template-sidebar.tsx

## File Metadata

- **Path**: `web/src/pages/agents/template-sidebar.tsx`
- **Extension**: `.tsx`
- **Lines**: 120
- **Characters**: 3,324
- **Size**: 3,324 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

    // section: 'All Templates',

## Detailed Walkthrough

### Exports (1)

- `SideBar`: Exported entity

### Functions (2)

- `SideBar()`: Function definition
- `handleMenuClick()`: Function definition

### Imports (5)

- `import { Button } from '@/components/ui/button';`
- `import { cn } from '@/lib/utils';`
- `import { t } from 'i18next';`
- `import { lowerFirst } from 'lodash';`
- `import {`

## Code Structure Analysis

- Total lines: 120
- Blank lines: 3 (2.5%)
- Comment lines: ~1 (0.8%)
- Code lines: ~116


## Dependencies and Imports

- `@/components/ui/button`
- `@/lib/utils`
- `i18next`
- `lodash`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agents`.

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

- Other files in `web/src/pages/agents/` directory
- Potential test file: `test_template-sidebar.tsx`

## Keywords

@/components/ui/button, @/lib/utils, Agent, All, App, Box, Button, ChartPie, Component, Consumer, ConsumerApp, Customer, CustomerSupport, Ingestion, Marketing, MenuItemKey, MessageCircleCode, Other, PencilRuler, Pipeline, Recommended, Route, SideBar, Sparkle, Support, Templates, TypeScript, active, handleMenuClick, i18next, lodash, menuItems

---
*Generated by RAGFlow Repository Documentation Generator*
