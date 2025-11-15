# File Documentation: web/src/components/parse-configuration/graph-rag-items.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/graph-rag-items.tsx`
- **Extension**: `.tsx`
- **Lines**: 139
- **Characters**: 4,074
- **Size**: 4,074 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { DocumentParserType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import { Form, Select, Switch } from 'antd';
import { upperFirst } from 'lodash';
import { useCallback, useMemo } from 'react';
import { DatasetConfigurationContainer } from '../dataset-configuration-container';
import EntityTypesItem from '../entity-types-item';

const excludedTagParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const showTagItems = (parserId: DocumentParserType) => {
  return !excludedTagParseMethods.includes(parserId);
};

const enum MethodValue {
  General = 'general',
  Light = 'light',
}

export const excludedParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.Picture,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Qa,
  DocumentParserType.Tag,
];

export const showGraphRagItems = (parserId: DocumentParserType | undefined) => {
  return !excludedParseMethods.some((x) => x === parserId);
};

type GraphRagItemsProps = {
  marginBottom?: boolean;
};

export function UseGraphRagItem() {
  const { t } = useTranslate('knowledgeConfiguration');

  return (
    <Form.Item
      name={['parser_config', 'graphrag', 'use_graphrag']}
      label={t('useGraphRag')}
      initialValue={false}
      valuePropName="checked"
      tooltip={t('useGraphRagTip')}
    >
      <Switch />
    </Form.Item>
  );
}

// The three types "table", "resume" and "one" do not display this configuration.
const GraphRagItems = ({ marginBottom = false }: GraphRagItemsProps) => {
  const { t } = useTranslate('knowledgeConfiguration');

  const methodOptions = useMemo(() => {
    return [MethodValue.Light, MethodValue.General].map((x) => ({
      value: x,
      label: upperFirst(x),
    }));
  }, []);

  const renderWideTooltip = useCallback(
    (title: React.ReactNode | string) => {
      return {
        title: typeof title === 'string' ? t(title) : title,
        overlayInnerStyle: { width: '32vw' },
      };
    },
    [t],
  );

  return (
    <DatasetConfigurationContainer className={cn({ 'mb-4': marginBottom })}>
      <UseGraphRagItem></UseGraphRagItem>
      <Form.Item
        shouldUpdate={(prevValues, curValues) =>
          prevValues.parser_config.graphrag.use_graphrag !==
          curValues.parser_config.graphrag.use_graphrag
        }
      >
        {({ getFieldValue }) => {
          const useRaptor = getFieldValue([
            'parser_config',
            'graphrag',
            'use_graphrag',
          ]);

          return (
            useRaptor && (
              <>
                <EntityTypesItem
                  field={['parser_config', 'graphrag', 'entity_types']}
                ></EntityTypesItem>
                <Form.Item
                  name={['parser_config', 'graphrag', 'method']}
                  label={t('graphRagMethod')}
                  tooltip={renderWideTooltip(
                    <div
                      dangerouslySetInnerHTML={{
                        __html: t('graphRagMethodTip'),
                      }}
                    ></div>,
                  )}
                  initialValue={MethodValue.Light}
                >
                  <Select options={methodOptions} />
                </Form.Item>
                <Form.Item
                  name={['parser_config', 'graphrag', 'resolution']}
                  label={t('resolution')}
                  tooltip={renderWideTooltip('resolutionTip')}
                >
                  <Switch />
                </Form.Item>
                <Form.Item
                  name={['parser_config', 'graphrag', 'community']}
                  label={t('community')}
                  tooltip={renderWideTooltip('communityTip')}
                >
                  <Switch />
                </Form.Item>
              </>
            )
          );
        }}
      </Form.Item>
    </DatasetConfigurationContainer>
  );
};

export default GraphRagItems;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/parse-configuration/graph-rag-items.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 139 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `showTagItems`: Exported entity
- `excludedParseMethods`: Exported entity
- `showGraphRagItems`: Exported entity
- `UseGraphRagItem`: Exported entity

### Functions (6)

- `showTagItems()`: Function definition
- `showGraphRagItems()`: Function definition
- `UseGraphRagItem()`: Function definition
- `GraphRagItems()`: Function definition
- `methodOptions()`: Function definition
- `renderWideTooltip()`: Function definition

### Imports (8)

- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { cn } from '@/lib/utils';`
- `import { Form, Select, Switch } from 'antd';`
- `import { upperFirst } from 'lodash';`
- `import { useCallback, useMemo } from 'react';`
- `import { DatasetConfigurationContainer } from '../dataset-configuration-container';`
- `import EntityTypesItem from '../entity-types-item';`

## Code Structure Analysis

- Total lines: 139
- Blank lines: 15 (10.8%)
- Comment lines: ~1 (0.7%)
- Code lines: ~123


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/lib/utils`
- `antd`
- `lodash`
- `react`
- `../dataset-configuration-container`
- `../entity-types-item`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/parse-configuration`.

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

- Other files in `web/src/components/parse-configuration/` directory
- Potential test file: `test_graph-rag-items.tsx`

## Keywords

../dataset-configuration-container, ../entity-types-item, @/constants/knowledge, @/hooks/common-hooks, @/lib/utils, DatasetConfigurationContainer, DocumentParserType, EntityTypesItem, Form, General, GraphRagItems, GraphRagItemsProps, Item, KnowledgeGraph, Light, MethodValue, Picture, React, ReactNode, Resume, Select, Switch, Table, Tag, The, TypeScript, UseGraphRagItem, antd, enum, excludedParseMethods, excludedTagParseMethods, lodash, methodOptions, react, renderWideTooltip, showGraphRagItems, showTagItems, useRaptor

---
*Generated by RAGFlow Repository Documentation Generator*
