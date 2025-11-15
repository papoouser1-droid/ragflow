# File Documentation: web/src/layouts/components/right-toolbar/index.tsx

## File Metadata

- **Path**: `web/src/layouts/components/right-toolbar/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 108
- **Characters**: 3,316
- **Size**: 3,316 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { DownOutlined, GithubOutlined } from '@ant-design/icons';
import { Dropdown, MenuProps, Space } from 'antd';
import camelCase from 'lodash/camelCase';
import React, { useCallback, useMemo } from 'react';
import User from '../user';

import { useTheme } from '@/components/theme-provider';
import { LanguageList, LanguageMap, ThemeEnum } from '@/constants/common';
import { useChangeLanguage } from '@/hooks/logic-hooks';
import { useFetchUserInfo, useListTenant } from '@/hooks/user-setting-hooks';
import { TenantRole } from '@/pages/user-setting/constants';
import { BellRing, CircleHelp, MoonIcon, SunIcon } from 'lucide-react';
import { useNavigate } from 'umi';
import styled from './index.less';

const Circle = ({ children, ...restProps }: React.PropsWithChildren) => {
  return (
    <div {...restProps} className={styled.circle}>
      {children}
    </div>
  );
};

const handleGithubCLick = () => {
  window.open('https://github.com/infiniflow/ragflow', 'target');
};

const handleDocHelpCLick = () => {
  window.open('https://ragflow.io/docs/dev/category/guides', 'target');
};

const RightToolBar = () => {
  const { t } = useTranslate('common');
  const changeLanguage = useChangeLanguage();
  const { setTheme, theme } = useTheme();
  const navigate = useNavigate();

  const {
    data: { language = 'English' },
  } = useFetchUserInfo();

  const handleItemClick: MenuProps['onClick'] = ({ key }) => {
    changeLanguage(key);
  };

  const { data } = useListTenant();

  const showBell = useMemo(() => {
    return data.some((x) => x.role === TenantRole.Invite);
  }, [data]);

  const items: MenuProps['items'] = LanguageList.map((x) => ({
    key: x,
    label: <span>{LanguageMap[x as keyof typeof LanguageMap]}</span>,
  })).reduce<MenuProps['items']>((pre, cur) => {
    return [...pre!, { type: 'divider' }, cur];
  }, []);

  const onMoonClick = React.useCallback(() => {
    setTheme(ThemeEnum.Light);
  }, [setTheme]);
  const onSunClick = React.useCallback(() => {
    setTheme(ThemeEnum.Dark);
  }, [setTheme]);

  const handleBellClick = useCallback(() => {
    navigate('/user-setting/team');
  }, [navigate]);

  return (
    <div className={styled.toolbarWrapper}>
      <Space wrap size={16}>
        <Dropdown menu={{ items, onClick: handleItemClick }} placement="bottom">
          <Space className={styled.language}>
            <b>{t(camelCase(language))}</b>
            <DownOutlined />
          </Space>
        </Dropdown>
        <Circle>
          <GithubOutlined onClick={handleGithubCLick} />
        </Circle>
        <Circle>
          <CircleHelp className="size-4" onClick={handleDocHelpCLick} />
        </Circle>
        <Circle>
          {theme === 'dark' ? (
            <MoonIcon onClick={onMoonClick} size={20} />
          ) : (
            <SunIcon onClick={onSunClick} size={20} />
          )}
        </Circle>
        {showBell && (
          <Circle>
            <div className="relative" onClick={handleBellClick}>
              <BellRing className="size-4 " />
              <span className="absolute size-1 rounded -right-1 -top-1 bg-red-600"></span>
            </div>
          </Circle>
        )}
        <User></User>
      </Space>
    </div>
  );
};

export default RightToolBar;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/layouts/components/right-toolbar/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 108 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (8)

- `Circle()`: Function definition
- `handleGithubCLick()`: Function definition
- `handleDocHelpCLick()`: Function definition
- `RightToolBar()`: Function definition
- `showBell()`: Function definition
- `onMoonClick()`: Function definition
- `onSunClick()`: Function definition
- `handleBellClick()`: Function definition

### Imports (14)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { DownOutlined, GithubOutlined } from '@ant-design/icons';`
- `import { Dropdown, MenuProps, Space } from 'antd';`
- `import camelCase from 'lodash/camelCase';`
- `import React, { useCallback, useMemo } from 'react';`
- `import User from '../user';`
- `import { useTheme } from '@/components/theme-provider';`
- `import { LanguageList, LanguageMap, ThemeEnum } from '@/constants/common';`
- `import { useChangeLanguage } from '@/hooks/logic-hooks';`
- `import { useFetchUserInfo, useListTenant } from '@/hooks/user-setting-hooks';`

## Code Structure Analysis

- Total lines: 108
- Blank lines: 15 (13.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~93


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@ant-design/icons`
- `antd`
- `lodash/camelCase`
- `react`
- `../user`
- `@/components/theme-provider`
- `@/constants/common`
- `@/hooks/logic-hooks`
- `@/hooks/user-setting-hooks`
- `@/pages/user-setting/constants`
- `lucide-react`
- `umi`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/layouts/components/right-toolbar`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/layouts/components/right-toolbar/` directory
- Potential test file: `test_index.tsx`

## Keywords

../user, ./index.less, @/components/theme-provider, @/constants/common, @/hooks/common-hooks, @/hooks/logic-hooks, @/hooks/user-setting-hooks, @/pages/user-setting/constants, @ant-design/icons, BellRing, Circle, CircleHelp, Dark, DownOutlined, Dropdown, English, GithubOutlined, Invite, LanguageList, LanguageMap, Light, MenuProps, MoonIcon, PropsWithChildren, React, RightToolBar, Space, SunIcon, TenantRole, ThemeEnum, TypeScript, User, ant, antd, changeLanguage, handleBellClick, handleDocHelpCLick, handleGithubCLick, handleItemClick, items, lodash/camelCase, lucide-react, navigate, onMoonClick, onSunClick, react, showBell, umi

---
*Generated by RAGFlow Repository Documentation Generator*
