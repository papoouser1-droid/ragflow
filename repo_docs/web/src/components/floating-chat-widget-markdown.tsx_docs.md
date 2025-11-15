# File Documentation: web/src/components/floating-chat-widget-markdown.tsx

## File Metadata

- **Path**: `web/src/components/floating-chat-widget-markdown.tsx`
- **Extension**: `.tsx`
- **Lines**: 318
- **Characters**: 10,176
- **Size**: 10,176 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import Image from '@/components/image';
import SvgIcon from '@/components/svg-icon';
import {
  useFetchDocumentThumbnailsByIds,
  useGetDocumentUrl,
} from '@/hooks/document-hooks';
import { IReference, IReferenceChunk } from '@/interfaces/database/chat';
import {
  preprocessLaTeX,
  replaceThinkToSection,
  showImage,
} from '@/utils/chat';
import { getExtension } from '@/utils/document-util';
import { InfoCircleOutlined } from '@ant-design/icons';
import { Button, Flex, Popover, Tooltip } from 'antd';
import classNames from 'classnames';
import DOMPurify from 'dompurify';
import 'katex/dist/katex.min.css';
import { omit } from 'lodash';
import { pipe } from 'lodash/fp';
import { useCallback, useEffect, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import Markdown from 'react-markdown';
import reactStringReplace from 'react-string-replace';
import SyntaxHighlighter from 'react-syntax-highlighter';
import {
  oneDark,
  oneLight,
} from 'react-syntax-highlighter/dist/esm/styles/prism';
import rehypeKatex from 'rehype-katex';
import rehypeRaw from 'rehype-raw';
import remarkGfm from 'remark-gfm';
import remarkMath from 'remark-math';
import { visitParents } from 'unist-util-visit-parents';
import { currentReg, replaceTextByOldReg } from '../pages/next-chats/utils';
import styles from './floating-chat-widget-markdown.less';
import { useIsDarkTheme } from './theme-provider';

const getChunkIndex = (match: string) => Number(match.replace(/\[|\]/g, ''));

const FloatingChatWidgetMarkdown = ({
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
  const getDocumentUrl = useGetDocumentUrl();
  const isDarkTheme = useIsDarkTheme();

  const contentWithCursor = useMemo(() => {
    let text = content === '' ? t('chat.searching') : content;
    const nextText = replaceTextByOldReg(text);
    return pipe(replaceThinkToSection, preprocessLaTeX)(nextText);
  }, [content, t]);

  useEffect(() => {
    const docAggs = reference?.doc_aggs;
    const docList = Array.isArray(docAggs)
      ? docAggs
      : Object.values(docAggs ?? {});
    setDocumentIds(docList.map((x: any) => x.doc_id).filter(Boolean));
  }, [reference, setDocumentIds]);

  const handleDocumentButtonClick = useCallback(
    (
      documentId: string,
      chunk: IReferenceChunk,
      isPdf: boolean,
      documentUrl?: string,
    ) =>
      () => {
        if (!documentId) return;
        if (!isPdf && documentUrl) {
          window.open(documentUrl, '_blank');
        } else if (clickDocumentButton) {
          clickDocumentButton(documentId, chunk);
        }
      },
    [clickDocumentButton],
  );

  const rehypeWrapReference = () => (tree: any) => {
    visitParents(tree, 'text', (node, ancestors) => {
      const latestAncestor = ancestors[ancestors.length - 1];
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

  const getReferenceInfo = useCallback(
    (chunkIndex: number) => {
      const chunkItem = reference?.chunks?.[chunkIndex];
      if (!chunkItem) return null;
      const docAggsArray = Array.isArray(reference?.doc_aggs)
        ? reference.doc_aggs
        : Object.values(reference?.doc_aggs ?? {});
      const document = docAggsArray.find(
        (x: any) => x?.doc_id === chunkItem?.document_id,
      ) as any;
      const documentId = document?.doc_id;
      const documentUrl =
        document?.url ?? (documentId ? getDocumentUrl(documentId) : undefined);
      const fileThumbnail = documentId ? fileThumbnails[documentId] : '';
      const fileExtension = documentId
        ? getExtension(document?.doc_name ?? '')
        : '';
      return {
        documentUrl,
        fileThumbnail,
        fileExtension,
        imageId: chunkItem.image_id,
        chunkItem,
        documentId,
        document,
      };
    },
    [fileThumbnails, reference, getDocumentUrl],
  );

  const getPopoverContent = useCallback(
    (chunkIndex: number) => {
      const info = getReferenceInfo(chunkIndex);

      if (!info) {
        return (
          <div className="p-2 text-xs text-red-500">
            Error: Missing document information.
          </div>
        );
      }

      const {
        documentUrl,
        fileThumbnail,
        fileExtension,
        imageId,
        chunkItem,
        documentId,
        document,
      } = info;

      return (
        <div
          key={`popover-content-${chunkItem.id}`}
          className="flex gap-2 widget-citation-content"
        >
          {imageId && (
            <Popover
              placement="left"
              content={
                <Image
                  id={imageId}
                  className="max-w-[80vw] max-h-[60vh] rounded"
                />
              }
            >
              <Image
                id={imageId}
                className="w-24 h-24 object-contain rounded m-1 cursor-pointer"
              />
            </Popover>
          )}
          <div className="space-y-2 flex-1 min-w-0">
            <div
              dangerouslySetInnerHTML={{
                __html: DOMPurify.sanitize(chunkItem?.content ?? ''),
              }}
              className="max-h-[250px] overflow-y-auto text-xs leading-relaxed p-2 bg-gray-50 dark:bg-gray-800 rounded prose-sm"
            ></div>
            {documentId && (
              <Flex gap={'small'} align="center">
                {fileThumbnail ? (
                  <img
                    src={fileThumbnail}
                    alt={document?.doc_name}
                    className="w-6 h-6 rounded"
                  />
                ) : (
                  <SvgIcon name={`file-icon/${fileExtension}`} width={20} />
                )}
                <Tooltip
                  title={
                    !documentUrl && fileExtension !== 'pdf'
                      ? 'Document link unavailable'
                      : document.doc_name
                  }
                >
                  <Button
                    type="link"
                    size="small"
                    className="p-0 text-xs break-words h-auto text-left flex-1"
                    onClick={handleDocumentButtonClick(
                      documentId,
                      chunkItem,
                      fileExtension === 'pdf',
                      documentUrl,
                    )}
                    disabled={!documentUrl && fileExtension !== 'pdf'}
                    style={{ whiteSpace: 'normal' }}
                  >
                    <span className="truncate">
                      {document?.doc_name ?? 'Unnamed Document'}
                    </span>
                  </Button>
                </Tooltip>
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
      return reactStringReplace(text, currentReg, (match, i) => {
        const chunkIndex = getChunkIndex(match);
        const info = getReferenceInfo(chunkIndex);

        if (!info) {
          return (
            <Tooltip key={`err-tooltip-${i}`} title="Reference unavailable">
              <InfoCircleOutlined className={styles.referenceIcon} />
            </Tooltip>
          );
        }

        const { imageId, chunkItem, documentId, fileExtension, documentUrl } =
          info;

        if (showImage(chunkItem?.doc_type)) {
          return (
            <Image
              key={`img-${i}`}
              id={imageId}
              className="block object-contain max-w-full max-h-48 rounded my-2 cursor-pointer"
              onClick={handleDocumentButtonClick(
                documentId,
                chunkItem,
                fileExtension === 'pdf',
                documentUrl,
              )}
            />
          );
        }

        return (
          <Popover content={getPopoverContent(chunkIndex)} key={`popover-${i}`}>
            <InfoCircleOutlined className={styles.referenceIcon} />
          </Popover>
        );
      });
    },
    [getPopoverContent, getReferenceInfo, handleDocumentButtonClick],
  );

  return (
    <div className="floating-chat-widget">
      <Markdown
        rehypePlugins={[rehypeWrapReference, rehypeKatex, rehypeRaw]}
        remarkPlugins={[remarkGfm, remarkMath]}
        className="text-sm leading-relaxed space-y-2 prose-sm max-w-full"
        components={
          {
            'custom-typography': ({ children }: { children: string }) =>
              renderReference(children),
            code(props: any) {
              // eslint-disable-next-line @typescript-eslint/no-unused-vars
              const { children, className, node, ...rest } = props;
              const match = /language-(\w+)/.exec(className || '');
              return match ? (
                <SyntaxHighlighter
                  {...omit(rest, 'inline')}
                  PreTag="div"
                  language={match[1]}
                  style={isDarkTheme ? oneDark : oneLight}
                  wrapLongLines
                >
                  {String(children).replace(/\n$/, '')}
                </SyntaxHighlighter>
              ) : (
                <code
                  {...rest}
                  className={classNames(
                    className,
                    'text-wrap text-xs bg-gray-200 dark:bg-gray-700 px-1 py-0.5 rounded',
                  )}
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
    </div>
  );
};

export default FloatingChatWidgetMarkdown;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/floating-chat-widget-markdown.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 318 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (11)

- `getChunkIndex()`: Function definition
- `FloatingChatWidgetMarkdown()`: Function definition
- `contentWithCursor()`: Function definition
- `nextText()`: Function definition
- `docList()`: Function definition
- `handleDocumentButtonClick()`: Function definition
- `rehypeWrapReference()`: Function definition
- `getReferenceInfo()`: Function definition
- `document()`: Function definition
- `getPopoverContent()`: Function definition
- `renderReference()`: Function definition

### Imports (27)

- `import Image from '@/components/image';`
- `import SvgIcon from '@/components/svg-icon';`
- `import {`
- `import { IReference, IReferenceChunk } from '@/interfaces/database/chat';`
- `import {`
- `import { getExtension } from '@/utils/document-util';`
- `import { InfoCircleOutlined } from '@ant-design/icons';`
- `import { Button, Flex, Popover, Tooltip } from 'antd';`
- `import classNames from 'classnames';`
- `import DOMPurify from 'dompurify';`

## Code Structure Analysis

- Total lines: 318
- Blank lines: 19 (6.0%)
- Comment lines: ~1 (0.3%)
- Code lines: ~298


## Dependencies and Imports

- `@/components/image`
- `@/components/svg-icon`
- `@/interfaces/database/chat`
- `@/utils/document-util`
- `@ant-design/icons`
- `antd`
- `classnames`
- `dompurify`
- `lodash`
- `lodash/fp`
- `react`
- `react-i18next`
- `react-markdown`
- `react-string-replace`
- `react-syntax-highlighter`
- `rehype-katex`
- `rehype-raw`
- `remark-gfm`
- `remark-math`
- `unist-util-visit-parents`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

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

- Other files in `web/src/components/` directory
- Potential test file: `test_floating-chat-widget-markdown.tsx`

## Keywords

../pages/next-chats/utils, ./floating-chat-widget-markdown.less, ./theme-provider, @/components/image, @/components/svg-icon, @/interfaces/database/chat, @/utils/document-util, @ant-design/icons, Array, Boolean, Button, DOMPurify, Document, Error, Flex, FloatingChatWidgetMarkdown, IReference, IReferenceChunk, Image, InfoCircleOutlined, Markdown, Missing, Number, Object, Popover, PreTag, Reference, String, SvgIcon, SyntaxHighlighter, Tooltip, TypeScript, Unnamed, ant, antd, chunkIndex, chunkItem, classnames, contentWithCursor, docAggs, docAggsArray, docList, document, documentId, documentUrl, dompurify, fileExtension, fileThumbnail, getChunkIndex, getDocumentUrl...

---
*Generated by RAGFlow Repository Documentation Generator*
