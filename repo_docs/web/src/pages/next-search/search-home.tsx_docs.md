# File Documentation: web/src/pages/next-search/search-home.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/search-home.tsx`
- **Extension**: `.tsx`
- **Lines**: 97
- **Characters**: 3,428
- **Size**: 3,431 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

  // const { data: userInfo } = useFetchUserInfo();

## Detailed Walkthrough

### Exports (1)

- `SearchPage`: Exported entity

### Functions (1)

- `SearchPage()`: Function definition

### Imports (9)

- `import { Input } from '@/components/originui/input';`
- `import Spotlight from '@/components/spotlight';`
- `import message from '@/components/ui/message';`
- `import { IUserInfo } from '@/interfaces/database/user-setting';`
- `import { cn } from '@/lib/utils';`
- `import { Search } from 'lucide-react';`
- `import { Dispatch, SetStateAction } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import './index.less';`

## Code Structure Analysis

- Total lines: 97
- Blank lines: 4 (4.1%)
- Comment lines: ~1 (1.0%)
- Code lines: ~92


## Dependencies and Imports

- `@/components/originui/input`
- `@/components/spotlight`
- `@/components/ui/message`
- `@/interfaces/database/user-setting`
- `@/lib/utils`
- `lucide-react`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-search`.

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

- Other files in `web/src/pages/next-search/` directory
- Potential test file: `test_search-home.tsx`

## Keywords

@/components/originui/input, @/components/spotlight, @/components/ui/message, @/interfaces/database/user-setting, @/lib/utils, Dispatch, Enter, IUserInfo, Input, RAGFlow, Search, SearchPage, SetStateAction, Spotlight, TypeScript, lucide-react, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
