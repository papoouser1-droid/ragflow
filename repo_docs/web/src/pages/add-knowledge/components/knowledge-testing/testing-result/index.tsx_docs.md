# File Documentation: web/src/pages/add-knowledge/components/knowledge-testing/testing-result/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-testing/testing-result/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 148
- **Characters**: 4,308
- **Size**: 4,308 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```tsx
import { ReactComponent as SelectedFilesCollapseIcon } from '@/assets/svg/selected-files-collapse.svg';
import { useTranslate } from '@/hooks/common-hooks';
import { ITestingChunk, ITestingResult } from '@/interfaces/database/knowledge';
import {
  Card,
  Collapse,
  Empty,
  Flex,
  Image,
  Pagination,
  PaginationProps,
  Space,
} from 'antd';
import camelCase from 'lodash/camelCase';
import SelectFiles from './select-files';

import { useGetPaginationWithRouter } from '@/hooks/logic-hooks';
import { api_host } from '@/utils/api';
import { showImage } from '@/utils/chat';
import { useCallback } from 'react';
import styles from './index.less';

const similarityList: Array<{ field: keyof ITestingChunk; label: string }> = [
  { field: 'similarity', label: 'Hybrid Similarity' },
  { field: 'term_similarity', label: 'Term Similarity' },
  { field: 'vector_similarity', label: 'Vector Similarity' },
];

const ChunkTitle = ({ item }: { item: ITestingChunk }) => {
  const { t } = useTranslate('knowledgeDetails');
  return (
    <Flex gap={10}>
      {similarityList.map((x) => (
        <Space key={x.field}>
          <span className={styles.similarityCircle}>
            {((item[x.field] as number) * 100).toFixed(2)}
          </span>
          <span className={styles.similarityText}>{t(camelCase(x.field))}</span>
        </Space>
      ))}
    </Flex>
  );
};

interface IProps {
  handleTesting: (documentIds?: string[]) => Promise<any>;
  selectedDocumentIds: string[];
  setSelectedDocumentIds: (ids: string[]) => void;
  data?: ITestingResult;
  loading?: boolean;
}

const TestingResult = ({
  handleTesting,
  selectedDocumentIds,
  setSelectedDocumentIds,
  data,
  loading,
}: IProps) => {
  const { documents, chunks, total } = data || {};
  const { t } = useTranslate('knowledgeDetails');
  const { pagination, setPagination } = useGetPaginationWithRouter();

  const onChange: PaginationProps['onChange'] = (pageNumber, pageSize) => {
    pagination.onChange?.(pageNumber, pageSize);
    handleTesting(selectedDocumentIds);
  };

  const onTesting = useCallback(
    (ids: string[]) => {
      setPagination({ page: 1 });
      handleTesting(ids);
    },
    [setPagination, handleTesting],
  );

  return (
    <section className={styles.testingResultWrapper}>
      <Collapse
        expandIcon={() => (
          <SelectedFilesCollapseIcon></SelectedFilesCollapseIcon>
        )}
        className={styles.selectFilesCollapse}
        items={[
          {
            key: '1',
            label: (
              <Flex
                justify={'space-between'}
                align="center"
                className={styles.selectFilesTitle}
              >
                <Space>
                  <span>
                    {selectedDocumentIds?.length ?? 0}/{documents?.length ?? 0}
                  </span>
                  {t('filesSelected')}
                </Space>
              </Flex>
            ),
            children: (
              <div>
                <SelectFiles
                  setSelectedDocumentIds={setSelectedDocumentIds}
                  handleTesting={onTesting}
                ></SelectFiles>
              </div>
            ),
          },
        ]}
      />
      <Flex
        gap={'large'}
        vertical
        flex={1}
        className={styles.selectFilesCollapse}
      >
        {loading === false && chunks && chunks.length > 0 ? (
          chunks?.map((x) => (
            <Card key={x.chunk_id} title={<ChunkTitle item={x}></ChunkTitle>}>
              <div className="flex justify-center">
                {showImage(x.doc_type_kwd) && (
                  <Image
                    id={x.image_id}
                    className={'object-contain max-h-[30vh] w-full text-center'}
                    src={`${api_host}/document/image/${x.image_id}`}
                  ></Image>
                )}
              </div>
              <div className="pt-4">{x.content_with_weight}</div>
            </Card>
          ))
        ) : loading === false && chunks && chunks.length === 0 ? (
          <Empty></Empty>
        ) : null}
      </Flex>
      <Pagination
        {...pagination}
        size={'small'}
        total={total}
        onChange={onChange}
      />
    </section>
  );
};

export default TestingResult;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-testing/testing-result/index.tsx`.

Based on the file structure and naming, it appears to be a testing - contains unit tests, integration tests, or test utilities.

The file contains approximately 148 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `ChunkTitle()`: Function definition
- `TestingResult()`: Function definition
- `onTesting()`: Function definition

### Imports (11)

- `import { ReactComponent as SelectedFilesCollapseIcon } from '@/assets/svg/selected-files-collapse.svg';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { ITestingChunk, ITestingResult } from '@/interfaces/database/knowledge';`
- `import {`
- `import camelCase from 'lodash/camelCase';`
- `import SelectFiles from './select-files';`
- `import { useGetPaginationWithRouter } from '@/hooks/logic-hooks';`
- `import { api_host } from '@/utils/api';`
- `import { showImage } from '@/utils/chat';`
- `import { useCallback } from 'react';`

## Code Structure Analysis

- Total lines: 148
- Blank lines: 10 (6.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~138


## Dependencies and Imports

- `@/assets/svg/selected-files-collapse.svg`
- `@/hooks/common-hooks`
- `@/interfaces/database/knowledge`
- `lodash/camelCase`
- `./select-files`
- `@/hooks/logic-hooks`
- `@/utils/api`
- `@/utils/chat`
- `react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-testing/testing-result`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `web/src/pages/add-knowledge/components/knowledge-testing/testing-result/` directory

## Keywords

./index.less, ./select-files, @/assets/svg/selected-files-collapse.svg, @/hooks/common-hooks, @/hooks/logic-hooks, @/interfaces/database/knowledge, @/utils/api, @/utils/chat, Array, Card, ChunkTitle, Collapse, Empty, Flex, Hybrid, IProps, ITestingChunk, ITestingResult, Image, Pagination, PaginationProps, Promise, ReactComponent, SelectFiles, SelectedFilesCollapseIcon, Similarity, Space, Term, TestingResult, TypeScript, Vector, lodash/camelCase, onChange, onTesting, react, similarityList

---
*Generated by RAGFlow Repository Documentation Generator*
