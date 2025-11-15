# Documentation: web/src/components/chunk-method-modal/hooks.ts

## File Metadata

- **Path**: `web/src/components/chunk-method-modal/hooks.ts`
- **Size**: 4105 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/chunk-method-modal/hooks.ts`.

## Original Source Code

```ts
import { DocumentParserType } from '@/constants/knowledge';
import { useHandleChunkMethodSelectChange } from '@/hooks/logic-hooks';
import { useSelectParserList } from '@/hooks/user-setting-hooks';
import { FormInstance } from 'antd';
import { useCallback, useEffect, useMemo, useState } from 'react';

const ParserListMap = new Map([
  [
    ['pdf'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Resume,
      DocumentParserType.Manual,
      DocumentParserType.Paper,
      DocumentParserType.Book,
      DocumentParserType.Laws,
      DocumentParserType.Presentation,
      DocumentParserType.One,
      DocumentParserType.Qa,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [
    ['doc', 'docx'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Resume,
      DocumentParserType.Book,
      DocumentParserType.Laws,
      DocumentParserType.One,
      DocumentParserType.Qa,
      DocumentParserType.Manual,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [
    ['xlsx', 'xls'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Qa,
      DocumentParserType.Table,
      DocumentParserType.One,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [['ppt', 'pptx'], [DocumentParserType.Presentation]],
  [
    ['jpg', 'jpeg', 'png', 'gif', 'bmp', 'tif', 'tiff', 'webp', 'svg', 'ico'],
    [DocumentParserType.Picture],
  ],
  [
    ['txt'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Resume,
      DocumentParserType.Book,
      DocumentParserType.Laws,
      DocumentParserType.One,
      DocumentParserType.Qa,
      DocumentParserType.Table,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [
    ['csv'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Resume,
      DocumentParserType.Book,
      DocumentParserType.Laws,
      DocumentParserType.One,
      DocumentParserType.Qa,
      DocumentParserType.Table,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [
    ['md'],
    [
      DocumentParserType.Naive,
      DocumentParserType.Qa,
      DocumentParserType.KnowledgeGraph,
    ],
  ],
  [['json'], [DocumentParserType.Naive, DocumentParserType.KnowledgeGraph]],
  [['eml'], [DocumentParserType.Email]],
]);

const getParserList = (
  values: string[],
  parserList: Array<{
    value: string;
    label: string;
  }>,
) => {
  return parserList.filter((x) => values?.some((y) => y === x.value));
};

export const useFetchParserListOnMount = (
  documentId: string,
  parserId: DocumentParserType,
  documentExtension: string,
  form: FormInstance,
) => {
  const [selectedTag, setSelectedTag] = useState<DocumentParserType>();
  const parserList = useSelectParserList();
  const handleChunkMethodSelectChange = useHandleChunkMethodSelectChange(form);

  const nextParserList = useMemo(() => {
    const key = [...ParserListMap.keys()].find((x) =>
      x.some((y) => y === documentExtension),
    );
    if (key) {
      const values = ParserListMap.get(key);
      return getParserList(values ?? [], parserList);
    }

    return getParserList(
      [
        DocumentParserType.Naive,
        DocumentParserType.Resume,
        DocumentParserType.Book,
        DocumentParserType.Laws,
        DocumentParserType.One,
        DocumentParserType.Qa,
        DocumentParserType.Table,
      ],
      parserList,
    );
  }, [parserList, documentExtension]);

  useEffect(() => {
    setSelectedTag(parserId);
  }, [parserId, documentId]);

  const handleChange = (tag: string) => {
    handleChunkMethodSelectChange(tag);
    setSelectedTag(tag as DocumentParserType);
  };

  return { parserList: nextParserList, handleChange, selectedTag };
};

const hideAutoKeywords = [
  DocumentParserType.Qa,
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const useShowAutoKeywords = () => {
  const showAutoKeywords = useCallback(
    (selectedTag: DocumentParserType | undefined) => {
      return hideAutoKeywords.every((x) => selectedTag !== x);
    },
    [],
  );

  return showAutoKeywords;
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/chunk-method-modal/hooks.ts` is located in the `web/src/components/chunk-method-modal` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-method-modal.

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
- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
