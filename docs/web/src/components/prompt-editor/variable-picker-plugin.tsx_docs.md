# Documentation: web/src/components/prompt-editor/variable-picker-plugin.tsx

## File Metadata

- **Path**: `web/src/components/prompt-editor/variable-picker-plugin.tsx`
- **Size**: 6944 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/prompt-editor/variable-picker-plugin.tsx`.

## Original Source Code

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
  useBasicTypeaheadTriggerMatch,
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
  useCallback,
  useContext,
  useEffect,
  useRef,
} from 'react';
import * as ReactDOM from 'react-dom';

import { FlowFormContext } from '@/pages/flow/context';
import { useBuildComponentIdSelectOptions } from '@/pages/flow/hooks/use-get-begin-query';
import { $createVariableNode } from './variable-node';

import { ProgrammaticTag } from './constant';
import './index.css';
class VariableInnerOption extends MenuOption {
  label: string;
  value: string;

  constructor(label: string, value: string) {
    super(value);
    this.label = label;
    this.value = value;
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
}: {
  index: number;
  option: VariableOption;
  selectOptionAndCleanUp: (
    option: VariableOption | VariableInnerOption,
  ) => void;
}) {
  return (
    <li
      key={option.key}
      tabIndex={-1}
      ref={option.setRefElement}
      role="option"
      id={'typeahead-item-' + index}
    >
      <div>
        <span className="text text-slate-500">{option.title}</span>
        <ul className="pl-2 py-1">
          {option.options.map((x) => (
            <li
              key={x.value}
              onClick={() => selectOptionAndCleanUp(x)}
              className="hover:bg-slate-300 p-1"
            >
              {x.label}
            </li>
          ))}
        </ul>
      </div>
    </li>
  );
}

export default function VariablePickerMenuPlugin({
  value,
}: {
  value?: string;
}): JSX.Element {
  const [editor] = useLexicalComposerContext();
  const isFirstRender = useRef(true);

  const node = useContext(FlowFormContext);

  const checkForTriggerMatch = useBasicTypeaheadTriggerMatch('/', {
    minLength: 0,
  });

  const [queryString, setQueryString] = React.useState<string | null>('');

  const options = useBuildComponentIdSelectOptions(node?.id, node?.parentId);

  const filteredOptions = React.useMemo(() => {
    if (!queryString) return options;
    const lowerQuery = queryString.toLowerCase();
    return options
      .map((x) => ({
        ...x,
        options: x.options.filter(
          (y) =>
            y.label.toLowerCase().includes(lowerQuery) ||
            y.value.toLowerCase().includes(lowerQuery),
        ),
      }))
      .filter((x) => x.options.length > 0);
  }, [options, queryString]);

  const nextOptions: VariableOption[] = filteredOptions.map(
    (x) =>
      new VariableOption(
        x.label,
        x.title,
        x.options.map((y) => new VariableInnerOption(y.label, y.value)),
      ),
  );

  const findLabelByValue = useCallback(
    (value: string) => {
      const children = options.reduce<Array<{ label: string; value: string }>>(
        (pre, cur) => {
          return pre.concat(cur.options);
        },
        [],
      );

      return children.find((x) => x.value === value)?.label;
    },
    [options],
  );

  const onSelectOption = useCallback(
    (
      selectedOption: VariableOption | VariableInnerOption,
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

        selection.insertNodes([
          $createVariableNode(
            (selectedOption as VariableInnerOption).value,
            selectedOption.label as string,
          ),
        ]);

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
        const label = findLabelByValue(content);
        if (label) {
          paragraph.append($createVariableNode(content, label));
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
    [findLabelByValue],
  );

  useEffect(() => {
    if (editor && value && isFirstRender.current) {
      isFirstRender.current = false;
      editor.update(
        () => {
          parseTextToVariableNodes(value);
        },
        { tag: ProgrammaticTag },
      );
    }
  }, [parseTextToVariableNodes, editor, value]);

  return (
    <LexicalTypeaheadMenuPlugin<VariableOption | VariableInnerOption>
      onQueryChange={setQueryString}
      onSelectOption={onSelectOption}
      triggerFn={checkForTriggerMatch}
      options={nextOptions}
      menuRenderFn={(anchorElementRef, { selectOptionAndCleanUp }) =>
        anchorElementRef.current && options.length
          ? ReactDOM.createPortal(
              <div className="typeahead-popover w-[200px] p-2">
                <ul>
                  {nextOptions.map((option, i: number) => (
                    <VariablePickerMenuItem
                      index={i}
                      key={option.key}
                      option={option}
                      selectOptionAndCleanUp={selectOptionAndCleanUp}
                    />
                  ))}
                </ul>
              </div>,
              anchorElementRef.current,
            )
          : null
      }
    />
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/prompt-editor/variable-picker-plugin.tsx` is located in the `web/src/components/prompt-editor` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to prompt-editor.

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

- [constant.ts](constant.ts_docs.md)
- [index.css](index.css_docs.md)
- [index.tsx](index.tsx_docs.md)
- [theme.ts](theme.ts_docs.md)
- [variable-node.tsx](variable-node.tsx_docs.md)
- [variable-on-change-plugin.tsx](variable-on-change-plugin.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
