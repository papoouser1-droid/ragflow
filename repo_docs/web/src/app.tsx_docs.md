# File Documentation: web/src/app.tsx

## File Metadata

- **Path**: `web/src/app.tsx`
- **Extension**: `.tsx`
- **Lines**: 119
- **Characters**: 3,759
- **Size**: 3,759 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Toaster as Sonner } from '@/components/ui/sonner';
import { Toaster } from '@/components/ui/toaster';
import i18n from '@/locales/config';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { App, ConfigProvider, ConfigProviderProps, theme } from 'antd';
import pt_BR from 'antd/lib/locale/pt_BR';
import deDE from 'antd/locale/de_DE';
import enUS from 'antd/locale/en_US';
import ru_RU from 'antd/locale/ru_RU';
import vi_VN from 'antd/locale/vi_VN';
import zhCN from 'antd/locale/zh_CN';
import zh_HK from 'antd/locale/zh_HK';
import dayjs from 'dayjs';
import advancedFormat from 'dayjs/plugin/advancedFormat';
import customParseFormat from 'dayjs/plugin/customParseFormat';
import localeData from 'dayjs/plugin/localeData';
import weekOfYear from 'dayjs/plugin/weekOfYear';
import weekYear from 'dayjs/plugin/weekYear';
import weekday from 'dayjs/plugin/weekday';
import React, { ReactNode, useEffect, useState } from 'react';
import { ThemeProvider, useTheme } from './components/theme-provider';
import { SidebarProvider } from './components/ui/sidebar';
import { TooltipProvider } from './components/ui/tooltip';
import { ThemeEnum } from './constants/common';
import storage from './utils/authorization-util';

dayjs.extend(customParseFormat);
dayjs.extend(advancedFormat);
dayjs.extend(weekday);
dayjs.extend(localeData);
dayjs.extend(weekOfYear);
dayjs.extend(weekYear);

const AntLanguageMap = {
  en: enUS,
  zh: zhCN,
  'zh-TRADITIONAL': zh_HK,
  ru: ru_RU,
  vi: vi_VN,
  'pt-BR': pt_BR,
  de: deDE,
};

if (process.env.NODE_ENV === 'development') {
  const whyDidYouRender = require('@welldone-software/why-did-you-render');
  whyDidYouRender(React, {
    trackAllPureComponents: true,
    trackExtraHooks: [],
    logOnDifferentValues: true,
  });
}

const queryClient = new QueryClient();

type Locale = ConfigProviderProps['locale'];

function Root({ children }: React.PropsWithChildren) {
  const { theme: themeragflow } = useTheme();
  const getLocale = (lng: string) =>
    AntLanguageMap[lng as keyof typeof AntLanguageMap] ?? enUS;

  const [locale, setLocal] = useState<Locale>(getLocale(storage.getLanguage()));

  i18n.on('languageChanged', function (lng: string) {
    storage.setLanguage(lng);
    setLocal(getLocale(lng));
  });

  return (
    <>
      <ConfigProvider
        theme={{
          token: {
            fontFamily: 'Inter',
          },
          algorithm:
            themeragflow === 'dark'
              ? theme.darkAlgorithm
              : theme.defaultAlgorithm,
        }}
        locale={locale}
      >
        <SidebarProvider className="h-full">
          <App>{children}</App>
        </SidebarProvider>
        <Sonner position={'top-right'} expand richColors closeButton></Sonner>
        <Toaster />
      </ConfigProvider>
      {/* <ReactQueryDevtools buttonPosition={'top-left'} initialIsOpen={false} /> */}
    </>
  );
}

const RootProvider = ({ children }: React.PropsWithChildren) => {
  useEffect(() => {
    // Because the language is saved in the backend, a token is required to obtain the api. However, the login page cannot obtain the language through the getUserInfo api, so the language needs to be saved in localstorage.
    const lng = storage.getLanguage();
    if (lng) {
      i18n.changeLanguage(lng);
    }
  }, []);

  return (
    <TooltipProvider>
      <QueryClientProvider client={queryClient}>
        <ThemeProvider
          defaultTheme={ThemeEnum.Dark}
          storageKey="ragflow-ui-theme"
        >
          <Root>{children}</Root>
        </ThemeProvider>
      </QueryClientProvider>
    </TooltipProvider>
  );
};
export function rootContainer(container: ReactNode) {
  return <RootProvider>{container}</RootProvider>;
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/app.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 119 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `rootContainer`: Exported entity

### Functions (4)

- `Root()`: Function definition
- `getLocale()`: Function definition
- `RootProvider()`: Function definition
- `rootContainer()`: Function definition

### Imports (25)

- `import { Toaster as Sonner } from '@/components/ui/sonner';`
- `import { Toaster } from '@/components/ui/toaster';`
- `import i18n from '@/locales/config';`
- `import { QueryClient, QueryClientProvider } from '@tanstack/react-query';`
- `import { App, ConfigProvider, ConfigProviderProps, theme } from 'antd';`
- `import pt_BR from 'antd/lib/locale/pt_BR';`
- `import deDE from 'antd/locale/de_DE';`
- `import enUS from 'antd/locale/en_US';`
- `import ru_RU from 'antd/locale/ru_RU';`
- `import vi_VN from 'antd/locale/vi_VN';`

## Code Structure Analysis

- Total lines: 119
- Blank lines: 12 (10.1%)
- Comment lines: ~1 (0.8%)
- Code lines: ~106


## Dependencies and Imports

- `@/components/ui/sonner`
- `@/components/ui/toaster`
- `@/locales/config`
- `@tanstack/react-query`
- `antd`
- `antd/lib/locale/pt_BR`
- `antd/locale/de_DE`
- `antd/locale/en_US`
- `antd/locale/ru_RU`
- `antd/locale/vi_VN`
- `antd/locale/zh_CN`
- `antd/locale/zh_HK`
- `dayjs`
- `dayjs/plugin/advancedFormat`
- `dayjs/plugin/customParseFormat`
- `dayjs/plugin/localeData`
- `dayjs/plugin/weekOfYear`
- `dayjs/plugin/weekYear`
- `dayjs/plugin/weekday`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/` directory
- Potential test file: `test_app.tsx`

## Keywords

./components/theme-provider, ./components/ui/sidebar, ./components/ui/tooltip, ./constants/common, ./utils/authorization-util, @/components/ui/sonner, @/components/ui/toaster, @/locales/config, @tanstack/react-query, AntLanguageMap, App, Because, ConfigProvider, ConfigProviderProps, Dark, However, Inter, Locale, NODE_ENV, PropsWithChildren, QueryClient, QueryClientProvider, React, ReactNode, ReactQueryDevtools, Root, RootProvider, SidebarProvider, Sonner, TRADITIONAL, ThemeEnum, ThemeProvider, Toaster, TooltipProvider, TypeScript, antd, antd/lib/locale/pt_BR, antd/locale/de_DE, antd/locale/en_US, antd/locale/ru_RU, antd/locale/vi_VN, antd/locale/zh_CN, antd/locale/zh_HK, dayjs, dayjs/plugin/advancedFormat, dayjs/plugin/customParseFormat, dayjs/plugin/localeData, dayjs/plugin/weekOfYear, dayjs/plugin/weekYear, dayjs/plugin/weekday...

---
*Generated by RAGFlow Repository Documentation Generator*
