# Documentation: web/src/pages/add-knowledge/components/knowledge-file/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/index.tsx`
- **Size**: 8108 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-file/index.tsx`.

## Original Source Code

```tsx
import ChunkMethodModal from '@/components/chunk-method-modal';
import SvgIcon from '@/components/svg-icon';
import {
  useFetchNextDocumentList,
  useSetNextDocumentStatus,
} from '@/hooks/document-hooks';
import { useSetSelectedRecord } from '@/hooks/logic-hooks';
import { useSelectParserList } from '@/hooks/user-setting-hooks';
import { getExtension } from '@/utils/document-util';
import { Divider, Flex, Switch, Table, Tooltip, Typography } from 'antd';
import type { ColumnsType } from 'antd/es/table';
import { useTranslation } from 'react-i18next';
import CreateFileModal from './create-file-modal';
import DocumentToolbar from './document-toolbar';
import {
  useChangeDocumentParser,
  useCreateEmptyDocument,
  useGetRowSelection,
  useHandleUploadDocument,
  useHandleWebCrawl,
  useNavigateToOtherPage,
  useRenameDocument,
  useShowMetaModal,
} from './hooks';
import ParsingActionCell from './parsing-action-cell';
import ParsingStatusCell from './parsing-status-cell';
import RenameModal from './rename-modal';
import WebCrawlModal from './web-crawl-modal';

import FileUploadModal from '@/components/file-upload-modal';
import { RunningStatus } from '@/constants/knowledge';
import { IDocumentInfo } from '@/interfaces/database/document';
import { formatDate } from '@/utils/date';
import { CircleHelp } from 'lucide-react';
import styles from './index.less';
import { SetMetaModal } from './set-meta-modal';

const { Text } = Typography;

const KnowledgeFile = () => {
  const { searchString, documents, pagination, handleInputChange } =
    useFetchNextDocumentList();
  const parserList = useSelectParserList();
  const { setDocumentStatus } = useSetNextDocumentStatus();
  const { toChunk } = useNavigateToOtherPage();
  const { currentRecord, setRecord } = useSetSelectedRecord<IDocumentInfo>();
  const {
    renameLoading,
    onRenameOk,
    renameVisible,
    hideRenameModal,
    showRenameModal,
  } = useRenameDocument(currentRecord.id);
  const {
    createLoading,
    onCreateOk,
    createVisible,
    hideCreateModal,
    showCreateModal,
  } = useCreateEmptyDocument();
  const {
    changeParserLoading,
    onChangeParserOk,
    changeParserVisible,
    hideChangeParserModal,
    showChangeParserModal,
  } = useChangeDocumentParser(currentRecord.id);
  const {
    documentUploadVisible,
    hideDocumentUploadModal,
    showDocumentUploadModal,
    onDocumentUploadOk,
    documentUploadLoading,
    uploadFileList,
    setUploadFileList,
    uploadProgress,
    setUploadProgress,
  } = useHandleUploadDocument();
  const {
    webCrawlUploadVisible,
    hideWebCrawlUploadModal,
    showWebCrawlUploadModal,
    onWebCrawlUploadOk,
    webCrawlUploadLoading,
  } = useHandleWebCrawl();
  const { t } = useTranslation('translation', {
    keyPrefix: 'knowledgeDetails',
  });

  const {
    showSetMetaModal,
    hideSetMetaModal,
    setMetaVisible,
    setMetaLoading,
    onSetMetaModalOk,
  } = useShowMetaModal(currentRecord.id);

  const rowSelection = useGetRowSelection();

  const columns: ColumnsType<IDocumentInfo> = [
    {
      title: t('name'),
      dataIndex: 'name',
      key: 'name',
      fixed: 'left',
      render: (text: any, { id, thumbnail, name }) => (
        <div className={styles.toChunks} onClick={() => toChunk(id)}>
          <Flex gap={10} align="center">
            {thumbnail ? (
              <img className={styles.img} src={thumbnail} alt="" />
            ) : (
              <SvgIcon
                name={`file-icon/${getExtension(name)}`}
                width={24}
              ></SvgIcon>
            )}
            <Text ellipsis={{ tooltip: text }} className={styles.nameText}>
              {text}
            </Text>
          </Flex>
        </div>
      ),
    },
    {
      title: t('chunkNumber'),
      dataIndex: 'chunk_num',
      key: 'chunk_num',
    },
    {
      title: t('uploadDate'),
      dataIndex: 'create_time',
      key: 'create_time',
      render(value) {
        return formatDate(value);
      },
    },
    {
      title: t('chunkMethod'),
      dataIndex: 'parser_id',
      key: 'parser_id',
      render: (text) => {
        return parserList.find((x) => x.value === text)?.label;
      },
    },
    {
      title: t('enabled'),
      key: 'status',
      dataIndex: 'status',
      render: (_, { status, id }) => (
        <>
          <Switch
            checked={status === '1'}
            onChange={(e) => {
              setDocumentStatus({ status: e, documentId: id });
            }}
          />
        </>
      ),
    },
    {
      title: (
        <span className="flex items-center gap-2">
          {t('parsingStatus')}
          <Tooltip title={t('parsingStatusTip')}>
            <CircleHelp className="size-3" />
          </Tooltip>
        </span>
      ),
      dataIndex: 'run',
      key: 'run',
      filters: Object.values(RunningStatus).map((value) => ({
        text: t(`runningStatus${value}`),
        value: value,
      })),
      onFilter: (value, record: IDocumentInfo) => record.run === value,
      render: (text, record) => {
        return <ParsingStatusCell record={record}></ParsingStatusCell>;
      },
    },
    {
      title: t('action'),
      key: 'action',
      render: (_, record) => (
        <ParsingActionCell
          setCurrentRecord={setRecord}
          showRenameModal={showRenameModal}
          showChangeParserModal={showChangeParserModal}
          showSetMetaModal={showSetMetaModal}
          record={record}
        ></ParsingActionCell>
      ),
    },
  ];

  const finalColumns = columns.map((x) => ({
    ...x,
    className: `${styles.column}`,
  }));

  return (
    <div className={styles.datasetWrapper}>
      <h3>{t('dataset')}</h3>
      <p>{t('datasetDescription')}</p>
      <Divider></Divider>
      <DocumentToolbar
        selectedRowKeys={rowSelection.selectedRowKeys as string[]}
        showCreateModal={showCreateModal}
        showWebCrawlModal={showWebCrawlUploadModal}
        showDocumentUploadModal={showDocumentUploadModal}
        searchString={searchString}
        handleInputChange={handleInputChange}
        documents={documents}
      ></DocumentToolbar>
      <Table
        rowKey="id"
        columns={finalColumns}
        dataSource={documents}
        pagination={pagination}
        rowSelection={rowSelection}
        className={styles.documentTable}
        scroll={{ scrollToFirstRowOnChange: true, x: 1300 }}
      />
      <CreateFileModal
        visible={createVisible}
        hideModal={hideCreateModal}
        loading={createLoading}
        onOk={onCreateOk}
      />
      <ChunkMethodModal
        documentId={currentRecord.id}
        parserId={currentRecord.parser_id}
        parserConfig={currentRecord.parser_config}
        documentExtension={getExtension(currentRecord.name)}
        onOk={onChangeParserOk}
        visible={changeParserVisible}
        hideModal={hideChangeParserModal}
        loading={changeParserLoading}
      />
      <RenameModal
        visible={renameVisible}
        onOk={onRenameOk}
        loading={renameLoading}
        hideModal={hideRenameModal}
        initialName={currentRecord.name}
      ></RenameModal>
      <FileUploadModal
        visible={documentUploadVisible}
        hideModal={hideDocumentUploadModal}
        loading={documentUploadLoading}
        onOk={onDocumentUploadOk}
        uploadFileList={uploadFileList}
        setUploadFileList={setUploadFileList}
        uploadProgress={uploadProgress}
        setUploadProgress={setUploadProgress}
      ></FileUploadModal>
      <WebCrawlModal
        visible={webCrawlUploadVisible}
        hideModal={hideWebCrawlUploadModal}
        loading={webCrawlUploadLoading}
        onOk={onWebCrawlUploadOk}
      ></WebCrawlModal>
      {setMetaVisible && (
        <SetMetaModal
          visible={setMetaVisible}
          hideModal={hideSetMetaModal}
          onOk={onSetMetaModalOk}
          loading={setMetaLoading}
          initialMetaData={currentRecord.meta_fields}
        ></SetMetaModal>
      )}
    </div>
  );
};

export default KnowledgeFile;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-file/index.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-file` directory.

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
- [document-toolbar.tsx](document-toolbar.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [utils.ts](utils.ts_docs.md)
- [web-crawl-modal.tsx](web-crawl-modal.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
