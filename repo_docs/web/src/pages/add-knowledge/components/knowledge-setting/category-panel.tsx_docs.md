# File Documentation: web/src/pages/add-knowledge/components/knowledge-setting/category-panel.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-setting/category-panel.tsx`
- **Extension**: `.tsx`
- **Lines**: 78
- **Characters**: 2,487
- **Size**: 2,487 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import SvgIcon from '@/components/svg-icon';
import { useTranslate } from '@/hooks/common-hooks';
import { useSelectParserList } from '@/hooks/user-setting-hooks';
import { Col, Divider, Empty, Row, Typography } from 'antd';
import DOMPurify from 'dompurify';
import camelCase from 'lodash/camelCase';
import { useMemo } from 'react';
import styles from './index.less';
import { TagTabs } from './tag-tabs';
import { ImageMap } from './utils';

const { Text } = Typography;

const CategoryPanel = ({ chunkMethod }: { chunkMethod: string }) => {
  const parserList = useSelectParserList();
  const { t } = useTranslate('knowledgeConfiguration');

  const item = useMemo(() => {
    const item = parserList.find((x) => x.value === chunkMethod);
    if (item) {
      return {
        title: item.label,
        description: t(camelCase(item.value)),
      };
    }
    return { title: '', description: '' };
  }, [parserList, chunkMethod, t]);

  const imageList = useMemo(() => {
    if (chunkMethod in ImageMap) {
      return ImageMap[chunkMethod as keyof typeof ImageMap];
    }
    return [];
  }, [chunkMethod]);

  return (
    <section className={styles.categoryPanelWrapper}>
      {imageList.length > 0 ? (
        <>
          <h5 className="font-semibold text-base mt-0 mb-1">
            {`"${item.title}" ${t('methodTitle')}`}
          </h5>
          <p
            dangerouslySetInnerHTML={{
              __html: DOMPurify.sanitize(item.description),
            }}
          ></p>
          <h5 className="font-semibold text-base mt-4 mb-1">{`"${item.title}" ${t('methodExamples')}`}</h5>
          <Text>{t('methodExamplesDescription')}</Text>
          <Row gutter={[10, 10]} className={styles.imageRow}>
            {imageList.map((x) => (
              <Col span={12} key={x}>
                <SvgIcon
                  name={x}
                  width={'100%'}
                  className={styles.image}
                ></SvgIcon>
              </Col>
            ))}
          </Row>
          <h5 className="font-semibold text-base mt-4 mb-1">
            {item.title} {t('dialogueExamplesTitle')}
          </h5>
          <Divider></Divider>
        </>
      ) : (
        <Empty description={''} image={null}>
          <p>{t('methodEmpty')}</p>
          <SvgIcon name={'chunk-method/chunk-empty'} width={'100%'}></SvgIcon>
        </Empty>
      )}
      {chunkMethod === 'tag' && <TagTabs></TagTabs>}
    </section>
  );
};

export default CategoryPanel;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-setting/category-panel.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 78 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `CategoryPanel()`: Function definition
- `item()`: Function definition
- `item()`: Function definition
- `imageList()`: Function definition

### Imports (10)

- `import SvgIcon from '@/components/svg-icon';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useSelectParserList } from '@/hooks/user-setting-hooks';`
- `import { Col, Divider, Empty, Row, Typography } from 'antd';`
- `import DOMPurify from 'dompurify';`
- `import camelCase from 'lodash/camelCase';`
- `import { useMemo } from 'react';`
- `import styles from './index.less';`
- `import { TagTabs } from './tag-tabs';`
- `import { ImageMap } from './utils';`

## Code Structure Analysis

- Total lines: 78
- Blank lines: 7 (9.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~71


## Dependencies and Imports

- `@/components/svg-icon`
- `@/hooks/common-hooks`
- `@/hooks/user-setting-hooks`
- `antd`
- `dompurify`
- `lodash/camelCase`
- `react`
- `./index.less`
- `./tag-tabs`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-setting`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/add-knowledge/components/knowledge-setting/` directory
- Potential test file: `test_category-panel.tsx`

## Keywords

./index.less, ./tag-tabs, ./utils, @/components/svg-icon, @/hooks/common-hooks, @/hooks/user-setting-hooks, CategoryPanel, Col, DOMPurify, Divider, Empty, ImageMap, Row, SvgIcon, TagTabs, Text, TypeScript, Typography, antd, dompurify, imageList, item, lodash/camelCase, parserList, react

---
*Generated by RAGFlow Repository Documentation Generator*
