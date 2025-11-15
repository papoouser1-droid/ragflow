# File Documentation: web/src/pages/dataset/dataset-setting/components/added-source-card.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/added-source-card.tsx`
- **Extension**: `.tsx`
- **Lines**: 98
- **Characters**: 3,093
- **Size**: 3,093 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { cn } from '@/lib/utils';
import {
  IDataSorceInfo,
  IDataSourceBase,
} from '@/pages/user-setting/data-source/interface';
import { Check } from 'lucide-react';
import { useMemo } from 'react';

export type IAddedSourceCardProps = IDataSorceInfo & {
  filterString: string;
  list: IDataSourceBase[];
  selectedList: IDataSourceBase[];
  setSelectedList: (list: IDataSourceBase[]) => void;
};
export const AddedSourceCard = (props: IAddedSourceCardProps) => {
  const {
    list: originList,
    name,
    icon,
    filterString,
    selectedList,
    setSelectedList,
  } = props;

  const list = useMemo(() => {
    return originList.map((item) => {
      const checked = selectedList?.some((i) => i.id === item.id) || false;
      return {
        ...item,
        checked: checked,
      };
    });
  }, [originList, selectedList]);

  const filterList = useMemo(
    () => list.filter((item) => item.name.indexOf(filterString) > -1),
    [filterString, list],
  );

  // const { navigateToDataSourceDetail } = useNavigatePage();
  // const toDetail = (id: string) => {
  //   navigateToDataSourceDetail(id);
  // };

  const onCheck = (item: IDataSourceBase & { checked: boolean }) => {
    if (item.checked) {
      setSelectedList(selectedList.filter((i) => i.id !== item.id));
    } else {
      setSelectedList([...(selectedList || []), item]);
    }
  };
  return (
    <>
      {filterList.length > 0 && (
        <Card className="bg-transparent border border-border-button px-5 pt-[10px] pb-5 rounded-md">
          <CardHeader className="flex flex-row items-center justify-between space-y-0 p-0 pb-3">
            {/* <Users className="mr-2 h-5 w-5 text-[#1677ff]" /> */}
            <CardTitle className="text-base flex gap-1 font-normal">
              {icon}
              {name}
            </CardTitle>
          </CardHeader>
          <CardContent className="p-2 flex flex-col gap-2">
            {filterList.map((item) => (
              <div
                key={item.id}
                className={cn(
                  'flex flex-row items-center justify-between rounded-md bg-bg-card px-2 py-1 cursor-pointer',
                  // { hidden: item.name.indexOf(filterString) <= -1 },
                )}
                onClick={() => {
                  console.log('item--->', item);
                  // toDetail(item.id);
                  onCheck(item);
                }}
              >
                <div className="text-sm text-text-secondary ">{item.name}</div>
                <div className="text-sm text-text-secondary  flex gap-2">
                  {item.checked && (
                    <Check
                      className="cursor-pointer"
                      size={14}
                      // onClick={() => {
                      //   toDetail(item.id);
                      // }}
                    />
                  )}
                </div>
              </div>
            ))}
          </CardContent>
        </Card>
      )}
    </>
  );
};

```

## High-Level Overview

  // const { navigateToDataSourceDetail } = useNavigatePage();
  // const toDetail = (id: string) => {
  //   navigateToDataSourceDetail(id);
  // };

## Detailed Walkthrough

### Exports (1)

- `AddedSourceCard`: Exported entity

### Functions (6)

- `AddedSourceCard()`: Function definition
- `list()`: Function definition
- `checked()`: Function definition
- `filterList()`: Function definition
- `toDetail()`: Function definition
- `onCheck()`: Function definition

### Imports (5)

- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import { cn } from '@/lib/utils';`
- `import {`
- `import { Check } from 'lucide-react';`
- `import { useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 98
- Blank lines: 6 (6.1%)
- Comment lines: ~9 (9.2%)
- Code lines: ~83


## Dependencies and Imports

- `@/components/ui/card`
- `@/lib/utils`
- `lucide-react`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset-setting/components/` directory
- Potential test file: `test_added-source-card.tsx`

## Keywords

@/components/ui/card, @/lib/utils, AddedSourceCard, Card, CardContent, CardHeader, CardTitle, Check, IAddedSourceCardProps, IDataSorceInfo, IDataSourceBase, TypeScript, Users, checked, filterList, list, lucide-react, onCheck, react, toDetail

---
*Generated by RAGFlow Repository Documentation Generator*
