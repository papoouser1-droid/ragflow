# File Documentation: web/src/components/chunk-method-modal/hooks.ts

## File Metadata

- **Path**: `web/src/components/chunk-method-modal/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 162
- **Characters**: 4,105
- **Size**: 4,105 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/chunk-method-modal/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 162 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `useFetchParserListOnMount`: Exported entity
- `useShowAutoKeywords`: Exported entity

### Functions (8)

- `getParserList()`: Function definition
- `useFetchParserListOnMount()`: Function definition
- `nextParserList()`: Function definition
- `key()`: Function definition
- `values()`: Function definition
- `handleChange()`: Function definition
- `useShowAutoKeywords()`: Function definition
- `showAutoKeywords()`: Function definition

### Imports (5)

- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useHandleChunkMethodSelectChange } from '@/hooks/logic-hooks';`
- `import { useSelectParserList } from '@/hooks/user-setting-hooks';`
- `import { FormInstance } from 'antd';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 162
- Blank lines: 12 (7.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~150


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/logic-hooks`
- `@/hooks/user-setting-hooks`
- `antd`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/chunk-method-modal`.

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

- Other files in `web/src/components/chunk-method-modal/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/constants/knowledge, @/hooks/logic-hooks, @/hooks/user-setting-hooks, Array, Book, DocumentParserType, Email, FormInstance, KnowledgeGraph, Laws, Manual, Map, Naive, One, Paper, ParserListMap, Picture, Presentation, Resume, Table, Tag, TypeScript, antd, getParserList, handleChange, handleChunkMethodSelectChange, hideAutoKeywords, key, nextParserList, parserList, react, showAutoKeywords, useFetchParserListOnMount, useShowAutoKeywords, values

---
*Generated by RAGFlow Repository Documentation Generator*
