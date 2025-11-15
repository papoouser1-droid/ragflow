# Documentation: web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx

## File Metadata

- **Path**: `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx`
- **Size**: 3593 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview/pdf-preview.tsx` is located in the `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/document-preview` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to document-preview.

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

- [csv-preview.tsx](csv-preview.tsx_docs.md)
- [doc-preview.tsx](doc-preview.tsx_docs.md)
- [document-header.tsx](document-header.tsx_docs.md)
- [excel-preview.tsx](excel-preview.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [image-preview.tsx](image-preview.tsx_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [ppt-preview.tsx](ppt-preview.tsx_docs.md)
- [txt-preview.tsx](txt-preview.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
