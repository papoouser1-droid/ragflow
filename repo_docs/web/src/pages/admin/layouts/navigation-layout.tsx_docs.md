# File Documentation: web/src/pages/admin/layouts/navigation-layout.tsx

## File Metadata

- **Path**: `web/src/pages/admin/layouts/navigation-layout.tsx`
- **Extension**: `.tsx`
- **Lines**: 143
- **Characters**: 4,183
- **Size**: 4,183 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { NavLink, Outlet, useNavigate } from 'umi';

import { useMutation, useQuery } from '@tanstack/react-query';

import {
  LucideMonitor,
  LucideServerCrash,
  LucideSquareUserRound,
  LucideUserCog,
  LucideUserStar,
} from 'lucide-react';

import { Button } from '@/components/ui/button';
import { cn } from '@/lib/utils';
import { Routes } from '@/routes';
import { getSystemVersion, logout } from '@/services/admin-service';

import authorizationUtil from '@/utils/authorization-util';

import ThemeSwitch from '../components/theme-switch';
import { IS_ENTERPRISE } from '../utils';

const AdminNavigationLayout = () => {
  const { t } = useTranslation();
  const navigate = useNavigate();

  const { data: version } = useQuery({
    queryKey: ['admin/version'],
    queryFn: async () => (await getSystemVersion())?.data?.data?.version,
  });

  const navItems = useMemo(
    () => [
      {
        path: Routes.AdminServices,
        name: t('admin.serviceStatus'),
        icon: <LucideServerCrash className="size-[1em]" />,
      },
      {
        path: Routes.AdminUserManagement,
        name: t('admin.userManagement'),
        icon: <LucideUserCog className="size-[1em]" />,
      },
      ...(IS_ENTERPRISE
        ? [
            {
              path: Routes.AdminWhitelist,
              name: t('admin.registrationWhitelist'),
              icon: <LucideUserStar className="size-[1em]" />,
            },
            {
              path: Routes.AdminRoles,
              name: t('admin.roles'),
              icon: <LucideSquareUserRound className="size-[1em]" />,
            },
            {
              path: Routes.AdminMonitoring,
              name: t('admin.monitoring'),
              icon: <LucideMonitor className="size-[1em]" />,
            },
          ]
        : []),
    ],
    [t],
  );

  const logoutMutation = useMutation({
    mutationKey: ['adminLogout'],
    mutationFn: async () => {
      await logout();
      authorizationUtil.removeAll();
      navigate(Routes.Admin);
    },
    retry: false,
  });

  return (
    <main className="w-screen h-screen flex flex-row px-6 pt-12 pb-6 dark:*:focus-visible:ring-white">
      <aside className="w-72 mr-6 flex flex-col gap-6">
        <div className="flex items-center mb-6">
          <img className="size-8 mr-5" src="/logo.svg" alt="logo" />
          <span className="text-xl font-bold">{t('admin.title')}</span>
        </div>

        <nav>
          <ul className="space-y-4">
            {navItems.map((it) => (
              <li key={it.path}>
                <NavLink
                  to={it.path}
                  className={({ isActive }) =>
                    cn(
                      'px-4 py-3 rounded-lg',
                      'text-base w-full flex items-center justify-start text-text-secondary',
                      'hover:bg-bg-card focus:bg-bg-card focus-visible:bg-bg-card',
                      'hover:text-text-primary focus:text-text-primary focus-visible:text-text-primary',
                      'active:text-text-primary',
                      'transition-colors',
                      {
                        'bg-bg-card text-text-primary': isActive,
                      },
                    )
                  }
                >
                  {it.icon}
                  <span className="ml-3">{it.name}</span>
                </NavLink>
              </li>
            ))}
          </ul>
        </nav>

        <div className="mt-auto space-y-4">
          <div className="flex justify-between items-center">
            <span className="leading-none text-xs text-accent-primary">
              {version}
            </span>

            <ThemeSwitch />
          </div>

          <Button
            size="lg"
            variant="transparent"
            block
            onClick={() => logoutMutation.mutate()}
          >
            {t('header.logout')}
          </Button>
        </div>
      </aside>

      <section className="flex-1 h-full">
        <Outlet />
      </section>
    </main>
  );
};

export default AdminNavigationLayout;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/admin/layouts/navigation-layout.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 143 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `AdminNavigationLayout()`: Function definition
- `navItems()`: Function definition
- `logoutMutation()`: Function definition

### Imports (12)

- `import { useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { NavLink, Outlet, useNavigate } from 'umi';`
- `import { useMutation, useQuery } from '@tanstack/react-query';`
- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { cn } from '@/lib/utils';`
- `import { Routes } from '@/routes';`
- `import { getSystemVersion, logout } from '@/services/admin-service';`
- `import authorizationUtil from '@/utils/authorization-util';`

## Code Structure Analysis

- Total lines: 143
- Blank lines: 17 (11.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~126


## Dependencies and Imports

- `react`
- `react-i18next`
- `umi`
- `@tanstack/react-query`
- `@/components/ui/button`
- `@/lib/utils`
- `@/routes`
- `@/services/admin-service`
- `@/utils/authorization-util`
- `../components/theme-switch`
- `../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/admin/layouts`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/admin/layouts/` directory
- Potential test file: `test_navigation-layout.tsx`

## Keywords

../components/theme-switch, ../utils, @/components/ui/button, @/lib/utils, @/routes, @/services/admin-service, @/utils/authorization-util, @tanstack/react-query, Admin, AdminMonitoring, AdminNavigationLayout, AdminRoles, AdminServices, AdminUserManagement, AdminWhitelist, Button, IS_ENTERPRISE, LucideMonitor, LucideServerCrash, LucideSquareUserRound, LucideUserCog, LucideUserStar, NavLink, Outlet, Routes, ThemeSwitch, TypeScript, logoutMutation, navItems, navigate, react, react-i18next, tanstack, umi

---
*Generated by RAGFlow Repository Documentation Generator*
