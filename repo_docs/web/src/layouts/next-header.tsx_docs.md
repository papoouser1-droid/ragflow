# File Documentation: web/src/layouts/next-header.tsx

## File Metadata

- **Path**: `web/src/layouts/next-header.tsx`
- **Extension**: `.tsx`
- **Lines**: 181
- **Characters**: 5,612
- **Size**: 5,612 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IconFontFill } from '@/components/icon-font';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { useTheme } from '@/components/theme-provider';
import { Button } from '@/components/ui/button';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import { Segmented, SegmentedValue } from '@/components/ui/segmented';
import { LanguageList, LanguageMap, ThemeEnum } from '@/constants/common';
import { useChangeLanguage } from '@/hooks/logic-hooks';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { useNavigateWithFromState } from '@/hooks/route-hook';
import { useFetchUserInfo } from '@/hooks/user-setting-hooks';
import { Routes } from '@/routes';
import { camelCase } from 'lodash';
import {
  ChevronDown,
  CircleHelp,
  Cpu,
  File,
  House,
  Library,
  MessageSquareText,
  Moon,
  Search,
  Sun,
} from 'lucide-react';
import React, { useCallback, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { useLocation } from 'umi';
import { BellButton } from './bell-button';

const handleDocHelpCLick = () => {
  window.open('https://ragflow.io/docs/dev/category/guides', 'target');
};

export function Header() {
  const { t } = useTranslation();
  const { pathname } = useLocation();
  const navigate = useNavigateWithFromState();
  const { navigateToOldProfile } = useNavigatePage();

  const changeLanguage = useChangeLanguage();
  const { setTheme, theme } = useTheme();

  const {
    data: { language = 'English', avatar, nickname },
  } = useFetchUserInfo();

  const handleItemClick = (key: string) => () => {
    changeLanguage(key);
  };

  const items = LanguageList.map((x) => ({
    key: x,
    label: <span>{LanguageMap[x as keyof typeof LanguageMap]}</span>,
  }));

  const onThemeClick = React.useCallback(() => {
    setTheme(theme === ThemeEnum.Dark ? ThemeEnum.Light : ThemeEnum.Dark);
  }, [setTheme, theme]);

  const tagsData = useMemo(
    () => [
      { path: Routes.Root, name: t('header.Root'), icon: House },
      { path: Routes.Datasets, name: t('header.dataset'), icon: Library },
      { path: Routes.Chats, name: t('header.chat'), icon: MessageSquareText },
      { path: Routes.Searches, name: t('header.search'), icon: Search },
      { path: Routes.Agents, name: t('header.flow'), icon: Cpu },
      { path: Routes.Files, name: t('header.fileManager'), icon: File },
    ],
    [t],
  );

  const options = useMemo(() => {
    return tagsData.map((tag) => {
      const HeaderIcon = tag.icon;

      return {
        label:
          tag.path === Routes.Root ? (
            <HeaderIcon className="size-6"></HeaderIcon>
          ) : (
            <span>{tag.name}</span>
          ),
        value: tag.path,
      };
    });
  }, [tagsData]);

  // const currentPath = useMemo(() => {
  //   return (
  //     tagsData.find((x) => pathname.startsWith(x.path))?.path || Routes.Root
  //   );
  // }, [pathname, tagsData]);

  const handleChange = (path: SegmentedValue) => {
    navigate(path as Routes);
  };

  const handleLogoClick = useCallback(() => {
    navigate(Routes.Root);
  }, [navigate]);

  return (
    <section className="py-5 px-10 flex justify-between items-center ">
      <div className="flex items-center gap-4">
        <img
          src={'/logo.svg'}
          alt="logo"
          className="size-10 mr-[12] cursor-pointer"
          onClick={handleLogoClick}
        />
      </div>
      <Segmented
        rounded="xxxl"
        sizeType="xl"
        buttonSize="xl"
        options={options}
        value={pathname}
        onChange={handleChange}
        activeClassName="text-bg-base bg-metallic-gradient border-b-[#00BEB4] border-b-2"
      ></Segmented>
      <div className="flex items-center gap-5 text-text-badge">
        <a
          target="_blank"
          href="https://discord.com/invite/NjYzJD3GM3"
          rel="noreferrer"
        >
          <IconFontFill name="a-DiscordIconSVGVectorIcon"></IconFontFill>
        </a>
        <a
          target="_blank"
          href="https://github.com/infiniflow/ragflow"
          rel="noreferrer"
        >
          <IconFontFill name="GitHub"></IconFontFill>
        </a>
        <DropdownMenu>
          <DropdownMenuTrigger>
            <div className="flex items-center gap-1">
              {t(`common.${camelCase(language)}`)}
              <ChevronDown className="size-4" />
            </div>
          </DropdownMenuTrigger>
          <DropdownMenuContent>
            {items.map((x) => (
              <DropdownMenuItem key={x.key} onClick={handleItemClick(x.key)}>
                {x.label}
              </DropdownMenuItem>
            ))}
          </DropdownMenuContent>
        </DropdownMenu>
        <Button variant={'ghost'} onClick={handleDocHelpCLick}>
          <CircleHelp />
        </Button>
        <Button variant={'ghost'} onClick={onThemeClick}>
          {theme === 'light' ? <Sun /> : <Moon />}
        </Button>
        <BellButton></BellButton>
        <div className="relative">
          <RAGFlowAvatar
            name={nickname}
            avatar={avatar}
            isPerson
            className="size-8 cursor-pointer"
            onClick={navigateToOldProfile}
          ></RAGFlowAvatar>
          {/* Temporarily hidden */}
          {/* <Badge className="h-5 w-8 absolute font-normal p-0 justify-center -right-8 -top-2 text-bg-base bg-gradient-to-l from-[#42D7E7] to-[#478AF5]">
            Pro
          </Badge> */}
        </div>
      </div>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/layouts/next-header.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 181 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `Header`: Exported entity

### Functions (10)

- `handleDocHelpCLick()`: Function definition
- `Header()`: Function definition
- `handleItemClick()`: Function definition
- `items()`: Function definition
- `onThemeClick()`: Function definition
- `tagsData()`: Function definition
- `options()`: Function definition
- `currentPath()`: Function definition
- `handleChange()`: Function definition
- `handleLogoClick()`: Function definition

### Imports (18)

- `import { IconFontFill } from '@/components/icon-font';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { useTheme } from '@/components/theme-provider';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { Segmented, SegmentedValue } from '@/components/ui/segmented';`
- `import { LanguageList, LanguageMap, ThemeEnum } from '@/constants/common';`
- `import { useChangeLanguage } from '@/hooks/logic-hooks';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { useNavigateWithFromState } from '@/hooks/route-hook';`

## Code Structure Analysis

- Total lines: 181
- Blank lines: 15 (8.3%)
- Comment lines: ~5 (2.8%)
- Code lines: ~161


## Dependencies and Imports

- `@/components/icon-font`
- `@/components/ragflow-avatar`
- `@/components/theme-provider`
- `@/components/ui/button`
- `@/components/ui/segmented`
- `@/constants/common`
- `@/hooks/logic-hooks`
- `@/hooks/logic-hooks/navigate-hooks`
- `@/hooks/route-hook`
- `@/hooks/user-setting-hooks`
- `@/routes`
- `lodash`
- `react`
- `react-i18next`
- `umi`
- `./bell-button`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/layouts`.

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

- Other files in `web/src/layouts/` directory
- Potential test file: `test_next-header.tsx`

## Keywords

./bell-button, @/components/icon-font, @/components/ragflow-avatar, @/components/theme-provider, @/components/ui/button, @/components/ui/segmented, @/constants/common, @/hooks/logic-hooks, @/hooks/logic-hooks/navigate-hooks, @/hooks/route-hook, @/hooks/user-setting-hooks, @/routes, Agents, Badge, BellButton, Button, Chats, ChevronDown, CircleHelp, Cpu, Dark, Datasets, DiscordIconSVGVectorIcon, DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuTrigger, English, File, Files, GitHub, Header, HeaderIcon, House, IconFontFill, LanguageList, LanguageMap, Library, Light, MessageSquareText, Moon, NjYzJD3GM3, Pro, RAGFlowAvatar, React, Root, Routes, Search, Searches, Segmented...

---
*Generated by RAGFlow Repository Documentation Generator*
