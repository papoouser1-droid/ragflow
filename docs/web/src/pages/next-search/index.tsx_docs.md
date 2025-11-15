# Documentation: web/src/pages/next-search/index.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/index.tsx`
- **Size**: 5109 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/next-search/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/next-search/index.tsx` is located in the `web/src/pages/next-search` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to next-search.

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

- [embed-app-modal.tsx](embed-app-modal.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [mindmap-drawer.tsx](mindmap-drawer.tsx_docs.md)
- [search-home.tsx](search-home.tsx_docs.md)
- [search-setting-aisummery-config.tsx](search-setting-aisummery-config.tsx_docs.md)
- [search-setting.tsx](search-setting.tsx_docs.md)
- [search-view.tsx](search-view.tsx_docs.md)
- [searching.tsx](searching.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
