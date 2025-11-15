# File Documentation: web/src/pages/knowledge/index.tsx

## File Metadata

- **Path**: `web/src/pages/knowledge/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 133
- **Characters**: 3,711
- **Size**: 3,714 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/knowledge/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 133 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `KnowledgeList()`: Function definition
- `nextList()`: Function definition
- `list()`: Function definition
- `total()`: Function definition

### Imports (11)

- `import { useInfiniteFetchKnowledgeList } from '@/hooks/knowledge-hooks';`
- `import { useFetchUserInfo } from '@/hooks/user-setting-hooks';`
- `import { PlusOutlined, SearchOutlined } from '@ant-design/icons';`
- `import {`
- `import { useTranslation } from 'react-i18next';`
- `import InfiniteScroll from 'react-infinite-scroll-component';`
- `import { useSaveKnowledge } from './hooks';`
- `import KnowledgeCard from './knowledge-card';`
- `import KnowledgeCreatingModal from './knowledge-creating-modal';`
- `import { useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 133
- Blank lines: 8 (6.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~125


## Dependencies and Imports

- `@/hooks/knowledge-hooks`
- `@/hooks/user-setting-hooks`
- `@ant-design/icons`
- `react-i18next`
- `react-infinite-scroll-component`
- `./hooks`
- `./knowledge-card`
- `./knowledge-creating-modal`
- `react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/knowledge`.

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

- Other files in `web/src/pages/knowledge/` directory
- Potential test file: `test_index.tsx`

## Keywords

./hooks, ./index.less, ./knowledge-card, ./knowledge-creating-modal, @/hooks/knowledge-hooks, @/hooks/user-setting-hooks, @ant-design/icons, Array, Button, Divider, Empty, Flex, InfiniteScroll, Input, KnowledgeCard, KnowledgeCreatingModal, KnowledgeList, PlusOutlined, SearchOutlined, Skeleton, Space, Spin, TypeScript, ant, list, nextList, react, react-i18next, react-infinite-scroll-component, total

---
*Generated by RAGFlow Repository Documentation Generator*
