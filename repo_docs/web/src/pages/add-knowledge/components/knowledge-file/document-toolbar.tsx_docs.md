# File Documentation: web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx`
- **Extension**: `.tsx`
- **Lines**: 241
- **Characters**: 6,011
- **Size**: 6,011 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ReactComponent as CancelIcon } from '@/assets/svg/cancel.svg';
import { ReactComponent as DeleteIcon } from '@/assets/svg/delete.svg';
import { ReactComponent as DisableIcon } from '@/assets/svg/disable.svg';
import { ReactComponent as EnableIcon } from '@/assets/svg/enable.svg';
import { ReactComponent as RunIcon } from '@/assets/svg/run.svg';
import { useShowDeleteConfirm, useTranslate } from '@/hooks/common-hooks';
import {
  useRemoveNextDocument,
  useRunNextDocument,
  useSetNextDocumentStatus,
} from '@/hooks/document-hooks';
import { IDocumentInfo } from '@/interfaces/database/document';
import {
  DownOutlined,
  FileOutlined,
  FileTextOutlined,
  PlusOutlined,
  SearchOutlined,
} from '@ant-design/icons';
import { Button, Dropdown, Flex, Input, MenuProps, Space } from 'antd';
import { useCallback, useMemo } from 'react';
import { toast } from 'sonner';
import { RunningStatus } from './constant';

import styles from './index.less';

interface IProps {
  selectedRowKeys: string[];
  showCreateModal(): void;
  showWebCrawlModal(): void;
  showDocumentUploadModal(): void;
  searchString: string;
  handleInputChange: React.ChangeEventHandler<HTMLInputElement>;
  documents: IDocumentInfo[];
}

const DocumentToolbar = ({
  searchString,
  selectedRowKeys,
  showCreateModal,
  showDocumentUploadModal,
  handleInputChange,
  documents,
}: IProps) => {
  const { t } = useTranslate('knowledgeDetails');
  const { removeDocument } = useRemoveNextDocument();
  const showDeleteConfirm = useShowDeleteConfirm();
  const { runDocumentByIds } = useRunNextDocument();
  const { setDocumentStatus } = useSetNextDocumentStatus();

  const actionItems: MenuProps['items'] = useMemo(() => {
    return [
      {
        key: '1',
        onClick: showDocumentUploadModal,
        label: (
          <div>
            <Button type="link">
              <Space>
                <FileTextOutlined />
                {t('localFiles')}
              </Space>
            </Button>
          </div>
        ),
      },
      { type: 'divider' },
      {
        key: '3',
        onClick: showCreateModal,
        label: (
          <div>
            <Button type="link">
              <FileOutlined />
              {t('emptyFiles')}
            </Button>
          </div>
        ),
      },
    ];
  }, [showDocumentUploadModal, showCreateModal, t]);

  const handleDelete = useCallback(() => {
    const deletedKeys = selectedRowKeys.filter(
      (x) =>
        !documents
          .filter((y) => y.run === RunningStatus.RUNNING)
          .some((y) => y.id === x),
    );
    if (deletedKeys.length === 0) {
      toast.error(t('theDocumentBeingParsedCannotBeDeleted'));
      return;
    }
    showDeleteConfirm({
      onOk: () => {
        removeDocument(deletedKeys);
      },
    });
  }, [selectedRowKeys, showDeleteConfirm, documents, t, removeDocument]);

  const runDocument = useCallback(
    (run: number) => {
      runDocumentByIds({
        documentIds: selectedRowKeys,
        run,
        shouldDelete: false,
      });
    },
    [runDocumentByIds, selectedRowKeys],
  );

  const handleRunClick = useCallback(() => {
    runDocument(1);
  }, [runDocument]);

  const handleCancelClick = useCallback(() => {
    runDocument(2);
  }, [runDocument]);

  const onChangeStatus = useCallback(
    (enabled: boolean) => {
      selectedRowKeys.forEach((id) => {
        setDocumentStatus({ status: enabled, documentId: id });
      });
    },
    [selectedRowKeys, setDocumentStatus],
  );

  const handleEnableClick = useCallback(() => {
    onChangeStatus(true);
  }, [onChangeStatus]);

  const handleDisableClick = useCallback(() => {
    onChangeStatus(false);
  }, [onChangeStatus]);

  const disabled = selectedRowKeys.length === 0;

  const items: MenuProps['items'] = useMemo(() => {
    return [
      {
        key: '0',
        onClick: handleEnableClick,
        label: (
          <Flex gap={10}>
            <EnableIcon></EnableIcon>
            <b>{t('enabled')}</b>
          </Flex>
        ),
      },
      {
        key: '1',
        onClick: handleDisableClick,
        label: (
          <Flex gap={10}>
            <DisableIcon></DisableIcon>
            <b>{t('disabled')}</b>
          </Flex>
        ),
      },
      { type: 'divider' },
      {
        key: '2',
        onClick: handleRunClick,
        label: (
          <Flex gap={10}>
            <RunIcon></RunIcon>
            <b>{t('run')}</b>
          </Flex>
        ),
      },
      {
        key: '3',
        onClick: handleCancelClick,
        label: (
          <Flex gap={10}>
            <CancelIcon />
            <b>{t('cancel')}</b>
          </Flex>
        ),
      },
      { type: 'divider' },
      {
        key: '4',
        onClick: handleDelete,
        label: (
          <Flex gap={10}>
            <span className={styles.deleteIconWrapper}>
              <DeleteIcon width={18} />
            </span>
            <b>{t('delete', { keyPrefix: 'common' })}</b>
          </Flex>
        ),
      },
    ];
  }, [
    handleDelete,
    handleRunClick,
    handleCancelClick,
    t,
    handleDisableClick,
    handleEnableClick,
  ]);

  return (
    <div className={styles.filter}>
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
      <Space>
        <Input
          placeholder={t('searchFiles')}
          value={searchString}
          style={{ width: 220 }}
          allowClear
          onChange={handleInputChange}
          prefix={<SearchOutlined />}
        />

        <Dropdown menu={{ items: actionItems }} trigger={['click']}>
          <Button type="primary" icon={<PlusOutlined />}>
            {t('addFile')}
          </Button>
        </Dropdown>
      </Space>
    </div>
  );
};

export default DocumentToolbar;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 241 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (9)

- `DocumentToolbar()`: Function definition
- `handleDelete()`: Function definition
- `deletedKeys()`: Function definition
- `runDocument()`: Function definition
- `handleRunClick()`: Function definition
- `handleCancelClick()`: Function definition
- `onChangeStatus()`: Function definition
- `handleEnableClick()`: Function definition
- `handleDisableClick()`: Function definition

### Imports (14)

- `import { ReactComponent as CancelIcon } from '@/assets/svg/cancel.svg';`
- `import { ReactComponent as DeleteIcon } from '@/assets/svg/delete.svg';`
- `import { ReactComponent as DisableIcon } from '@/assets/svg/disable.svg';`
- `import { ReactComponent as EnableIcon } from '@/assets/svg/enable.svg';`
- `import { ReactComponent as RunIcon } from '@/assets/svg/run.svg';`
- `import { useShowDeleteConfirm, useTranslate } from '@/hooks/common-hooks';`
- `import {`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import {`
- `import { Button, Dropdown, Flex, Input, MenuProps, Space } from 'antd';`

## Code Structure Analysis

- Total lines: 241
- Blank lines: 17 (7.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~224


## Dependencies and Imports

- `@/assets/svg/cancel.svg`
- `@/assets/svg/delete.svg`
- `@/assets/svg/disable.svg`
- `@/assets/svg/enable.svg`
- `@/assets/svg/run.svg`
- `@/hooks/common-hooks`
- `@/interfaces/database/document`
- `antd`
- `react`
- `sonner`
- `./constant`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-file`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-file/` directory
- Potential test file: `test_document-toolbar.tsx`

## Keywords

./constant, ./index.less, @/assets/svg/cancel.svg, @/assets/svg/delete.svg, @/assets/svg/disable.svg, @/assets/svg/enable.svg, @/assets/svg/run.svg, @/hooks/common-hooks, @/interfaces/database/document, Button, CancelIcon, ChangeEventHandler, DeleteIcon, DisableIcon, DocumentToolbar, DownOutlined, Dropdown, EnableIcon, FileOutlined, FileTextOutlined, Flex, HTMLInputElement, IDocumentInfo, IProps, Input, MenuProps, PlusOutlined, RUNNING, React, ReactComponent, RunIcon, RunningStatus, SearchOutlined, Space, TypeScript, actionItems, ant, antd, deletedKeys, disabled, handleCancelClick, handleDelete, handleDisableClick, handleEnableClick, handleRunClick, items, onChangeStatus, react, runDocument, showDeleteConfirm...

---
*Generated by RAGFlow Repository Documentation Generator*
