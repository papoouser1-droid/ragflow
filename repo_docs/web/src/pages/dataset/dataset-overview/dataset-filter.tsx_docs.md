# File Documentation: web/src/pages/dataset/dataset-overview/dataset-filter.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-overview/dataset-filter.tsx`
- **Extension**: `.tsx`
- **Lines**: 92
- **Characters**: 2,778
- **Size**: 2,778 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FilterButton } from '@/components/list-filter-bar';
import {
  CheckboxFormMultipleProps,
  FilterPopover,
} from '@/components/list-filter-bar/filter-popover';
import { Button } from '@/components/ui/button';
import { SearchInput } from '@/components/ui/input';
import { cn } from '@/lib/utils';
import { ChangeEventHandler, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { LogTabs } from './dataset-common';

interface IProps {
  searchString?: string;
  onSearchChange?: ChangeEventHandler<HTMLInputElement>;
  active?: (typeof LogTabs)[keyof typeof LogTabs];
  setActive?: (active: (typeof LogTabs)[keyof typeof LogTabs]) => void;
}
const DatasetFilter = (
  props: IProps & Omit<CheckboxFormMultipleProps, 'setOpen'>,
) => {
  const {
    searchString,
    onSearchChange,
    value,
    onChange,
    filters,
    onOpenChange,
    active = LogTabs.FILE_LOGS,
    setActive,
    ...rest
  } = props;
  const { t } = useTranslation();
  const filterCount = useMemo(() => {
    return typeof value === 'object' && value !== null
      ? Object.values(value).reduce((pre, cur) => {
          return pre + cur.length;
        }, 0)
      : 0;
  }, [value]);
  return (
    <div className="flex items-center justify-between mb-4">
      <div className="flex space-x-2 bg-bg-card p-1 rounded-md">
        <Button
          className={cn(
            'px-4 py-2 rounded-md hover:text-text-primary hover:bg-bg-base',
            {
              'bg-bg-base text-text-primary': active === LogTabs.FILE_LOGS,
              'bg-transparent text-text-secondary ':
                active !== LogTabs.FILE_LOGS,
            },
          )}
          onClick={() => setActive?.(LogTabs.FILE_LOGS)}
        >
          {t('knowledgeDetails.fileLogs')}
        </Button>
        <Button
          className={cn(
            'px-4 py-2 rounded-md hover:text-text-primary hover:bg-bg-base',
            {
              'bg-bg-base text-text-primary': active === LogTabs.DATASET_LOGS,
              'bg-transparent text-text-secondary ':
                active !== LogTabs.DATASET_LOGS,
            },
          )}
          onClick={() => setActive?.(LogTabs.DATASET_LOGS)}
        >
          {t('knowledgeDetails.datasetLogs')}
        </Button>
      </div>
      <div className="flex items-center space-x-2">
        <FilterPopover
          value={value}
          onChange={onChange}
          filters={filters}
          onOpenChange={onOpenChange}
        >
          <FilterButton count={filterCount}></FilterButton>
        </FilterPopover>

        <SearchInput
          value={searchString}
          onChange={onSearchChange}
          className="w-32"
        ></SearchInput>
      </div>
    </div>
  );
};

export { DatasetFilter };

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-overview/dataset-filter.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 92 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `DatasetFilter()`: Function definition
- `filterCount()`: Function definition

### Imports (8)

- `import { FilterButton } from '@/components/list-filter-bar';`
- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { SearchInput } from '@/components/ui/input';`
- `import { cn } from '@/lib/utils';`
- `import { ChangeEventHandler, useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { LogTabs } from './dataset-common';`

## Code Structure Analysis

- Total lines: 92
- Blank lines: 4 (4.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~88


## Dependencies and Imports

- `@/components/list-filter-bar`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/lib/utils`
- `react`
- `react-i18next`
- `./dataset-common`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-overview`.

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

- Other files in `web/src/pages/dataset/dataset-overview/` directory
- Potential test file: `test_dataset-filter.tsx`

## Keywords

./dataset-common, @/components/list-filter-bar, @/components/ui/button, @/components/ui/input, @/lib/utils, Button, ChangeEventHandler, CheckboxFormMultipleProps, DATASET_LOGS, DatasetFilter, FILE_LOGS, FilterButton, FilterPopover, HTMLInputElement, IProps, LogTabs, Object, Omit, SearchInput, TypeScript, filterCount, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
