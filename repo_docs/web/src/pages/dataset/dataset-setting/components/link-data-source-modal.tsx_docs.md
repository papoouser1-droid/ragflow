# File Documentation: web/src/pages/dataset/dataset-setting/components/link-data-source-modal.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/link-data-source-modal.tsx`
- **Extension**: `.tsx`
- **Lines**: 87
- **Characters**: 2,527
- **Size**: 2,527 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import { SearchInput } from '@/components/ui/input';
import { Modal } from '@/components/ui/modal/modal';
import { IConnector } from '@/interfaces/database/knowledge';
import { useListDataSource } from '@/pages/user-setting/data-source/hooks';
import { IDataSourceBase } from '@/pages/user-setting/data-source/interface';
import { t } from 'i18next';
import { useEffect, useState } from 'react';
import { AddedSourceCard } from './added-source-card';

const LinkDataSourceModal = ({
  selectedList,
  open,
  setOpen,
  onSubmit,
}: {
  selectedList: IConnector[];
  open: boolean;
  setOpen: (open: boolean) => void;
  onSubmit?: (list: IDataSourceBase[] | undefined) => void;
}) => {
  const [list, setList] = useState<IDataSourceBase[]>();
  const [fileterString, setFileterString] = useState('');

  useEffect(() => {
    setList(selectedList);
  }, [selectedList]);

  const { categorizedList } = useListDataSource();
  const handleFormSubmit = (values: any) => {
    console.log(values, selectedList);
    onSubmit?.(list);
  };
  return (
    <Modal
      className="!w-[560px]"
      title={t('knowledgeConfiguration.linkDataSource')}
      open={open}
      onCancel={() => {
        setList(selectedList);
      }}
      onOpenChange={setOpen}
      showfooter={false}
    >
      <div className="flex flex-col gap-4 ">
        {/* {JSON.stringify(selectedList)} */}
        <SearchInput
          value={fileterString}
          onChange={(e) => setFileterString(e.target.value)}
        />
        <div className="flex flex-col gap-3">
          {categorizedList.map((item, index) => (
            <AddedSourceCard
              key={index}
              selectedList={list as IDataSourceBase[]}
              setSelectedList={(list) => setList(list)}
              filterString={fileterString}
              {...item}
            />
          ))}
        </div>
        <div className="flex justify-end gap-1">
          <Button
            type="button"
            variant={'outline'}
            className="btn-primary"
            onClick={() => {
              setOpen(false);
            }}
          >
            {t('modal.cancelText')}
          </Button>
          <Button
            type="button"
            variant={'default'}
            className="btn-primary"
            onClick={handleFormSubmit}
          >
            {t('modal.okText')}
          </Button>
        </div>
      </div>
    </Modal>
  );
};
export default LinkDataSourceModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-setting/components/link-data-source-modal.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 87 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `LinkDataSourceModal()`: Function definition
- `handleFormSubmit()`: Function definition

### Imports (9)

- `import { Button } from '@/components/ui/button';`
- `import { SearchInput } from '@/components/ui/input';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { IConnector } from '@/interfaces/database/knowledge';`
- `import { useListDataSource } from '@/pages/user-setting/data-source/hooks';`
- `import { IDataSourceBase } from '@/pages/user-setting/data-source/interface';`
- `import { t } from 'i18next';`
- `import { useEffect, useState } from 'react';`
- `import { AddedSourceCard } from './added-source-card';`

## Code Structure Analysis

- Total lines: 87
- Blank lines: 4 (4.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~83


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/input`
- `@/components/ui/modal/modal`
- `@/interfaces/database/knowledge`
- `@/pages/user-setting/data-source/hooks`
- `@/pages/user-setting/data-source/interface`
- `i18next`
- `react`
- `./added-source-card`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting/components`.

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

- Other files in `web/src/pages/dataset/dataset-setting/components/` directory
- Potential test file: `test_link-data-source-modal.tsx`

## Keywords

./added-source-card, @/components/ui/button, @/components/ui/input, @/components/ui/modal/modal, @/interfaces/database/knowledge, @/pages/user-setting/data-source/hooks, @/pages/user-setting/data-source/interface, AddedSourceCard, Button, IConnector, IDataSourceBase, JSON, LinkDataSourceModal, Modal, SearchInput, TypeScript, handleFormSubmit, i18next, react

---
*Generated by RAGFlow Repository Documentation Generator*
