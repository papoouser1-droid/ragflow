# File Documentation: web/src/pages/document-viewer/hooks.ts

## File Metadata

- **Path**: `web/src/pages/document-viewer/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 110
- **Characters**: 3,093
- **Size**: 3,093 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { Authorization } from '@/constants/authorization';
import { getAuthorization } from '@/utils/authorization-util';
import jsPreviewExcel from '@js-preview/excel';
import axios from 'axios';
import mammoth from 'mammoth';
import { useCallback, useEffect, useRef, useState } from 'react';

export const useCatchError = (api: string) => {
  const [error, setError] = useState('');
  const fetchDocument = useCallback(async () => {
    const ret = await axios.get(api);
    const { data } = ret;
    if (!(data instanceof ArrayBuffer) && data.code !== 0) {
      setError(data.message);
    }
    return ret;
  }, [api]);

  useEffect(() => {
    fetchDocument();
  }, [fetchDocument]);

  return { fetchDocument, error };
};

export const useFetchDocument = () => {
  const fetchDocument = useCallback(async (api: string) => {
    const ret = await axios.get(api, {
      headers: {
        [Authorization]: getAuthorization(),
      },
      responseType: 'arraybuffer',
    });
    return ret;
  }, []);

  return { fetchDocument };
};

export const useFetchExcel = (filePath: string) => {
  const [status, setStatus] = useState(true);
  const { fetchDocument } = useFetchDocument();
  const containerRef = useRef<HTMLDivElement>(null);
  const { error } = useCatchError(filePath);

  const fetchDocumentAsync = useCallback(async () => {
    let myExcelPreviewer;
    if (containerRef.current) {
      myExcelPreviewer = jsPreviewExcel.init(containerRef.current);
    }
    const jsonFile = await fetchDocument(filePath);
    myExcelPreviewer
      ?.preview(jsonFile.data)
      .then(() => {
        console.log('succeed');
        setStatus(true);
      })
      .catch((e) => {
        console.warn('failed', e);
        myExcelPreviewer.destroy();
        setStatus(false);
      });
  }, [filePath, fetchDocument]);

  useEffect(() => {
    fetchDocumentAsync();
  }, [fetchDocumentAsync]);

  return { status, containerRef, error };
};

export const useFetchDocx = (filePath: string) => {
  const [succeed, setSucceed] = useState(true);
  const [error, setError] = useState<string>();
  const { fetchDocument } = useFetchDocument();
  const containerRef = useRef<HTMLDivElement>(null);

  const fetchDocumentAsync = useCallback(async () => {
    try {
      const jsonFile = await fetchDocument(filePath);
      mammoth
        .convertToHtml(
          { arrayBuffer: jsonFile.data },
          { includeDefaultStyleMap: true },
        )
        .then((result) => {
          setSucceed(true);
          const docEl = document.createElement('div');
          docEl.className = 'document-container';
          docEl.innerHTML = result.value;
          const container = containerRef.current;
          if (container) {
            container.innerHTML = docEl.outerHTML;
          }
        })
        .catch(() => {
          setSucceed(false);
        });
    } catch (error: any) {
      setError(error.toString());
    }
  }, [filePath, fetchDocument]);

  useEffect(() => {
    fetchDocumentAsync();
  }, [fetchDocumentAsync]);

  return { succeed, containerRef, error };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/document-viewer/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 110 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `useCatchError`: Exported entity
- `useFetchDocument`: Exported entity
- `useFetchExcel`: Exported entity
- `useFetchDocx`: Exported entity

### Functions (10)

- `useCatchError()`: Function definition
- `fetchDocument()`: Function definition
- `useFetchDocument()`: Function definition
- `fetchDocument()`: Function definition
- `useFetchExcel()`: Function definition
- `fetchDocumentAsync()`: Function definition
- `jsonFile()`: Function definition
- `useFetchDocx()`: Function definition
- `fetchDocumentAsync()`: Function definition
- `jsonFile()`: Function definition

### Imports (6)

- `import { Authorization } from '@/constants/authorization';`
- `import { getAuthorization } from '@/utils/authorization-util';`
- `import jsPreviewExcel from '@js-preview/excel';`
- `import axios from 'axios';`
- `import mammoth from 'mammoth';`
- `import { useCallback, useEffect, useRef, useState } from 'react';`

## Code Structure Analysis

- Total lines: 110
- Blank lines: 14 (12.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~96


## Dependencies and Imports

- `@/constants/authorization`
- `@/utils/authorization-util`
- `@js-preview/excel`
- `axios`
- `mammoth`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/document-viewer`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/document-viewer/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/constants/authorization, @/utils/authorization-util, @js-preview/excel, ArrayBuffer, Authorization, HTMLDivElement, TypeScript, axios, container, containerRef, docEl, fetchDocument, fetchDocumentAsync, js, jsonFile, mammoth, myExcelPreviewer, react, ret, useCatchError, useFetchDocument, useFetchDocx, useFetchExcel

---
*Generated by RAGFlow Repository Documentation Generator*
