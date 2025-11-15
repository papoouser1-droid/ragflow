# Documentation: web/src/pages/chat/markdown-content/index.tsx

## File Metadata

- **Path**: `web/src/pages/chat/markdown-content/index.tsx`
- **Size**: 8574 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chat/markdown-content/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chat/markdown-content/index.tsx` is located in the `web/src/pages/chat/markdown-content` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to markdown-content.

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


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
