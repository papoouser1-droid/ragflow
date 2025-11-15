# File Documentation: web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx

## File Metadata

- **Path**: `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx`
- **Extension**: `.tsx`
- **Lines**: 128
- **Characters**: 3,593
- **Size**: 3,593 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { memo, useEffect, useRef } from 'react';
import {
  AreaHighlight,
  Highlight,
  IHighlight,
  PdfHighlighter,
  PdfLoader,
  Popup,
} from 'react-pdf-highlighter';

import { useCatchDocumentError } from '@/components/pdf-previewer/hooks';
import { Spin } from '@/components/ui/spin';
import FileError from '@/pages/document-viewer/file-error';
import styles from './index.less';

export interface IProps {
  highlights: IHighlight[];
  setWidthAndHeight: (width: number, height: number) => void;
  url: string;
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

// TODO: merge with DocumentPreviewer
const PdfPreview = ({ highlights: state, setWidthAndHeight, url }: IProps) => {
  // const url = useGetDocumentUrl();

  const ref = useRef<(highlight: IHighlight) => void>(() => {});
  const error = useCatchDocumentError(url);

  const resetHash = () => {};

  useEffect(() => {
    if (state.length > 0) {
      ref?.current(state[0]);
    }
  }, [state]);

  return (
    <div
      className={`${styles.documentContainer} rounded-[10px] overflow-hidden	`}
    >
      <PdfLoader
        url={url}
        beforeLoad={
          <div className="absolute inset-0 flex items-center justify-center">
            <Spin />
          </div>
        }
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

export default memo(PdfPreview);

```

## High-Level Overview

// TODO: merge with DocumentPreviewer
  // const url = useGetDocumentUrl();

## Detailed Walkthrough


### Functions (4)

- `HighlightPopup()`: Function definition
- `PdfPreview()`: Function definition
- `ref()`: Function definition
- `resetHash()`: Function definition

### Imports (6)

- `import { memo, useEffect, useRef } from 'react';`
- `import {`
- `import { useCatchDocumentError } from '@/components/pdf-previewer/hooks';`
- `import { Spin } from '@/components/ui/spin';`
- `import FileError from '@/pages/document-viewer/file-error';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 12 (9.4%)
- Comment lines: ~2 (1.6%)
- Code lines: ~114


## Dependencies and Imports

- `react`
- `@/components/pdf-previewer/hooks`
- `@/components/ui/spin`
- `@/pages/document-viewer/file-error`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview`.

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

- Other files in `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/` directory
- Potential test file: `test_pdf-preview.tsx`

## Keywords

./index.less, @/components/pdf-previewer/hooks, @/components/ui/spin, @/pages/document-viewer/file-error, AreaHighlight, Boolean, DocumentPreviewer, FileError, Highlight, HighlightPopup, Highlight__popup, IHighlight, IProps, PdfHighlighter, PdfLoader, PdfPreview, Popup, Spin, TODO, TypeScript, component, error, height, isTextHighlight, react, ref, resetHash, url, viewport, width

---
*Generated by RAGFlow Repository Documentation Generator*
