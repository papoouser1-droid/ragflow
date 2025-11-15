# File Documentation: web/src/pages/user-setting/sidebar/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/sidebar/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 115
- **Characters**: 4,374
- **Size**: 4,374 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IconFontFill } from '@/components/icon-font';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import ThemeToggle from '@/components/theme-toggle';
import { Button } from '@/components/ui/button';
import { Domain } from '@/constants/common';
import { useLogout } from '@/hooks/login-hooks';
import { useSecondPathName } from '@/hooks/route-hook';
import {
  useFetchSystemVersion,
  useFetchUserInfo,
} from '@/hooks/use-user-setting-request';
import { cn } from '@/lib/utils';
import { Routes } from '@/routes';
import { t } from 'i18next';
import { Banknote, Box, Server, Unplug, User, Users } from 'lucide-react';
import { useEffect } from 'react';
import { useHandleMenuClick } from './hooks';

const menuItems = [
  { icon: Server, label: t('setting.dataSources'), key: Routes.DataSource },
  { icon: Box, label: t('setting.model'), key: Routes.Model },
  { icon: Banknote, label: 'MCP', key: Routes.Mcp },
  { icon: Users, label: t('setting.team'), key: Routes.Team },
  { icon: User, label: t('setting.profile'), key: Routes.Profile },
  { icon: Unplug, label: t('setting.api'), key: Routes.Api },
  // {
  //   icon: MessageSquareQuote,
  //   label: 'Prompt Templates',
  //   key: Routes.Profile,
  // },
  // { icon: TextSearch, label: 'Retrieval Templates', key: Routes.Profile },
  // { icon: Cog, label: t('setting.system'), key: Routes.System },
  // { icon: Banknote, label: 'Plan', key: Routes.Plan },
];

export function SideBar() {
  const pathName = useSecondPathName();
  const { data: userInfo } = useFetchUserInfo();
  const { handleMenuClick, active } = useHandleMenuClick();
  const { version, fetchSystemVersion } = useFetchSystemVersion();
  useEffect(() => {
    if (location.host !== Domain) {
      fetchSystemVersion();
    }
  }, [fetchSystemVersion]);
  const { logout } = useLogout();

  return (
    <aside className="w-[303px] bg-bg-base flex flex-col">
      <div className="px-6 flex gap-2 items-center">
        <RAGFlowAvatar
          avatar={userInfo?.avatar}
          name={userInfo?.nickname}
          isPerson
        />
        <p className="text-sm text-text-primary">{userInfo?.email}</p>
      </div>
      <div className="flex-1 overflow-auto">
        {menuItems.map((item, idx) => {
          const hoverKey = pathName === item.key;
          return (
            <div key={idx}>
              <div key={idx} className="mx-6 my-5 ">
                <Button
                  variant={hoverKey ? 'secondary' : 'ghost'}
                  className={cn('w-full justify-between gap-2.5 p-3 relative', {
                    'bg-bg-card text-text-primary': active === item.key,
                    'bg-bg-base text-text-secondary': active !== item.key,
                  })}
                  onClick={handleMenuClick(item.key)}
                >
                  <section className="flex items-center gap-2.5">
                    {item.key === Routes.Mcp ? (
                      <IconFontFill name={'mcp'} className="size-4 w-4 h-4" />
                    ) : (
                      <item.icon className="w-6 h-6" />
                    )}
                    <span>{item.label}</span>
                  </section>
                  {/* {item.key === Routes.System && (
                    <div className="mr-2 px-2 bg-accent-primary-5 text-accent-primary rounded-md">
                      {version}
                    </div>
                  )} */}
                  {/* {active && (
                    <div className="absolute right-0 w-[5px] h-[66px] bg-primary rounded-l-xl shadow-[0_0_5.94px_#7561ff,0_0_11.88px_#7561ff,0_0_41.58px_#7561ff,0_0_83.16px_#7561ff,0_0_142.56px_#7561ff,0_0_249.48px_#7561ff]" />
                  )} */}
                </Button>
              </div>
            </div>
          );
        })}
      </div>

      <div className="p-6 mt-auto ">
        <div className="flex items-center gap-2 mb-6 justify-between">
          <div className="mr-2 px-2 text-accent-primary rounded-md">
            {version}
          </div>
          <ThemeToggle />
        </div>
        <Button
          variant="outline"
          className="w-full gap-3 !bg-bg-base border !border-border-button !text-text-secondary"
          onClick={() => {
            logout();
          }}
        >
          {t('setting.logout')}
        </Button>
      </div>
    </aside>
  );
}

```

## High-Level Overview

  // {
  //   icon: MessageSquareQuote,
  //   label: 'Prompt Templates',
  //   key: Routes.Profile,
  // },
  // { icon: TextSearch, label: 'Retrieval Templates', key: Routes.Profile },
  // { icon: Cog, label: t('setting.system'), key: Routes.System },
  // { icon: Banknote, label: 'Plan', key: Routes.Plan },

## Detailed Walkthrough

### Exports (1)

- `SideBar`: Exported entity

### Functions (1)

- `SideBar()`: Function definition

### Imports (14)

- `import { IconFontFill } from '@/components/icon-font';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import ThemeToggle from '@/components/theme-toggle';`
- `import { Button } from '@/components/ui/button';`
- `import { Domain } from '@/constants/common';`
- `import { useLogout } from '@/hooks/login-hooks';`
- `import { useSecondPathName } from '@/hooks/route-hook';`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { Routes } from '@/routes';`

## Code Structure Analysis

- Total lines: 115
- Blank lines: 5 (4.3%)
- Comment lines: ~8 (7.0%)
- Code lines: ~102


## Dependencies and Imports

- `@/components/icon-font`
- `@/components/ragflow-avatar`
- `@/components/theme-toggle`
- `@/components/ui/button`
- `@/constants/common`
- `@/hooks/login-hooks`
- `@/hooks/route-hook`
- `@/lib/utils`
- `@/routes`
- `i18next`
- `lucide-react`
- `react`
- `./hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/sidebar`.

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

- Other files in `web/src/pages/user-setting/sidebar/` directory
- Potential test file: `test_index.tsx`

## Keywords

./hooks, @/components/icon-font, @/components/ragflow-avatar, @/components/theme-toggle, @/components/ui/button, @/constants/common, @/hooks/login-hooks, @/hooks/route-hook, @/lib/utils, @/routes, Api, Banknote, Box, Button, Cog, DataSource, Domain, IconFontFill, MCP, Mcp, MessageSquareQuote, Model, Plan, Profile, Prompt, RAGFlowAvatar, Retrieval, Routes, Server, SideBar, System, Team, Templates, TextSearch, ThemeToggle, TypeScript, Unplug, User, Users, hoverKey, i18next, lucide-react, menuItems, pathName, react

---
*Generated by RAGFlow Repository Documentation Generator*
