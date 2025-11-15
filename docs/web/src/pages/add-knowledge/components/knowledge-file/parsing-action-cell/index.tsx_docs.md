# Documentation: web/src/pages/add-knowledge/components/knowledge-file/parsing-action-cell/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/parsing-action-cell/index.tsx`
- **Size**: 3921 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-file/parsing-action-cell/index.tsx`.

## Original Source Code

```tsx
import { useShowDeleteConfirm, useTranslate } from '@/hooks/common-hooks';
import { useRemoveNextDocument } from '@/hooks/document-hooks';
import { IDocumentInfo } from '@/interfaces/database/document';
import { downloadDocument } from '@/utils/file-util';
import {
  DeleteOutlined,
  DownloadOutlined,
  EditOutlined,
  ToolOutlined,
} from '@ant-design/icons';
import { Button, Dropdown, MenuProps, Space, Tooltip } from 'antd';
import { isParserRunning } from '../utils';

import { useCallback } from 'react';
import { DocumentType } from '../constant';
import styles from './index.less';

interface IProps {
  record: IDocumentInfo;
  setCurrentRecord: (record: IDocumentInfo) => void;
  showRenameModal: () => void;
  showChangeParserModal: () => void;
  showSetMetaModal: () => void;
}

const ParsingActionCell = ({
  record,
  setCurrentRecord,
  showRenameModal,
  showChangeParserModal,
  showSetMetaModal,
}: IProps) => {
  const documentId = record.id;
  const isRunning = isParserRunning(record.run);
  const { t } = useTranslate('knowledgeDetails');
  const { removeDocument } = useRemoveNextDocument();
  const showDeleteConfirm = useShowDeleteConfirm();
  const isVirtualDocument = record.type === DocumentType.Virtual;

  const onRmDocument = () => {
    if (!isRunning) {
      showDeleteConfirm({
        onOk: () => removeDocument([documentId]),
        content: record?.parser_config?.graphrag?.use_graphrag
          ? t('deleteDocumentConfirmContent')
          : '',
      });
    }
  };

  const onDownloadDocument = () => {
    downloadDocument({
      id: documentId,
      filename: record.name,
    });
  };

  const setRecord = useCallback(() => {
    setCurrentRecord(record);
  }, [record, setCurrentRecord]);

  const onShowRenameModal = () => {
    setRecord();
    showRenameModal();
  };
  const onShowChangeParserModal = () => {
    setRecord();
    showChangeParserModal();
  };

  const onShowSetMetaModal = useCallback(() => {
    setRecord();
    showSetMetaModal();
  }, [setRecord, showSetMetaModal]);

  const chunkItems: MenuProps['items'] = [
    {
      key: '1',
      label: (
        <div className="flex flex-col">
          <Button type="link" onClick={onShowChangeParserModal}>
            {t('chunkMethod')}
          </Button>
        </div>
      ),
    },
    { type: 'divider' },
    {
      key: '2',
      label: (
        <div className="flex flex-col">
          <Button type="link" onClick={onShowSetMetaModal}>
            {t('setMetaData')}
          </Button>
        </div>
      ),
    },
  ];

  return (
    <Space size={0}>
      {isVirtualDocument || (
        <Dropdown
          menu={{ items: chunkItems }}
          trigger={['click']}
          disabled={isRunning || record.parser_id === 'tag'}
        >
          <Button type="text" className={styles.iconButton}>
            <ToolOutlined size={20} />
          </Button>
        </Dropdown>
      )}
      <Tooltip title={t('rename', { keyPrefix: 'common' })}>
        <Button
          type="text"
          disabled={isRunning}
          onClick={onShowRenameModal}
          className={styles.iconButton}
        >
          <EditOutlined size={20} />
        </Button>
      </Tooltip>
      <Tooltip title={t('delete', { keyPrefix: 'common' })}>
        <Button
          type="text"
          disabled={isRunning}
          onClick={onRmDocument}
          className={styles.iconButton}
        >
          <DeleteOutlined size={20} />
        </Button>
      </Tooltip>
      {isVirtualDocument || (
        <Tooltip title={t('download', { keyPrefix: 'common' })}>
          <Button
            type="text"
            disabled={isRunning}
            onClick={onDownloadDocument}
            className={styles.iconButton}
          >
            <DownloadOutlined size={20} />
          </Button>
        </Tooltip>
      )}
    </Space>
  );
};

export default ParsingActionCell;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-file/parsing-action-cell/index.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-file/parsing-action-cell` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parsing-action-cell.

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

- [index.less](index.less_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
