# Documentation: web/src/components/parse-configuration/graph-rag-items.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/graph-rag-items.tsx`
- **Size**: 4074 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/parse-configuration/graph-rag-items.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/parse-configuration/graph-rag-items.tsx` is located in the `web/src/components/parse-configuration` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parse-configuration.

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

- [graph-rag-form-fields.tsx](graph-rag-form-fields.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [raptor-form-fields-old.tsx](raptor-form-fields-old.tsx_docs.md)
- [raptor-form-fields.tsx](raptor-form-fields.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
