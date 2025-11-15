# Documentation: web/src/app.tsx

## File Metadata

- **Path**: `web/src/app.tsx`
- **Size**: 3759 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/app.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/app.tsx` is located in the `web/src` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to src.

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

- [conf.json](conf.json_docs.md)
- [custom.d.ts](custom.d.ts_docs.md)
- [global.less](global.less_docs.md)
- [inter.less](inter.less_docs.md)
- [routes.ts](routes.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
