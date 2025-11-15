# Documentation: web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx`
- **Size**: 3496 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-testing/testing-control/index.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-testing/testing-control` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to testing-control.

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
- [label-word-cloud.tsx](label-word-cloud.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
