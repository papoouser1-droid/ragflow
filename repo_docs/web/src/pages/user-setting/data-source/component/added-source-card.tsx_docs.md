# File Documentation: web/src/pages/user-setting/data-source/component/added-source-card.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/component/added-source-card.tsx`
- **Extension**: `.tsx`
- **Lines**: 63
- **Characters**: 2,309
- **Size**: 2,309 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { Settings, Trash2 } from 'lucide-react';
import { useDeleteDataSource } from '../hooks';
import { IDataSorceInfo, IDataSourceBase } from '../interface';
import { delSourceModal } from './delete-source-modal';

export type IAddedSourceCardProps = IDataSorceInfo & {
  list: IDataSourceBase[];
};
export const AddedSourceCard = (props: IAddedSourceCardProps) => {
  const { list, name, icon } = props;
  const { handleDelete } = useDeleteDataSource();
  const { navigateToDataSourceDetail } = useNavigatePage();
  const toDetail = (id: string) => {
    navigateToDataSourceDetail(id);
  };
  return (
    <Card className="bg-transparent border border-border-button px-5 pt-[10px] pb-5 rounded-md">
      <CardHeader className="flex flex-row items-center justify-between space-y-0 p-0 pb-3">
        {/* <Users className="mr-2 h-5 w-5 text-[#1677ff]" /> */}
        <CardTitle className="text-base items-center flex gap-1 font-normal">
          {icon}
          {name}
        </CardTitle>
      </CardHeader>
      <CardContent className="p-2 flex flex-col gap-2">
        {list.map((item) => (
          <div
            key={item.id}
            className="flex flex-row items-center justify-between rounded-md bg-bg-card px-[10px] py-4"
          >
            <div className="text-sm text-text-secondary ">{item.name}</div>
            <div className="text-sm text-text-secondary  flex gap-2">
              <Settings
                className="cursor-pointer"
                size={14}
                onClick={() => {
                  toDetail(item.id);
                }}
              />
              {/* <ConfirmDeleteDialog onOk={() => handleDelete(item)}> */}
              <Trash2
                className="cursor-pointer"
                size={14}
                onClick={() =>
                  delSourceModal({
                    data: item,
                    onOk: () => {
                      handleDelete(item);
                    },
                  })
                }
              />
              {/* </ConfirmDeleteDialog> */}
            </div>
          </div>
        ))}
      </CardContent>
    </Card>
  );
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/data-source/component/added-source-card.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 63 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `AddedSourceCard`: Exported entity

### Functions (2)

- `AddedSourceCard()`: Function definition
- `toDetail()`: Function definition

### Imports (6)

- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { Settings, Trash2 } from 'lucide-react';`
- `import { useDeleteDataSource } from '../hooks';`
- `import { IDataSorceInfo, IDataSourceBase } from '../interface';`
- `import { delSourceModal } from './delete-source-modal';`

## Code Structure Analysis

- Total lines: 63
- Blank lines: 2 (3.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~61


## Dependencies and Imports

- `@/components/ui/card`
- `@/hooks/logic-hooks/navigate-hooks`
- `lucide-react`
- `../hooks`
- `../interface`
- `./delete-source-modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/data-source/component`.

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

- Other files in `web/src/pages/user-setting/data-source/component/` directory
- Potential test file: `test_added-source-card.tsx`

## Keywords

../hooks, ../interface, ./delete-source-modal, @/components/ui/card, @/hooks/logic-hooks/navigate-hooks, AddedSourceCard, Card, CardContent, CardHeader, CardTitle, ConfirmDeleteDialog, IAddedSourceCardProps, IDataSorceInfo, IDataSourceBase, Settings, Trash2, TypeScript, Users, lucide-react, toDetail

---
*Generated by RAGFlow Repository Documentation Generator*
