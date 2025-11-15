# File Documentation: web/src/pages/files/move-dialog.tsx

## File Metadata

- **Path**: `web/src/pages/files/move-dialog.tsx`
- **Extension**: `.tsx`
- **Lines**: 85
- **Characters**: 2,316
- **Size**: 2,316 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  AsyncTreeSelect,
  TreeNodeType,
} from '@/components/ui/async-tree-select';
import { ButtonLoading } from '@/components/ui/button';
import {
  Dialog,
  DialogContent,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import { useFetchPureFileList } from '@/hooks/file-manager-hooks';
import { IModalProps } from '@/interfaces/common';
import { IFile } from '@/interfaces/database/file-manager';
import { isEmpty } from 'lodash';
import { useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';

export function MoveDialog({ hideModal, onOk, loading }: IModalProps<any>) {
  const { t } = useTranslation();

  const { fetchList } = useFetchPureFileList();

  const [treeValue, setTreeValue] = useState<number | string>('');

  const [treeData, setTreeData] = useState([]);

  const onLoadData = useCallback(
    async ({ id }: TreeNodeType) => {
      const ret = await fetchList(id as string);
      if (ret.code === 0) {
        setTreeData((tree) => {
          return tree.concat(
            ret.data.files
              .filter((x: IFile) => x.type === 'folder')
              .map((x: IFile) => ({
                id: x.id,
                parentId: x.parent_id,
                title: x.name,
                isLeaf:
                  typeof x.has_child_folder === 'boolean'
                    ? !x.has_child_folder
                    : false,
              })),
          );
        });
      }
    },
    [fetchList],
  );

  const handleSubmit = useCallback(() => {
    onOk?.(treeValue);
  }, [onOk, treeValue]);

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>{t('common.move')}</DialogTitle>
        </DialogHeader>
        <div>
          <AsyncTreeSelect
            treeData={treeData}
            value={treeValue}
            onChange={setTreeValue}
            loadData={onLoadData}
          ></AsyncTreeSelect>
        </div>
        <DialogFooter>
          <ButtonLoading
            type="submit"
            onClick={handleSubmit}
            disabled={isEmpty(treeValue)}
            loading={loading}
          >
            {t('common.save')}
          </ButtonLoading>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/files/move-dialog.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 85 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `MoveDialog`: Exported entity

### Functions (3)

- `MoveDialog()`: Function definition
- `onLoadData()`: Function definition
- `handleSubmit()`: Function definition

### Imports (9)

- `import {`
- `import { ButtonLoading } from '@/components/ui/button';`
- `import {`
- `import { useFetchPureFileList } from '@/hooks/file-manager-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import { IFile } from '@/interfaces/database/file-manager';`
- `import { isEmpty } from 'lodash';`
- `import { useCallback, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 85
- Blank lines: 8 (9.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~77


## Dependencies and Imports

- `@/components/ui/button`
- `@/hooks/file-manager-hooks`
- `@/interfaces/common`
- `@/interfaces/database/file-manager`
- `lodash`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/files`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/files/` directory
- Potential test file: `test_move-dialog.tsx`

## Keywords

@/components/ui/button, @/hooks/file-manager-hooks, @/interfaces/common, @/interfaces/database/file-manager, AsyncTreeSelect, ButtonLoading, Dialog, DialogContent, DialogFooter, DialogHeader, DialogTitle, IFile, IModalProps, MoveDialog, TreeNodeType, TypeScript, handleSubmit, lodash, onLoadData, react, react-i18next, ret

---
*Generated by RAGFlow Repository Documentation Generator*
