# File Documentation: web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 110
- **Characters**: 3,496
- **Size**: 3,496 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```tsx
import Rerank from '@/components/rerank';
import SimilaritySlider from '@/components/similarity-slider';
import { useTranslate } from '@/hooks/common-hooks';
import { useChunkIsTesting } from '@/hooks/knowledge-hooks';
import { Button, Card, Divider, Flex, Form, Input } from 'antd';
import { FormInstance } from 'antd/lib';
import { LabelWordCloud } from './label-word-cloud';

import { CrossLanguageItem } from '@/components/cross-language-item';
import { UseKnowledgeGraphItem } from '@/components/use-knowledge-graph-item';
import styles from './index.less';

type FieldType = {
  similarity_threshold?: number;
  vector_similarity_weight?: number;
  question: string;
};

interface IProps {
  form: FormInstance;
  handleTesting: (documentIds?: string[]) => Promise<any>;
  selectedDocumentIds: string[];
}

const TestingControl = ({
  form,
  handleTesting,
  selectedDocumentIds,
}: IProps) => {
  const question = Form.useWatch('question', { form, preserve: true });
  const loading = useChunkIsTesting();
  const { t } = useTranslate('knowledgeDetails');

  const buttonDisabled =
    !question || (typeof question === 'string' && question.trim() === '');

  const onClick = () => {
    handleTesting(selectedDocumentIds);
  };

  return (
    <section className={styles.testingControlWrapper}>
      <div>
        <b>{t('testing')}</b>
      </div>
      <p>{t('testingDescription')}</p>
      <Divider></Divider>
      <section>
        <Form name="testing" layout="vertical" form={form}>
          <SimilaritySlider isTooltipShown></SimilaritySlider>
          <Rerank></Rerank>
          <UseKnowledgeGraphItem filedName={['use_kg']}></UseKnowledgeGraphItem>
          <CrossLanguageItem name={'cross_languages'}></CrossLanguageItem>
          <Card size="small" title={t('testText')}>
            <Form.Item<FieldType>
              name={'question'}
              rules={[{ required: true, message: t('testTextPlaceholder') }]}
            >
              <Input.TextArea autoSize={{ minRows: 8 }}></Input.TextArea>
            </Form.Item>
            <Flex justify={'end'}>
              <Button
                type="primary"
                size="small"
                onClick={onClick}
                disabled={buttonDisabled}
                loading={loading}
              >
                {t('testingLabel')}
              </Button>
            </Flex>
          </Card>
        </Form>
      </section>
      <LabelWordCloud></LabelWordCloud>
      {/* <section>
        <div className={styles.historyTitle}>
          <Space size={'middle'}>
            <HistoryOutlined className={styles.historyIcon} />
            <b>Test history</b>
          </Space>
        </div>
        <Space
          direction="vertical"
          size={'middle'}
          className={styles.historyCardWrapper}
        >
          {list.map((x) => (
            <Card className={styles.historyCard} key={x}>
              <Flex justify={'space-between'} gap={'small'}>
                <span>{x}</span>
                <div className={styles.historyText}>
                  content dcjsjl snldsh svnodvn svnodrfn svjdoghdtbnhdo
                  sdvhodhbuid sldghdrlh
                </div>
                <Flex gap={'small'}>
                  <span>time</span>
                  <DeleteOutlined></DeleteOutlined>
                </Flex>
              </Flex>
            </Card>
          ))}
        </Space>
      </section> */}
    </section>
  );
};

export default TestingControl;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx`.

Based on the file structure and naming, it appears to be a testing - contains unit tests, integration tests, or test utilities.

The file contains approximately 110 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `TestingControl()`: Function definition
- `onClick()`: Function definition

### Imports (10)

- `import Rerank from '@/components/rerank';`
- `import SimilaritySlider from '@/components/similarity-slider';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useChunkIsTesting } from '@/hooks/knowledge-hooks';`
- `import { Button, Card, Divider, Flex, Form, Input } from 'antd';`
- `import { FormInstance } from 'antd/lib';`
- `import { LabelWordCloud } from './label-word-cloud';`
- `import { CrossLanguageItem } from '@/components/cross-language-item';`
- `import { UseKnowledgeGraphItem } from '@/components/use-knowledge-graph-item';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 110
- Blank lines: 9 (8.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~101


## Dependencies and Imports

- `@/components/rerank`
- `@/components/similarity-slider`
- `@/hooks/common-hooks`
- `@/hooks/knowledge-hooks`
- `antd`
- `antd/lib`
- `./label-word-cloud`
- `@/components/cross-language-item`
- `@/components/use-knowledge-graph-item`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-testing/testing-control`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/` directory

## Keywords

./index.less, ./label-word-cloud, @/components/cross-language-item, @/components/rerank, @/components/similarity-slider, @/components/use-knowledge-graph-item, @/hooks/common-hooks, @/hooks/knowledge-hooks, Button, Card, CrossLanguageItem, DeleteOutlined, Divider, FieldType, Flex, Form, FormInstance, HistoryOutlined, IProps, Input, Item, LabelWordCloud, Promise, Rerank, SimilaritySlider, Space, Test, TestingControl, TextArea, TypeScript, UseKnowledgeGraphItem, antd, antd/lib, buttonDisabled, loading, onClick, question

---
*Generated by RAGFlow Repository Documentation Generator*
