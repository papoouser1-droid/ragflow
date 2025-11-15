# File Documentation: web/src/pages/next-search/index.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 163
- **Characters**: 5,109
- **Size**: 5,109 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useFetchTokenListBeforeOtherStep } from '@/components/embed-dialog/use-show-embed-dialog';
import { PageHeader } from '@/components/page-header';
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/components/ui/breadcrumb';
import { Button } from '@/components/ui/button';
import { SharedFrom } from '@/constants/chat';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import {
  useFetchTenantInfo,
  useFetchUserInfo,
} from '@/hooks/user-setting-hooks';
import { Send, Settings } from 'lucide-react';
import { useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';
import {
  ISearchAppDetailProps,
  useFetchSearchDetail,
} from '../next-searches/hooks';
import EmbedAppModal from './embed-app-modal';
import { useCheckSettings } from './hooks';
import './index.less';
import SearchHome from './search-home';
import { SearchSetting } from './search-setting';
import SearchingPage from './searching';

export default function SearchPage() {
  const { navigateToSearchList } = useNavigatePage();
  const [isSearching, setIsSearching] = useState(false);
  const { data: SearchData } = useFetchSearchDetail();
  const { beta, handleOperate } = useFetchTokenListBeforeOtherStep();

  const [openSetting, setOpenSetting] = useState(false);
  const [openEmbed, setOpenEmbed] = useState(false);
  const [searchText, setSearchText] = useState('');
  const { data: tenantInfo } = useFetchTenantInfo();
  const { data: userInfo } = useFetchUserInfo();
  const tenantId = tenantInfo.tenant_id;
  const { t } = useTranslation();
  const { openSetting: checkOpenSetting } = useCheckSettings(
    SearchData as ISearchAppDetailProps,
  );
  useEffect(() => {
    setOpenSetting(checkOpenSetting);
  }, [checkOpenSetting]);

  useEffect(() => {
    if (isSearching) {
      setOpenSetting(false);
    }
  }, [isSearching]);

  return (
    <section>
      <PageHeader>
        <Breadcrumb>
          <BreadcrumbList>
            <BreadcrumbItem>
              <BreadcrumbLink onClick={navigateToSearchList}>
                {t('header.search')}
              </BreadcrumbLink>
            </BreadcrumbItem>
            <BreadcrumbSeparator />
            <BreadcrumbItem>
              <BreadcrumbPage>{SearchData?.name}</BreadcrumbPage>
            </BreadcrumbItem>
          </BreadcrumbList>
        </Breadcrumb>
      </PageHeader>
      <div className="flex gap-3 w-full bg-bg-base">
        <div className="flex-1">
          {!isSearching && (
            <div className="animate-fade-in-down">
              <SearchHome
                setIsSearching={setIsSearching}
                isSearching={isSearching}
                searchText={searchText}
                setSearchText={setSearchText}
                userInfo={userInfo}
                canSearch={!checkOpenSetting}
              />
            </div>
          )}
          {isSearching && (
            <div className="animate-fade-in-up">
              <SearchingPage
                setIsSearching={setIsSearching}
                searchText={searchText}
                setSearchText={setSearchText}
                data={SearchData as ISearchAppDetailProps}
              />
            </div>
          )}
        </div>
        {openSetting && (
          <SearchSetting
            className="mt-20 mr-2"
            open={openSetting}
            setOpen={setOpenSetting}
            data={SearchData as ISearchAppDetailProps}
          />
        )}
        {
          <EmbedAppModal
            open={openEmbed}
            setOpen={setOpenEmbed}
            url="/next-search/share"
            token={SearchData?.id as string}
            from={SharedFrom.Search}
            tenantId={tenantId}
            beta={beta}
          />
        }
        {
          // <EmbedDialog
          //   visible={openEmbed}
          //   hideModal={setOpenEmbed}
          //   token={SearchData?.id as string}
          //   from={SharedFrom.Search}
          //   beta={beta}
          //   isAgent={false}
          // ></EmbedDialog>
        }
      </div>
      <div className="absolute right-5 top-4 ">
        <Button
          className="bg-text-primary  text-bg-base border-b-accent-primary border-b-2"
          onClick={() => {
            handleOperate().then((res) => {
              console.log(res, 'res');
              if (res) {
                setOpenEmbed(!openEmbed);
              }
            });
          }}
        >
          <Send />
          <div>{t('search.embedApp')}</div>
        </Button>
      </div>
      {!isSearching && (
        <div className="absolute left-5 bottom-12 ">
          <Button
            variant="transparent"
            className="bg-bg-card"
            onClick={() => setOpenSetting(!openSetting)}
          >
            <Settings className="text-text-secondary" />
            <div className="text-text-secondary">
              {t('search.searchSettings')}
            </div>
          </Button>
        </div>
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-search/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 163 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `SearchPage`: Exported entity

### Functions (1)

- `SearchPage()`: Function definition

### Imports (17)

- `import { useFetchTokenListBeforeOtherStep } from '@/components/embed-dialog/use-show-embed-dialog';`
- `import { PageHeader } from '@/components/page-header';`
- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { SharedFrom } from '@/constants/chat';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import {`
- `import { Send, Settings } from 'lucide-react';`
- `import { useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 163
- Blank lines: 5 (3.1%)
- Comment lines: ~8 (4.9%)
- Code lines: ~150


## Dependencies and Imports

- `@/components/embed-dialog/use-show-embed-dialog`
- `@/components/page-header`
- `@/components/ui/button`
- `@/constants/chat`
- `@/hooks/logic-hooks/navigate-hooks`
- `lucide-react`
- `react`
- `react-i18next`
- `./embed-app-modal`
- `./hooks`
- `./search-home`
- `./search-setting`
- `./searching`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-search`.

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

- Other files in `web/src/pages/next-search/` directory
- Potential test file: `test_index.tsx`

## Keywords

./embed-app-modal, ./hooks, ./search-home, ./search-setting, ./searching, @/components/embed-dialog/use-show-embed-dialog, @/components/page-header, @/components/ui/button, @/constants/chat, @/hooks/logic-hooks/navigate-hooks, Breadcrumb, BreadcrumbItem, BreadcrumbLink, BreadcrumbList, BreadcrumbPage, BreadcrumbSeparator, Button, EmbedAppModal, EmbedDialog, ISearchAppDetailProps, PageHeader, Search, SearchData, SearchHome, SearchPage, SearchSetting, SearchingPage, Send, Settings, SharedFrom, TypeScript, lucide-react, react, react-i18next, tenantId

---
*Generated by RAGFlow Repository Documentation Generator*
