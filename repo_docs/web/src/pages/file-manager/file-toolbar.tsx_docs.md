# File Documentation: web/src/pages/file-manager/file-toolbar.tsx

## File Metadata

- **Path**: `web/src/pages/file-manager/file-toolbar.tsx`
- **Extension**: `.tsx`
- **Lines**: 192
- **Characters**: 4,819
- **Size**: 4,819 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import {
  IListResult,
  useFetchParentFolderList,
} from '@/hooks/file-manager-hooks';
import {
  DownOutlined,
  FileTextOutlined,
  FolderOpenOutlined,
  PlusOutlined,
  SearchOutlined,
} from '@ant-design/icons';
import {
  Breadcrumb,
  BreadcrumbProps,
  Button,
  Dropdown,
  Flex,
  Input,
  MenuProps,
  Space,
} from 'antd';
import { useCallback, useMemo } from 'react';
import {
  useHandleBreadcrumbClick,
  useHandleDeleteFile,
  useSelectBreadcrumbItems,
} from './hooks';

import { FolderInput, Trash2 } from 'lucide-react';
import styles from './index.less';

interface IProps
  extends Pick<IListResult, 'searchString' | 'handleInputChange'> {
  selectedRowKeys: string[];
  showFolderCreateModal: () => void;
  showFileUploadModal: () => void;
  setSelectedRowKeys: (keys: string[]) => void;
  showMoveFileModal: (ids: string[]) => void;
}

const FileToolbar = ({
  selectedRowKeys,
  showFolderCreateModal,
  showFileUploadModal,
  setSelectedRowKeys,
  searchString,
  handleInputChange,
  showMoveFileModal,
}: IProps) => {
  const { t } = useTranslate('knowledgeDetails');
  const breadcrumbItems = useSelectBreadcrumbItems();
  const { handleBreadcrumbClick } = useHandleBreadcrumbClick();
  const parentFolderList = useFetchParentFolderList();
  const isKnowledgeBase =
    parentFolderList.at(-1)?.source_type === 'knowledgebase';

  const itemRender: BreadcrumbProps['itemRender'] = (
    currentRoute,
    params,
    items,
  ) => {
    const isLast = currentRoute?.path === items[items.length - 1]?.path;

    return isLast ? (
      <span>{currentRoute.title}</span>
    ) : (
      <span
        className={styles.breadcrumbItemButton}
        onClick={() => handleBreadcrumbClick(currentRoute.path)}
      >
        {currentRoute.title}
      </span>
    );
  };

  const actionItems: MenuProps['items'] = useMemo(() => {
    return [
      {
        key: '1',
        onClick: showFileUploadModal,
        label: (
          <div>
            <Button type="link">
              <Space>
                <FileTextOutlined />
                {t('uploadFile', { keyPrefix: 'fileManager' })}
              </Space>
            </Button>
          </div>
        ),
      },
      { type: 'divider' },
      {
        key: '2',
        onClick: showFolderCreateModal,
        label: (
          <div>
            <Button type="link">
              <Space>
                <FolderOpenOutlined />
                {t('newFolder', { keyPrefix: 'fileManager' })}
              </Space>
            </Button>
          </div>
        ),
      },
    ];
  }, [t, showFolderCreateModal, showFileUploadModal]);

  const { handleRemoveFile } = useHandleDeleteFile(
    selectedRowKeys,
    setSelectedRowKeys,
  );

  const handleShowMoveFileModal = useCallback(() => {
    showMoveFileModal(selectedRowKeys);
  }, [selectedRowKeys, showMoveFileModal]);

  const disabled = selectedRowKeys.length === 0;

  const items: MenuProps['items'] = useMemo(() => {
    return [
      {
        key: '4',
        onClick: handleRemoveFile,
        label: (
          <Flex gap={10}>
            <span className="flex items-center justify-center">
              <Trash2 className="size-4" />
            </span>
            <b>{t('delete', { keyPrefix: 'common' })}</b>
          </Flex>
        ),
      },
      {
        key: '5',
        onClick: handleShowMoveFileModal,
        label: (
          <Flex gap={10}>
            <span className="flex items-center justify-center">
              <FolderInput className="size-4"></FolderInput>
            </span>
            <b>{t('move', { keyPrefix: 'common' })}</b>
          </Flex>
        ),
      },
    ];
  }, [handleShowMoveFileModal, t, handleRemoveFile]);

  return (
    <div className={styles.filter}>
      <Breadcrumb items={breadcrumbItems} itemRender={itemRender} />
      <Space>
        {isKnowledgeBase || (
          <Dropdown
            menu={{ items }}
            placement="bottom"
            arrow={false}
            disabled={disabled}
          >
            <Button>
              <Space>
                <b> {t('bulk')}</b>
                <DownOutlined />
              </Space>
            </Button>
          </Dropdown>
        )}
        <Input
          placeholder={t('searchFiles')}
          value={searchString}
          style={{ width: 220 }}
          allowClear
          onChange={handleInputChange}
          prefix={<SearchOutlined />}
        />

        {isKnowledgeBase || (
          <Dropdown menu={{ items: actionItems }} trigger={['click']}>
            <Button type="primary" icon={<PlusOutlined />}>
              {t('addFile')}
            </Button>
          </Dropdown>
        )}
      </Space>
    </div>
  );
};

export default FileToolbar;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/file-manager/file-toolbar.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 192 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `FileToolbar()`: Function definition
- `handleShowMoveFileModal()`: Function definition

### Imports (8)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import {`
- `import {`
- `import { useCallback, useMemo } from 'react';`
- `import {`
- `import { FolderInput, Trash2 } from 'lucide-react';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 192
- Blank lines: 14 (7.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~178


## Dependencies and Imports

- `@/hooks/common-hooks`
- `react`
- `lucide-react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/file-manager`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/file-manager/` directory
- Potential test file: `test_file-toolbar.tsx`

## Keywords

./index.less, @/hooks/common-hooks, Breadcrumb, BreadcrumbProps, Button, DownOutlined, Dropdown, FileTextOutlined, FileToolbar, Flex, FolderInput, FolderOpenOutlined, IListResult, IProps, Input, MenuProps, Pick, PlusOutlined, SearchOutlined, Space, Trash2, TypeScript, actionItems, ant, breadcrumbItems, disabled, handleShowMoveFileModal, isKnowledgeBase, isLast, itemRender, items, lucide-react, parentFolderList, react

---
*Generated by RAGFlow Repository Documentation Generator*
