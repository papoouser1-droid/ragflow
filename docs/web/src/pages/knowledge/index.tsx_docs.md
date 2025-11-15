# Documentation: web/src/pages/knowledge/index.tsx

## File Metadata

- **Path**: `web/src/pages/knowledge/index.tsx`
- **Size**: 3714 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/knowledge/index.tsx`.

## Original Source Code

```tsx
import { useInfiniteFetchKnowledgeList } from '@/hooks/knowledge-hooks';
import { useFetchUserInfo } from '@/hooks/user-setting-hooks';
import { PlusOutlined, SearchOutlined } from '@ant-design/icons';
import {
  Button,
  Divider,
  Empty,
  Flex,
  Input,
  Skeleton,
  Space,
  Spin,
} from 'antd';
import { useTranslation } from 'react-i18next';
import InfiniteScroll from 'react-infinite-scroll-component';
import { useSaveKnowledge } from './hooks';
import KnowledgeCard from './knowledge-card';
import KnowledgeCreatingModal from './knowledge-creating-modal';

import { useMemo } from 'react';
import styles from './index.less';

const KnowledgeList = () => {
  const { data: userInfo } = useFetchUserInfo();
  const { t } = useTranslation('translation', { keyPrefix: 'knowledgeList' });
  const {
    visible,
    hideModal,
    showModal,
    onCreateOk,
    loading: creatingLoading,
  } = useSaveKnowledge();
  const {
    fetchNextPage,
    data,
    hasNextPage,
    searchString,
    handleInputChange,
    loading,
  } = useInfiniteFetchKnowledgeList();

  const nextList = useMemo(() => {
    const list =
      data?.pages?.flatMap((x) => (Array.isArray(x.kbs) ? x.kbs : [])) ?? [];
    return list;
  }, [data?.pages]);

  const total = useMemo(() => {
    return data?.pages.at(-1).total ?? 0;
  }, [data?.pages]);

  return (
    <Flex className={styles.knowledge} vertical flex={1}>
      <div className={styles.topWrapper}>
        <div>
          <span className={styles.title}>
            {t('welcome')}, {userInfo.nickname}
          </span>
          <p className={styles.description}>{t('description')}</p>
        </div>
        <Space size={'large'}>
          <Input
            placeholder={t('searchKnowledgePlaceholder')}
            value={searchString}
            style={{ width: 220 }}
            allowClear
            onChange={handleInputChange}
            prefix={<SearchOutlined />}
          />

          <Button
            type="primary"
            icon={<PlusOutlined />}
            onClick={showModal}
            className={styles.topButton}
          >
            {t('createKnowledgeBase')}
          </Button>
        </Space>
      </div>
      <Spin spinning={loading}>
        <div
          id="scrollableDiv"
          style={{
            height: 'calc(100vh - 250px)',
            overflow: 'auto',
            padding: '0 16px',
          }}
        >
          <InfiniteScroll
            dataLength={nextList?.length ?? 0}
            next={fetchNextPage}
            hasMore={hasNextPage}
            loader={<Skeleton avatar paragraph={{ rows: 1 }} active />}
            endMessage={
              !!total && <Divider plain>{t('noMoreData')} 🤐</Divider>
            }
            scrollableTarget="scrollableDiv"
            scrollThreshold="200px"
          >
            <Flex
              gap={'large'}
              wrap="wrap"
              className={styles.knowledgeCardContainer}
            >
              {nextList?.length > 0 ? (
                nextList.map((item: any, index: number) => {
                  return (
                    <KnowledgeCard
                      item={item}
                      key={`${item?.name}-${index}`}
                    ></KnowledgeCard>
                  );
                })
              ) : (
                <Empty className={styles.knowledgeEmpty}></Empty>
              )}
            </Flex>
          </InfiniteScroll>
        </div>
      </Spin>
      <KnowledgeCreatingModal
        loading={creatingLoading}
        visible={visible}
        hideModal={hideModal}
        onOk={onCreateOk}
      ></KnowledgeCreatingModal>
    </Flex>
  );
};

export default KnowledgeList;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/knowledge/index.tsx` is located in the `web/src/pages/knowledge` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge.

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


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
