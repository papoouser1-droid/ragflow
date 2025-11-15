# Documentation: web/src/pages/file-manager/file-toolbar.tsx

## File Metadata

- **Path**: `web/src/pages/file-manager/file-toolbar.tsx`
- **Size**: 4819 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/file-manager/file-toolbar.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/file-manager/file-toolbar.tsx` is located in the `web/src/pages/file-manager` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to file-manager.

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

- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
