# File Documentation: web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx`
- **Extension**: `.tsx`
- **Lines**: 413
- **Characters**: 11,211
- **Size**: 11,211 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
/**
 * Copyright (c) Meta Platforms, Inc. and affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 */

import { useLexicalComposerContext } from '@lexical/react/LexicalComposerContext';
import {
  LexicalTypeaheadMenuPlugin,
  MenuOption,
} from '@lexical/react/LexicalTypeaheadMenuPlugin';
import {
  $createParagraphNode,
  $createTextNode,
  $getRoot,
  $getSelection,
  $isRangeSelection,
  TextNode,
} from 'lexical';
import React, {
  ReactElement,
  ReactNode,
  useCallback,
  useEffect,
  useRef,
} from 'react';
import * as ReactDOM from 'react-dom';

import { $createVariableNode } from './variable-node';

import { JsonSchemaDataType } from '@/pages/agent/constant';
import {
  useFindAgentStructuredOutputLabel,
  useShowSecondaryMenu,
} from '@/pages/agent/hooks/use-build-structured-output';
import { useFilterQueryVariableOptionsByTypes } from '@/pages/agent/hooks/use-get-begin-query';
import { get } from 'lodash';
import { PromptIdentity } from '../../agent-form/use-build-prompt-options';
import { StructuredOutputSecondaryMenu } from '../structured-output-secondary-menu';
import { ProgrammaticTag } from './constant';
import './index.css';
class VariableInnerOption extends MenuOption {
  label: string;
  value: string;
  parentLabel: string | JSX.Element;
  icon?: ReactNode;
  type?: string;

  constructor(
    label: string,
    value: string,
    parentLabel: string | JSX.Element,
    icon?: ReactNode,
    type?: string,
  ) {
    super(value);
    this.label = label;
    this.value = value;
    this.parentLabel = parentLabel;
    this.icon = icon;
    this.type = type;
  }
}

class VariableOption extends MenuOption {
  label: ReactElement | string;
  title: string;
  options: VariableInnerOption[];

  constructor(
    label: ReactElement | string,
    title: string,
    options: VariableInnerOption[],
  ) {
    super(title);
    this.label = label;
    this.title = title;
    this.options = options;
  }
}

function VariablePickerMenuItem({
  index,
  option,
  selectOptionAndCleanUp,
  types,
}: {
  index: number;
  option: VariableOption;
  types?: JsonSchemaDataType[];
  selectOptionAndCleanUp: (
    option: VariableOption | VariableInnerOption,
  ) => void;
}) {
  const showSecondaryMenu = useShowSecondaryMenu();

  return (
    <li
      key={option.key}
      tabIndex={-1}
      ref={option.setRefElement}
      role="option"
      id={'typeahead-item-' + index}
    >
      <div>
        <span className="text text-text-secondary">{option.title}</span>
        <ul className="pl-2 py-1">
          {option.options.map((x) => {
            const shouldShowSecondary = showSecondaryMenu(x.value, x.label);

            if (shouldShowSecondary) {
              return (
                <StructuredOutputSecondaryMenu
                  key={x.value}
                  data={x}
                  types={types}
                  click={(y) =>
                    selectOptionAndCleanUp({
                      ...x,
                      ...y,
                    } as VariableInnerOption)
                  }
                ></StructuredOutputSecondaryMenu>
              );
            }

            return (
              <li
                key={x.value}
                onClick={() => selectOptionAndCleanUp(x)}
                className="hover:bg-bg-card p-1 text-text-primary rounded-sm flex justify-between items-center"
              >
                <span className="truncate flex-1 min-w-0">{x.label}</span>
                <span className="text-text-secondary">{get(x, 'type')}</span>
              </li>
            );
          })}
        </ul>
      </div>
    </li>
  );
}

export type VariablePickerMenuOptionType = {
  label: string;
  title: string;
  value?: string;
  options: Array<{
    label: string;
    value: string;
    icon: ReactNode;
    type?: string;
  }>;
};

export type VariablePickerMenuPluginProps = {
  value?: string;
  extraOptions?: VariablePickerMenuOptionType[];
  baseOptions?: VariablePickerMenuOptionType[];
  types?: JsonSchemaDataType[];
};
export default function VariablePickerMenuPlugin({
  value,
  extraOptions,
  baseOptions,
  types,
}: VariablePickerMenuPluginProps): JSX.Element {
  const [editor] = useLexicalComposerContext();

  const findAgentStructuredOutputLabel = useFindAgentStructuredOutputLabel();

  // const checkForTriggerMatch = useBasicTypeaheadTriggerMatch('/', {
  //   minLength: 0,
  // });

  const testTriggerFn = React.useCallback((text: string) => {
    const lastChar = text.slice(-1);
    if (lastChar === '/') {
      console.log('Found trigger character "/"');
      return {
        leadOffset: text.length - 1,
        matchingString: '',
        replaceableString: '/',
      };
    }
    return null;
  }, []);

  const previousValue = useRef<string | undefined>();

  const [queryString, setQueryString] = React.useState<string | null>('');

  let options = useFilterQueryVariableOptionsByTypes(types);

  if (baseOptions) {
    options = baseOptions as typeof options;
  }

  const buildNextOptions = useCallback(() => {
    let filteredOptions = [...options, ...(extraOptions ?? [])];
    if (queryString) {
      const lowerQuery = queryString.toLowerCase();
      filteredOptions = options
        .map((x) => ({
          ...x,
          options: x.options.filter(
            (y) =>
              y.label.toLowerCase().includes(lowerQuery) ||
              y.value.toLowerCase().includes(lowerQuery),
          ),
        }))
        .filter((x) => x.options.length > 0);
    }

    const finalOptions: VariableOption[] = filteredOptions.map(
      (x) =>
        new VariableOption(
          x.label,
          x.title,
          x.options.map((y) => {
            return new VariableInnerOption(
              y.label,
              y.value,
              x.label,
              y.icon,
              y.type,
            );
          }),
        ),
    );
    return finalOptions;
  }, [extraOptions, options, queryString]);

  const findItemByValue = useCallback(
    (value: string) => {
      const children = options.reduce<
        Array<{
          label: string;
          value: string;
          parentLabel?: string | ReactNode;
          icon?: ReactNode;
        }>
      >((pre, cur) => {
        return pre.concat(cur.options);
      }, []);

      // agent structured output
      const agentStructuredOutput = findAgentStructuredOutputLabel(
        value,
        children,
      );
      if (agentStructuredOutput) {
        return agentStructuredOutput;
      }

      return children.find((x) => x.value === value);
    },
    [findAgentStructuredOutputLabel, options],
  );

  const onSelectOption = useCallback(
    (
      selectedOption: VariableInnerOption,
      nodeToRemove: TextNode | null,
      closeMenu: () => void,
    ) => {
      editor.update(() => {
        const selection = $getSelection();

        if (!$isRangeSelection(selection) || selectedOption === null) {
          return;
        }

        if (nodeToRemove) {
          nodeToRemove.remove();
        }
        const variableNode = $createVariableNode(
          (selectedOption as VariableInnerOption).value,
          selectedOption.label as string,
          selectedOption.parentLabel as string | ReactNode,
          selectedOption.icon as ReactNode,
        );
        if (selectedOption.parentLabel === PromptIdentity) {
          selection.insertText(selectedOption.value);
        } else {
          selection.insertNodes([variableNode]);
        }

        closeMenu();
      });
    },
    [editor],
  );

  const parseTextToVariableNodes = useCallback(
    (text: string) => {
      const paragraph = $createParagraphNode();

      // Regular expression to match content within {}
      const regex = /{([^}]*)}/g;
      let match;
      let lastIndex = 0;
      while ((match = regex.exec(text)) !== null) {
        const { 1: content, index, 0: template } = match;

        // Add the previous text part (if any)
        if (index > lastIndex) {
          const textNode = $createTextNode(text.slice(lastIndex, index));

          paragraph.append(textNode);
        }

        // Add variable node or text node
        const nodeItem = findItemByValue(content);

        if (nodeItem) {
          paragraph.append(
            $createVariableNode(
              content,
              nodeItem.label,
              nodeItem.parentLabel,
              nodeItem.icon,
            ),
          );
        } else {
          paragraph.append($createTextNode(template));
        }

        // Update index
        lastIndex = regex.lastIndex;
      }

      // Add the last part of text (if any)
      if (lastIndex < text.length) {
        const textNode = $createTextNode(text.slice(lastIndex));
        paragraph.append(textNode);
      }

      $getRoot().clear().append(paragraph);

      if ($isRangeSelection($getSelection())) {
        $getRoot().selectEnd();
      }
    },
    [findItemByValue],
  );

  useEffect(() => {
    if (editor && value && value !== previousValue.current) {
      previousValue.current = value;
      editor.update(
        () => {
          parseTextToVariableNodes(value);
        },
        { tag: ProgrammaticTag },
      );
    }
  }, [parseTextToVariableNodes, editor, value]);

  // Fixed the issue where the cursor would go to the end when changing its own data
  useEffect(() => {
    return editor.registerUpdateListener(({ editorState, tags }) => {
      // If we trigger the programmatic update ourselves, we should not write back to avoid an infinite loop.
      if (tags.has(ProgrammaticTag)) return;

      editorState.read(() => {
        const text = $getRoot().getTextContent();
        if (text !== previousValue.current) {
          previousValue.current = text;
        }
      });
    });
  }, [editor]);

  return (
    <LexicalTypeaheadMenuPlugin<VariableOption | VariableInnerOption>
      onQueryChange={setQueryString}
      onSelectOption={(option, textNodeContainingQuery, closeMenu) =>
        onSelectOption(
          option as VariableInnerOption, // Only the second level menu can be selected
          textNodeContainingQuery,
          closeMenu,
        )
      }
      triggerFn={testTriggerFn}
      options={buildNextOptions()}
      menuRenderFn={(anchorElementRef, { selectOptionAndCleanUp }) => {
        const nextOptions = buildNextOptions();
        return anchorElementRef.current && nextOptions.length
          ? ReactDOM.createPortal(
              <div className="typeahead-popover w-80 p-2 bg-bg-base">
                <ul className="scroll-auto overflow-x-hidden">
                  {nextOptions.map((option, i: number) => (
                    <VariablePickerMenuItem
                      index={i}
                      key={option.key}
                      option={option}
                      types={types}
                      selectOptionAndCleanUp={selectOptionAndCleanUp}
                    />
                  ))}
                </ul>
              </div>,
              anchorElementRef.current,
            )
          : null;
      }}
    />
  );
}

```

## High-Level Overview

/**

## Detailed Walkthrough

### Exports (1)

- `VariablePickerMenuPlugin`: Exported entity

### Functions (10)

- `VariablePickerMenuItem()`: Function definition
- `VariablePickerMenuPlugin()`: Function definition
- `testTriggerFn()`: Function definition
- `buildNextOptions()`: Function definition
- `findItemByValue()`: Function definition
- `children()`: Function definition
- `agentStructuredOutput()`: Function definition
- `onSelectOption()`: Function definition
- `parseTextToVariableNodes()`: Function definition
- `textNode()`: Function definition

### Imports (14)

- `import { useLexicalComposerContext } from '@lexical/react/LexicalComposerContext';`
- `import {`
- `import {`
- `import React, {`
- `import * as ReactDOM from 'react-dom';`
- `import { $createVariableNode } from './variable-node';`
- `import { JsonSchemaDataType } from '@/pages/agent/constant';`
- `import {`
- `import { useFilterQueryVariableOptionsByTypes } from '@/pages/agent/hooks/use-get-begin-query';`
- `import { get } from 'lodash';`

## Code Structure Analysis

- Total lines: 413
- Blank lines: 43 (10.4%)
- Comment lines: ~18 (4.4%)
- Code lines: ~352


## Dependencies and Imports

- `@lexical/react/LexicalComposerContext`
- `react-dom`
- `./variable-node`
- `@/pages/agent/constant`
- `@/pages/agent/hooks/use-get-begin-query`
- `lodash`
- `../../agent-form/use-build-prompt-options`
- `../structured-output-secondary-menu`
- `./constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/components/prompt-editor`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

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

- Other files in `web/src/pages/agent/form/components/prompt-editor/` directory
- Potential test file: `test_variable-picker-plugin.tsx`

## Keywords

../../agent-form/use-build-prompt-options, ../structured-output-secondary-menu, ./constant, ./variable-node, @/pages/agent/constant, @/pages/agent/hooks/use-get-begin-query, @lexical/react/LexicalComposerContext, Add, Array, Copyright, Element, Fixed, Found, Inc, JSX, JsonSchemaDataType, LICENSE, LexicalComposerContext, LexicalTypeaheadMenuPlugin, MIT, MenuOption, Meta, Only, Platforms, ProgrammaticTag, PromptIdentity, React, ReactDOM, ReactElement, ReactNode, Regular, StructuredOutputSecondaryMenu, TextNode, This, TypeScript, Update, VariableInnerOption, VariableOption, VariablePickerMenuItem, VariablePickerMenuOptionType, VariablePickerMenuPlugin, VariablePickerMenuPluginProps, agentStructuredOutput, buildNextOptions, checkForTriggerMatch, children, filteredOptions, finalOptions, findAgentStructuredOutputLabel, findItemByValue...

---
*Generated by RAGFlow Repository Documentation Generator*
