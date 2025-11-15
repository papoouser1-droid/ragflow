# Documentation: web/src/pages/search/index.tsx

## File Metadata

- **Path**: `web/src/pages/search/index.tsx`
- **Size**: 9947 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/search/index.tsx`.

## Original Source Code

```tsx
import FileIcon from '@/components/file-icon';
import HightLightMarkdown from '@/components/highlight-markdown';
import { ImageWithPopover } from '@/components/image';
import PdfDrawer from '@/components/pdf-drawer';
import { useClickDrawer } from '@/components/pdf-drawer/hooks';
import RetrievalDocuments from '@/components/retrieval-documents';
import SvgIcon from '@/components/svg-icon';
import {
  useFetchKnowledgeList,
  useSelectTestingResult,
} from '@/hooks/knowledge-hooks';
import { useGetPaginationWithRouter } from '@/hooks/logic-hooks';
import { IReference } from '@/interfaces/database/chat';
import {
  Button,
  Card,
  Divider,
  Flex,
  FloatButton,
  Input,
  Layout,
  List,
  Pagination,
  PaginationProps,
  Popover,
  Skeleton,
  Space,
  Spin,
  Tag,
  Tooltip,
} from 'antd';
import classNames from 'classnames';
import DOMPurify from 'dompurify';
import { isEmpty } from 'lodash';
import { CircleStop, SendHorizontal } from 'lucide-react';
import { useCallback, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import MarkdownContent from '../chat/markdown-content';
import { useSendQuestion, useShowMindMapDrawer } from './hooks';
import styles from './index.less';
import MindMapDrawer from './mindmap-drawer';
import SearchSidebar from './sidebar';

const { Content } = Layout;
const { Search } = Input;

const SearchPage = () => {
  const { t } = useTranslation();
  const [checkedList, setCheckedList] = useState<string[]>([]);
  const { chunks, total } = useSelectTestingResult();
  const { list: knowledgeList } = useFetchKnowledgeList();
  const checkedWithoutEmbeddingIdList = useMemo(() => {
    return checkedList.filter((x) => knowledgeList.some((y) => y.id === x));
  }, [checkedList, knowledgeList]);

  const {
    sendQuestion,
    handleClickRelatedQuestion,
    handleSearchStrChange,
    handleTestChunk,
    setSelectedDocumentIds,
    answer,
    sendingLoading,
    relatedQuestions,
    searchStr,
    loading,
    isFirstRender,
    selectedDocumentIds,
    isSearchStrEmpty,
    stopOutputMessage,
  } = useSendQuestion(checkedWithoutEmbeddingIdList);
  const { visible, hideModal, documentId, selectedChunk, clickDocumentButton } =
    useClickDrawer();
  const { pagination } = useGetPaginationWithRouter();
  const {
    mindMapVisible,
    hideMindMapModal,
    showMindMapModal,
    mindMapLoading,
    mindMap,
  } = useShowMindMapDrawer(checkedWithoutEmbeddingIdList, searchStr);

  const onChange: PaginationProps['onChange'] = (pageNumber, pageSize) => {
    pagination.onChange?.(pageNumber, pageSize);
    handleTestChunk(selectedDocumentIds, pageNumber, pageSize);
  };

  const handleSearch = useCallback(() => {
    sendQuestion(searchStr);
  }, [searchStr, sendQuestion]);

  const InputSearch = (
    <Search
      value={searchStr}
      onChange={handleSearchStrChange}
      placeholder={t('header.search')}
      allowClear
      addonAfter={
        sendingLoading ? (
          <Button onClick={stopOutputMessage}>
            <CircleStop />
          </Button>
        ) : (
          <Button onClick={handleSearch}>
            <SendHorizontal className="size-5 text-blue-500" />
          </Button>
        )
      }
      onSearch={sendQuestion}
      size="large"
      loading={sendingLoading}
      disabled={checkedWithoutEmbeddingIdList.length === 0}
      className={classNames(
        styles.searchInput,
        isFirstRender ? styles.globalInput : styles.partialInput,
      )}
    />
  );

  return (
    <>
      <Layout className={styles.searchPage}>
        <SearchSidebar
          isFirstRender={isFirstRender}
          checkedList={checkedWithoutEmbeddingIdList}
          setCheckedList={setCheckedList}
        ></SearchSidebar>
        <Layout className={isFirstRender ? styles.mainLayout : ''}>
          <Content>
            {isFirstRender ? (
              <Flex justify="center" className={styles.firstRenderContent}>
                <Flex vertical align="center" gap={'large'}>
                  {InputSearch}
                </Flex>
              </Flex>
            ) : (
              <Flex className={styles.content}>
                <section className={styles.main}>
                  {InputSearch}
                  <Card
                    title={
                      <Flex gap={10}>
                        <img src="/logo.svg" alt="" width={20} />
                        {t('chat.answerTitle')}
                      </Flex>
                    }
                    className={styles.answerWrapper}
                  >
                    {isEmpty(answer) && sendingLoading ? (
                      <Skeleton active />
                    ) : (
                      answer.answer && (
                        <MarkdownContent
                          loading={sendingLoading}
                          content={answer.answer}
                          reference={answer.reference ?? ({} as IReference)}
                          clickDocumentButton={clickDocumentButton}
                        ></MarkdownContent>
                      )
                    )}
                  </Card>
                  <Divider></Divider>
                  <RetrievalDocuments
                    selectedDocumentIds={selectedDocumentIds}
                    setSelectedDocumentIds={setSelectedDocumentIds}
                    onTesting={handleTestChunk}
                  ></RetrievalDocuments>
                  <Divider></Divider>
                  <Spin spinning={loading}>
                    {chunks?.length > 0 && (
                      <List
                        dataSource={chunks || []}
                        className={styles.chunks}
                        renderItem={(item) => (
                          <List.Item>
                            <Card className={styles.card}>
                              <Space>
                                <ImageWithPopover
                                  id={item.img_id}
                                ></ImageWithPopover>
                                <Flex vertical gap={10}>
                                  <Popover
                                    content={
                                      <div className={styles.popupMarkdown}>
                                        <HightLightMarkdown>
                                          {item.content_with_weight}
                                        </HightLightMarkdown>
                                      </div>
                                    }
                                  >
                                    <div
                                      dangerouslySetInnerHTML={{
                                        __html: DOMPurify.sanitize(
                                          `${item.highlight}...`,
                                        ),
                                      }}
                                      className={styles.highlightContent}
                                    ></div>
                                  </Popover>
                                  <Space
                                    className={styles.documentReference}
                                    onClick={() =>
                                      clickDocumentButton(
                                        item.doc_id,
                                        item as any,
                                      )
                                    }
                                  >
                                    <FileIcon
                                      id={item.image_id}
                                      name={item.docnm_kwd}
                                    ></FileIcon>
                                    {item.docnm_kwd}
                                  </Space>
                                </Flex>
                              </Space>
                            </Card>
                          </List.Item>
                        )}
                      />
                    )}
                  </Spin>
                  {relatedQuestions?.length > 0 && (
                    <Card title={t('chat.relatedQuestion')}>
                      <Flex wrap="wrap" gap={'10px 0'}>
                        {relatedQuestions?.map((x, idx) => (
                          <Tag
                            key={idx}
                            className={styles.tag}
                            onClick={handleClickRelatedQuestion(x)}
                          >
                            {x}
                          </Tag>
                        ))}
                      </Flex>
                    </Card>
                  )}
                  <Divider></Divider>
                  <Pagination
                    {...pagination}
                    total={total}
                    onChange={onChange}
                    className={styles.pagination}
                  />
                </section>
              </Flex>
            )}
          </Content>
        </Layout>
      </Layout>
      {!isFirstRender &&
        !isSearchStrEmpty &&
        !isEmpty(checkedWithoutEmbeddingIdList) && (
          <Tooltip title={t('chunk.mind')} zIndex={1}>
            <FloatButton
              className={styles.mindMapFloatButton}
              onClick={showMindMapModal}
              icon={
                <SvgIcon name="paper-clip" width={24} height={30}></SvgIcon>
              }
            />
          </Tooltip>
        )}
      {visible && (
        <PdfDrawer
          visible={visible}
          hideModal={hideModal}
          documentId={documentId}
          chunk={selectedChunk}
        ></PdfDrawer>
      )}
      {mindMapVisible && (
        <MindMapDrawer
          visible={mindMapVisible}
          hideModal={hideMindMapModal}
          data={mindMap}
          loading={mindMapLoading}
        ></MindMapDrawer>
      )}
    </>
  );
};

export default SearchPage;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/search/index.tsx` is located in the `web/src/pages/search` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to search.

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
- [mindmap-drawer.tsx](mindmap-drawer.tsx_docs.md)
- [sidebar.tsx](sidebar.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
