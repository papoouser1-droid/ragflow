# File Documentation: web/src/pages/add-knowledge/components/knowledge-file/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 276
- **Characters**: 8,108
- **Size**: 8,108 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-file/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 276 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `KnowledgeFile()`: Function definition
- `finalColumns()`: Function definition

### Imports (23)

- `import ChunkMethodModal from '@/components/chunk-method-modal';`
- `import SvgIcon from '@/components/svg-icon';`
- `import {`
- `import { useSetSelectedRecord } from '@/hooks/logic-hooks';`
- `import { useSelectParserList } from '@/hooks/user-setting-hooks';`
- `import { getExtension } from '@/utils/document-util';`
- `import { Divider, Flex, Switch, Table, Tooltip, Typography } from 'antd';`
- `import type { ColumnsType } from 'antd/es/table';`
- `import { useTranslation } from 'react-i18next';`
- `import CreateFileModal from './create-file-modal';`

## Code Structure Analysis

- Total lines: 276
- Blank lines: 10 (3.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~266


## Dependencies and Imports

- `@/components/chunk-method-modal`
- `@/components/svg-icon`
- `@/hooks/logic-hooks`
- `@/hooks/user-setting-hooks`
- `@/utils/document-util`
- `antd`
- `antd/es/table`
- `react-i18next`
- `./create-file-modal`
- `./document-toolbar`
- `./parsing-action-cell`
- `./parsing-status-cell`
- `./rename-modal`
- `./web-crawl-modal`
- `@/components/file-upload-modal`
- `@/constants/knowledge`
- `@/interfaces/database/document`
- `@/utils/date`
- `lucide-react`
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
- Potential test file: `test_index.tsx`

## Keywords

./create-file-modal, ./document-toolbar, ./index.less, ./parsing-action-cell, ./parsing-status-cell, ./rename-modal, ./set-meta-modal, ./web-crawl-modal, @/components/chunk-method-modal, @/components/file-upload-modal, @/components/svg-icon, @/constants/knowledge, @/hooks/logic-hooks, @/hooks/user-setting-hooks, @/interfaces/database/document, @/utils/date, @/utils/document-util, ChunkMethodModal, CircleHelp, ColumnsType, CreateFileModal, Divider, DocumentToolbar, FileUploadModal, Flex, IDocumentInfo, KnowledgeFile, Object, ParsingActionCell, ParsingStatusCell, RenameModal, RunningStatus, SetMetaModal, SvgIcon, Switch, Table, Text, Tooltip, TypeScript, Typography, WebCrawlModal, antd, antd/es/table, columns, finalColumns, lucide-react, parserList, react-i18next, rowSelection

---
*Generated by RAGFlow Repository Documentation Generator*
