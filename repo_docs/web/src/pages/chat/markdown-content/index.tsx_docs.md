# File Documentation: web/src/pages/chat/markdown-content/index.tsx

## File Metadata

- **Path**: `web/src/pages/chat/markdown-content/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 285
- **Characters**: 8,574
- **Size**: 8,574 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import Image from '@/components/image';
import SvgIcon from '@/components/svg-icon';
import { IReference, IReferenceChunk } from '@/interfaces/database/chat';
import { getExtension } from '@/utils/document-util';
import { InfoCircleOutlined } from '@ant-design/icons';
import { Button, Flex, Popover } from 'antd';
import DOMPurify from 'dompurify';
import { useCallback, useEffect, useMemo } from 'react';
import Markdown from 'react-markdown';
import reactStringReplace from 'react-string-replace';
import SyntaxHighlighter from 'react-syntax-highlighter';
import rehypeKatex from 'rehype-katex';
import rehypeRaw from 'rehype-raw';
import remarkGfm from 'remark-gfm';
import remarkMath from 'remark-math';
import { visitParents } from 'unist-util-visit-parents';

import { useFetchDocumentThumbnailsByIds } from '@/hooks/document-hooks';
import { useTranslation } from 'react-i18next';

import 'katex/dist/katex.min.css'; // `rehype-katex` does not import the CSS for you

import {
  preprocessLaTeX,
  replaceThinkToSection,
  showImage,
} from '@/utils/chat';
import { currentReg, replaceTextByOldReg } from '../utils';

import classNames from 'classnames';
import { omit } from 'lodash';
import { pipe } from 'lodash/fp';
import styles from './index.less';

const getChunkIndex = (match: string) => Number(match);
// TODO: The display of the table is inconsistent with the display previously placed in the MessageItem.
const MarkdownContent = ({
  reference,
  clickDocumentButton,
  content,
}: {
  content: string;
  loading: boolean;
  reference: IReference;
  clickDocumentButton?: (documentId: string, chunk: IReferenceChunk) => void;
}) => {
  const { t } = useTranslation();
  const { setDocumentIds, data: fileThumbnails } =
    useFetchDocumentThumbnailsByIds();
  const contentWithCursor = useMemo(() => {
    let text = DOMPurify.sanitize(content, {
      ADD_TAGS: ['think', 'section'],
      ADD_ATTR: ['class'],
    });

    // let text = content;
    if (text === '') {
      text = t('chat.searching');
    }
    const nextText = replaceTextByOldReg(text);
    return pipe(replaceThinkToSection, preprocessLaTeX)(nextText);
  }, [content, t]);

  useEffect(() => {
    const docAggs = reference?.doc_aggs;
    setDocumentIds(Array.isArray(docAggs) ? docAggs.map((x) => x.doc_id) : []);
  }, [reference, setDocumentIds]);

  const handleDocumentButtonClick = useCallback(
    (
      documentId: string,
      chunk: IReferenceChunk,
      isPdf: boolean,
      documentUrl?: string,
    ) =>
      () => {
        if (!isPdf) {
          if (!documentUrl) {
            return;
          }
          window.open(documentUrl, '_blank');
        } else {
          clickDocumentButton?.(documentId, chunk);
        }
      },
    [clickDocumentButton],
  );

  const rehypeWrapReference = () => {
    return function wrapTextTransform(tree: any) {
      visitParents(tree, 'text', (node, ancestors) => {
        const latestAncestor = ancestors.at(-1);
        if (
          latestAncestor.tagName !== 'custom-typography' &&
          latestAncestor.tagName !== 'code'
        ) {
          node.type = 'element';
          node.tagName = 'custom-typography';
          node.properties = {};
          node.children = [{ type: 'text', value: node.value }];
        }
      });
    };
  };

  const getReferenceInfo = useCallback(
    (chunkIndex: number) => {
      const chunks = reference?.chunks ?? [];
      const chunkItem = chunks[chunkIndex];
      const document = reference?.doc_aggs?.find(
        (x) => x?.doc_id === chunkItem?.document_id,
      );
      const documentId = document?.doc_id;
      const documentUrl = document?.url;
      const fileThumbnail = documentId ? fileThumbnails[documentId] : '';
      const fileExtension = documentId ? getExtension(document?.doc_name) : '';
      const imageId = chunkItem?.image_id;

      return {
        documentUrl,
        fileThumbnail,
        fileExtension,
        imageId,
        chunkItem,
        documentId,
        document,
      };
    },
    [fileThumbnails, reference],
  );

  const getPopoverContent = useCallback(
    (chunkIndex: number) => {
      const {
        documentUrl,
        fileThumbnail,
        fileExtension,
        imageId,
        chunkItem,
        documentId,
        document,
      } = getReferenceInfo(chunkIndex);

      return (
        <div key={chunkItem?.id} className="flex gap-2">
          {imageId && (
            <Popover
              placement="left"
              content={
                <Image
                  id={imageId}
                  className={styles.referenceImagePreview}
                ></Image>
              }
            >
              <Image
                id={imageId}
                className={styles.referenceChunkImage}
              ></Image>
            </Popover>
          )}
          <div className={'space-y-2 max-w-[40vw]'}>
            <div
              dangerouslySetInnerHTML={{
                __html: DOMPurify.sanitize(chunkItem?.content ?? ''),
              }}
              className={classNames(styles.chunkContentText)}
            ></div>
            {documentId && (
              <Flex gap={'small'}>
                {fileThumbnail ? (
                  <img
                    src={fileThumbnail}
                    alt=""
                    className={styles.fileThumbnail}
                  />
                ) : (
                  <SvgIcon
                    name={`file-icon/${fileExtension}`}
                    width={24}
                  ></SvgIcon>
                )}
                <Button
                  type="link"
                  className={classNames(styles.documentLink, 'text-wrap')}
                  onClick={handleDocumentButtonClick(
                    documentId,
                    chunkItem,
                    fileExtension === 'pdf',
                    documentUrl,
                  )}
                >
                  {document?.doc_name}
                </Button>
              </Flex>
            )}
          </div>
        </div>
      );
    },
    [getReferenceInfo, handleDocumentButtonClick],
  );

  const renderReference = useCallback(
    (text: string) => {
      let replacedText = reactStringReplace(text, currentReg, (match, i) => {
        const chunkIndex = getChunkIndex(match);

        const { documentUrl, fileExtension, imageId, chunkItem, documentId } =
          getReferenceInfo(chunkIndex);

        const docType = chunkItem?.doc_type;

        return showImage(docType) ? (
          <Image
            id={imageId}
            className={styles.referenceInnerChunkImage}
            onClick={
              documentId
                ? handleDocumentButtonClick(
                    documentId,
                    chunkItem,
                    fileExtension === 'pdf',
                    documentUrl,
                  )
                : () => {}
            }
          ></Image>
        ) : (
          <Popover content={getPopoverContent(chunkIndex)} key={i}>
            <InfoCircleOutlined className={styles.referenceIcon} />
          </Popover>
        );
      });

      // replacedText = reactStringReplace(replacedText, curReg, (match, i) => (
      //   <span className={styles.cursor} key={i}></span>
      // ));

      return replacedText;
    },
    [getPopoverContent, getReferenceInfo, handleDocumentButtonClick],
  );

  return (
    <Markdown
      rehypePlugins={[rehypeWrapReference, rehypeKatex, rehypeRaw]}
      remarkPlugins={[remarkGfm, remarkMath]}
      className={styles.markdownContentWrapper}
      components={
        {
          'custom-typography': ({ children }: { children: string }) =>
            renderReference(children),
          code(props: any) {
            const { children, className, ...rest } = props;
            const restProps = omit(rest, 'node');
            const match = /language-(\w+)/.exec(className || '');
            return match ? (
              <SyntaxHighlighter
                {...restProps}
                PreTag="div"
                language={match[1]}
                wrapLongLines
              >
                {String(children).replace(/\n$/, '')}
              </SyntaxHighlighter>
            ) : (
              <code
                {...restProps}
                className={classNames(className, 'text-wrap')}
              >
                {children}
              </code>
            );
          },
        } as any
      }
    >
      {contentWithCursor}
    </Markdown>
  );
};

export default MarkdownContent;

```

## High-Level Overview

// TODO: The display of the table is inconsistent with the display previously placed in the MessageItem.

## Detailed Walkthrough


### Functions (12)

- `getChunkIndex()`: Function definition
- `MarkdownContent()`: Function definition
- `contentWithCursor()`: Function definition
- `nextText()`: Function definition
- `docAggs()`: Function definition
- `handleDocumentButtonClick()`: Function definition
- `rehypeWrapReference()`: Function definition
- `wrapTextTransform()`: Function definition
- `getReferenceInfo()`: Function definition
- `document()`: Function definition
- `getPopoverContent()`: Function definition
- `renderReference()`: Function definition

### Imports (25)

- `import Image from '@/components/image';`
- `import SvgIcon from '@/components/svg-icon';`
- `import { IReference, IReferenceChunk } from '@/interfaces/database/chat';`
- `import { getExtension } from '@/utils/document-util';`
- `import { InfoCircleOutlined } from '@ant-design/icons';`
- `import { Button, Flex, Popover } from 'antd';`
- `import DOMPurify from 'dompurify';`
- `import { useCallback, useEffect, useMemo } from 'react';`
- `import Markdown from 'react-markdown';`
- `import reactStringReplace from 'react-string-replace';`

## Code Structure Analysis

- Total lines: 285
- Blank lines: 22 (7.7%)
- Comment lines: ~5 (1.8%)
- Code lines: ~258


## Dependencies and Imports

- `@/components/image`
- `@/components/svg-icon`
- `@/interfaces/database/chat`
- `@/utils/document-util`
- `@ant-design/icons`
- `antd`
- `dompurify`
- `react`
- `react-markdown`
- `react-string-replace`
- `react-syntax-highlighter`
- `rehype-katex`
- `rehype-raw`
- `remark-gfm`
- `remark-math`
- `unist-util-visit-parents`
- `@/hooks/document-hooks`
- `react-i18next`
- `../utils`
- `classnames`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/markdown-content`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal
- **Code Execution**: Avoid eval/exec with user input - potential code injection

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/chat/markdown-content/` directory
- Potential test file: `test_index.tsx`

## Keywords

../utils, ./index.less, @/components/image, @/components/svg-icon, @/hooks/document-hooks, @/interfaces/database/chat, @/utils/document-util, @ant-design/icons, ADD_ATTR, ADD_TAGS, Array, Button, CSS, DOMPurify, Flex, IReference, IReferenceChunk, Image, InfoCircleOutlined, Markdown, MarkdownContent, MessageItem, Number, Popover, PreTag, String, SvgIcon, SyntaxHighlighter, TODO, The, TypeScript, ant, antd, chunkIndex, chunkItem, chunks, classnames, contentWithCursor, docAggs, docType, document, documentId, documentUrl, dompurify, fileExtension, fileThumbnail, getChunkIndex, getPopoverContent, getReferenceInfo, handleDocumentButtonClick...

---
*Generated by RAGFlow Repository Documentation Generator*
