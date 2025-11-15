# File Documentation: web/src/components/prompt-editor/index.tsx

## File Metadata

- **Path**: `web/src/components/prompt-editor/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 161
- **Characters**: 4,546
- **Size**: 4,549 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { CodeHighlightNode, CodeNode } from '@lexical/code';
import {
  InitialConfigType,
  LexicalComposer,
} from '@lexical/react/LexicalComposer';
import { ContentEditable } from '@lexical/react/LexicalContentEditable';
import { LexicalErrorBoundary } from '@lexical/react/LexicalErrorBoundary';
import { RichTextPlugin } from '@lexical/react/LexicalRichTextPlugin';
import { HeadingNode, QuoteNode } from '@lexical/rich-text';
import {
  $getRoot,
  $getSelection,
  $nodesOfType,
  EditorState,
  Klass,
  LexicalNode,
} from 'lexical';

import { cn } from '@/lib/utils';
import { useLexicalComposerContext } from '@lexical/react/LexicalComposerContext';
import { Variable } from 'lucide-react';
import { ReactNode, useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { Tooltip, TooltipContent, TooltipTrigger } from '../ui/tooltip';
import theme from './theme';
import { VariableNode } from './variable-node';
import { VariableOnChangePlugin } from './variable-on-change-plugin';
import VariablePickerMenuPlugin from './variable-picker-plugin';

// Catch any errors that occur during Lexical updates and log them
// or throw them as needed. If you don't throw them, Lexical will
// try to recover gracefully without losing user data.
function onError(error: Error) {
  console.error(error);
}

const Nodes: Array<Klass<LexicalNode>> = [
  HeadingNode,
  QuoteNode,
  CodeHighlightNode,
  CodeNode,
  VariableNode,
];

type PromptContentProps = { showToolbar?: boolean };

type IProps = {
  value?: string;
  onChange?: (value?: string) => void;
  placeholder?: ReactNode;
} & PromptContentProps;

function PromptContent({ showToolbar = true }: PromptContentProps) {
  const [editor] = useLexicalComposerContext();
  const [isBlur, setIsBlur] = useState(false);
  const { t } = useTranslation();

  const insertTextAtCursor = useCallback(() => {
    editor.update(() => {
      const selection = $getSelection();

      if (selection !== null) {
        selection.insertText(' /');
      }
    });
  }, [editor]);

  const handleVariableIconClick = useCallback(() => {
    insertTextAtCursor();
  }, [insertTextAtCursor]);

  const handleBlur = useCallback(() => {
    setIsBlur(true);
  }, []);

  const handleFocus = useCallback(() => {
    setIsBlur(false);
  }, []);

  return (
    <section
      className={cn('border rounded-sm ', { 'border-blue-400': !isBlur })}
    >
      {showToolbar && (
        <div className="border-b px-2 py-2 justify-end flex">
          <Tooltip>
            <TooltipTrigger asChild>
              <span className="inline-block cursor-pointer cursor p-0.5 hover:bg-gray-100 dark:hover:bg-slate-800 rounded-sm">
                <Variable size={16} onClick={handleVariableIconClick} />
              </span>
            </TooltipTrigger>
            <TooltipContent>
              <p>{t('flow.insertVariableTip')}</p>
            </TooltipContent>
          </Tooltip>
        </div>
      )}
      <ContentEditable
        className="min-h-40 relative px-2 py-1 focus-visible:outline-none"
        onBlur={handleBlur}
        onFocus={handleFocus}
      />
    </section>
  );
}

export function PromptEditor({
  value,
  onChange,
  placeholder,
  showToolbar,
}: IProps) {
  const { t } = useTranslation();
  const initialConfig: InitialConfigType = {
    namespace: 'PromptEditor',
    theme,
    onError,
    nodes: Nodes,
  };

  const onValueChange = useCallback(
    (editorState: EditorState) => {
      editorState?.read(() => {
        const listNodes = $nodesOfType(VariableNode); // to be removed
        // const allNodes = $dfs();
        console.log('🚀 ~ onChange ~ allNodes:', listNodes);

        const text = $getRoot().getTextContent();

        onChange?.(text);
      });
    },
    [onChange],
  );

  return (
    <div className="relative">
      <LexicalComposer initialConfig={initialConfig}>
        <RichTextPlugin
          contentEditable={
            <PromptContent showToolbar={showToolbar}></PromptContent>
          }
          placeholder={
            <div
              className="absolute top-10 left-2 text-text-secondary"
              data-xxx
            >
              {placeholder || t('common.pleaseInput')}
            </div>
          }
          ErrorBoundary={LexicalErrorBoundary}
        />
        <VariablePickerMenuPlugin value={value}></VariablePickerMenuPlugin>
        <VariableOnChangePlugin
          onChange={onValueChange}
        ></VariableOnChangePlugin>
      </LexicalComposer>
    </div>
  );
}

```

## High-Level Overview

// Catch any errors that occur during Lexical updates and log them
// or throw them as needed. If you don't throw them, Lexical will
// try to recover gracefully without losing user data.

## Detailed Walkthrough

### Exports (1)

- `PromptEditor`: Exported entity

### Functions (8)

- `onError()`: Function definition
- `PromptContent()`: Function definition
- `insertTextAtCursor()`: Function definition
- `handleVariableIconClick()`: Function definition
- `handleBlur()`: Function definition
- `handleFocus()`: Function definition
- `PromptEditor()`: Function definition
- `onValueChange()`: Function definition

### Imports (17)

- `import { CodeHighlightNode, CodeNode } from '@lexical/code';`
- `import {`
- `import { ContentEditable } from '@lexical/react/LexicalContentEditable';`
- `import { LexicalErrorBoundary } from '@lexical/react/LexicalErrorBoundary';`
- `import { RichTextPlugin } from '@lexical/react/LexicalRichTextPlugin';`
- `import { HeadingNode, QuoteNode } from '@lexical/rich-text';`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { useLexicalComposerContext } from '@lexical/react/LexicalComposerContext';`
- `import { Variable } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 161
- Blank lines: 18 (11.2%)
- Comment lines: ~4 (2.5%)
- Code lines: ~139


## Dependencies and Imports

- `@lexical/code`
- `@lexical/react/LexicalContentEditable`
- `@lexical/react/LexicalErrorBoundary`
- `@lexical/react/LexicalRichTextPlugin`
- `@lexical/rich-text`
- `@/lib/utils`
- `@lexical/react/LexicalComposerContext`
- `lucide-react`
- `react`
- `react-i18next`
- `../ui/tooltip`
- `./theme`
- `./variable-node`
- `./variable-on-change-plugin`
- `./variable-picker-plugin`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/prompt-editor`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/prompt-editor/` directory
- Potential test file: `test_index.tsx`

## Keywords

../ui/tooltip, ./theme, ./variable-node, ./variable-on-change-plugin, ./variable-picker-plugin, @/lib/utils, @lexical/code, @lexical/react/LexicalComposerContext, @lexical/react/LexicalContentEditable, @lexical/react/LexicalErrorBoundary, @lexical/react/LexicalRichTextPlugin, @lexical/rich-text, Array, Catch, CodeHighlightNode, CodeNode, ContentEditable, EditorState, Error, ErrorBoundary, HeadingNode, IProps, InitialConfigType, Klass, Lexical, LexicalComposer, LexicalComposerContext, LexicalContentEditable, LexicalErrorBoundary, LexicalNode, LexicalRichTextPlugin, Nodes, PromptContent, PromptContentProps, PromptEditor, QuoteNode, ReactNode, RichTextPlugin, Tooltip, TooltipContent, TooltipTrigger, TypeScript, Variable, VariableNode, VariableOnChangePlugin, VariablePickerMenuPlugin, allNodes, handleBlur, handleFocus, handleVariableIconClick...

---
*Generated by RAGFlow Repository Documentation Generator*
