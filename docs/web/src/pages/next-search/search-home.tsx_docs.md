# Documentation: web/src/pages/next-search/search-home.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/search-home.tsx`
- **Size**: 3431 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/next-search/search-home.tsx`.

## Original Source Code

```tsx
import { Input } from '@/components/originui/input';
import Spotlight from '@/components/spotlight';
import message from '@/components/ui/message';
import { IUserInfo } from '@/interfaces/database/user-setting';
import { cn } from '@/lib/utils';
import { Search } from 'lucide-react';
import { Dispatch, SetStateAction } from 'react';
import { useTranslation } from 'react-i18next';
import './index.less';

export default function SearchPage({
  isSearching,
  setIsSearching,
  searchText,
  setSearchText,
  userInfo,
  canSearch,
}: {
  isSearching: boolean;
  setIsSearching: Dispatch<SetStateAction<boolean>>;
  searchText: string;
  setSearchText: Dispatch<SetStateAction<string>>;
  userInfo?: IUserInfo;
  canSearch?: boolean;
}) {
  // const { data: userInfo } = useFetchUserInfo();
  const { t } = useTranslation();
  return (
    <section className="relative w-full flex transition-all justify-center items-center mt-[15vh]">
      <div className="relative z-10 px-8 pt-8 flex  text-transparent flex-col justify-center items-center w-[780px]">
        <h1
          className={cn(
            'text-4xl font-bold bg-gradient-to-l from-[#40EBE3] to-[#4A51FF] bg-clip-text',
          )}
        >
          RAGFlow
        </h1>

        <div className="rounded-lg  text-primary text-xl sticky flex justify-center w-full transform scale-100 mt-8 p-6 h-[240px] border">
          {!isSearching && <Spotlight className="z-0" />}
          <div className="flex flex-col justify-center items-center  w-2/3">
            {!isSearching && (
              <>
                <p className="mb-4 transition-opacity">👋 Hi there</p>
                <p className="mb-10 transition-opacity">
                  {userInfo && (
                    <>
                      {t('search.welcomeBack')}, {userInfo.nickname}
                    </>
                  )}
                </p>
              </>
            )}

            <div className="relative w-full ">
              <Input
                placeholder={t('search.searchGreeting')}
                className="w-full rounded-full py-7 px-4 pr-10 text-text-primary text-lg bg-bg-base delay-700"
                value={searchText}
                onKeyUp={(e) => {
                  if (e.key === 'Enter') {
                    if (canSearch === false) {
                      message.warning(t('search.chooseDataset'));
                      return;
                    }
                    setIsSearching(!isSearching);
                  }
                }}
                onChange={(e) => {
                  if (canSearch === false) {
                    message.warning(t('search.chooseDataset'));
                    return;
                  }
                  setSearchText(e.target.value || '');
                }}
              />
              <button
                type="button"
                className="absolute right-2 top-1/2 -translate-y-1/2 transform rounded-full bg-text-primary p-2 text-bg-base shadow w-12"
                onClick={() => {
                  if (canSearch === false) {
                    message.warning(t('search.chooseDataset'));
                    return;
                  }
                  setIsSearching(!isSearching);
                }}
              >
                <Search size={22} className="m-auto" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/next-search/search-home.tsx` is located in the `web/src/pages/next-search` directory.

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
- [index.tsx](index.tsx_docs.md)
- [mindmap-drawer.tsx](mindmap-drawer.tsx_docs.md)
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
