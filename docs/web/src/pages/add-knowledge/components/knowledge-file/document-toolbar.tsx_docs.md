# Documentation: web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx`
- **Size**: 6011 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-file/document-toolbar.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-file` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge-file.

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

- [constant.ts](constant.ts_docs.md)
- [create-file-modal.tsx](create-file-modal.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [utils.ts](utils.ts_docs.md)
- [web-crawl-modal.tsx](web-crawl-modal.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
