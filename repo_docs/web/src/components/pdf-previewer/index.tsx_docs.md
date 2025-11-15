# File Documentation: web/src/components/pdf-previewer/index.tsx

## File Metadata

- **Path**: `web/src/components/pdf-previewer/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 136
- **Characters**: 3,848
- **Size**: 3,848 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  useGetChunkHighlights,
  useGetDocumentUrl,
} from '@/hooks/document-hooks';
import { IReferenceChunk } from '@/interfaces/database/chat';
import { IChunk } from '@/interfaces/database/knowledge';
import FileError from '@/pages/document-viewer/file-error';
import { Skeleton } from 'antd';
import { useEffect, useRef, useState } from 'react';
import {
  AreaHighlight,
  Highlight,
  IHighlight,
  PdfHighlighter,
  PdfLoader,
  Popup,
} from 'react-pdf-highlighter';
import { useCatchDocumentError } from './hooks';

import styles from './index.less';

interface IProps {
  chunk: IChunk | IReferenceChunk;
  documentId: string;
  visible: boolean;
}

const HighlightPopup = ({
  comment,
}: {
  comment: { text: string; emoji: string };
}) =>
  comment.text ? (
    <div className="Highlight__popup">
      {comment.emoji} {comment.text}
    </div>
  ) : null;

const DocumentPreviewer = ({ chunk, documentId, visible }: IProps) => {
  const getDocumentUrl = useGetDocumentUrl(documentId);
  const { highlights: state, setWidthAndHeight } = useGetChunkHighlights(chunk);
  const ref = useRef<(highlight: IHighlight) => void>(() => {});
  const [loaded, setLoaded] = useState(false);
  const url = getDocumentUrl();
  const error = useCatchDocumentError(url);

  const resetHash = () => {};

  useEffect(() => {
    setLoaded(visible);
  }, [visible]);

  useEffect(() => {
    if (state.length > 0 && loaded) {
      setLoaded(false);
      ref.current(state[0]);
    }
  }, [state, loaded]);

  return (
    <div className={styles.documentContainer}>
      <PdfLoader
        url={url}
        beforeLoad={<Skeleton active />}
        workerSrc="/pdfjs-dist/pdf.worker.min.js"
        errorMessage={<FileError>{error}</FileError>}
      >
        {(pdfDocument) => {
          pdfDocument.getPage(1).then((page) => {
            const viewport = page.getViewport({ scale: 1 });
            const width = viewport.width;
            const height = viewport.height;
            setWidthAndHeight(width, height);
          });

          return (
            <PdfHighlighter
              pdfDocument={pdfDocument}
              enableAreaSelection={(event) => event.altKey}
              onScrollChange={resetHash}
              scrollRef={(scrollTo) => {
                ref.current = scrollTo;
                setLoaded(true);
              }}
              onSelectionFinished={() => null}
              highlightTransform={(
                highlight,
                index,
                setTip,
                hideTip,
                viewportToScaled,
                screenshot,
                isScrolledTo,
              ) => {
                const isTextHighlight = !Boolean(
                  highlight.content && highlight.content.image,
                );

                const component = isTextHighlight ? (
                  <Highlight
                    isScrolledTo={isScrolledTo}
                    position={highlight.position}
                    comment={highlight.comment}
                  />
                ) : (
                  <AreaHighlight
                    isScrolledTo={isScrolledTo}
                    highlight={highlight}
                    onChange={() => {}}
                  />
                );

                return (
                  <Popup
                    popupContent={<HighlightPopup {...highlight} />}
                    onMouseOver={(popupContent) =>
                      setTip(highlight, () => popupContent)
                    }
                    onMouseOut={hideTip}
                    key={index}
                  >
                    {component}
                  </Popup>
                );
              }}
              highlights={state}
            />
          );
        }}
      </PdfLoader>
    </div>
  );
};

export default DocumentPreviewer;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/pdf-previewer/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 136 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `HighlightPopup()`: Function definition
- `DocumentPreviewer()`: Function definition
- `ref()`: Function definition
- `resetHash()`: Function definition

### Imports (9)

- `import {`
- `import { IReferenceChunk } from '@/interfaces/database/chat';`
- `import { IChunk } from '@/interfaces/database/knowledge';`
- `import FileError from '@/pages/document-viewer/file-error';`
- `import { Skeleton } from 'antd';`
- `import { useEffect, useRef, useState } from 'react';`
- `import {`
- `import { useCatchDocumentError } from './hooks';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 136
- Blank lines: 13 (9.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~123


## Dependencies and Imports

- `@/interfaces/database/chat`
- `@/interfaces/database/knowledge`
- `@/pages/document-viewer/file-error`
- `antd`
- `react`
- `./hooks`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/pdf-previewer`.

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

- Other files in `web/src/components/pdf-previewer/` directory
- Potential test file: `test_index.tsx`

## Keywords

./hooks, ./index.less, @/interfaces/database/chat, @/interfaces/database/knowledge, @/pages/document-viewer/file-error, AreaHighlight, Boolean, DocumentPreviewer, FileError, Highlight, HighlightPopup, Highlight__popup, IChunk, IHighlight, IProps, IReferenceChunk, PdfHighlighter, PdfLoader, Popup, Skeleton, TypeScript, antd, component, error, getDocumentUrl, height, isTextHighlight, react, ref, resetHash, url, viewport, width

---
*Generated by RAGFlow Repository Documentation Generator*
