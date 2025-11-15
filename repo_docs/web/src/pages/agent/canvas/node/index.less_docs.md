# File Documentation: web/src/pages/agent/canvas/node/index.less

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/index.less`
- **Extension**: `.less`
- **Lines**: 299
- **Characters**: 4,807
- **Size**: 4,809 bytes
- **Purpose**: General file in the repository

## Original Source

```
.dark {
  background: rgb(63, 63, 63) !important;
}
.ragNode {
  .commonNode();
  .nodeName {
    font-size: 10px;
    color: black;
  }
  label {
    display: block;
    color: #777;
    font-size: 12px;
  }
  .description {
    font-size: 10px;
  }

  .categorizeAnchorPointText {
    position: absolute;
    top: -4px;
    left: 8px;
    white-space: nowrap;
  }
}

@lightBackgroundColor: rgba(150, 150, 150, 0.1);
@darkBackgroundColor: rgba(150, 150, 150, 0.2);

.selectedNode {
  border: 1.5px solid rgb(59, 118, 244);
}

.selectedIterationNode {
  border-bottom: 1.5px solid rgb(59, 118, 244);
  border-left: 1.5px solid rgb(59, 118, 244);
  border-right: 1.5px solid rgb(59, 118, 244);
}

.iterationHeader {
  .commonNodeShadow();
}

.selectedHeader {
  border-top: 1.9px solid rgb(59, 118, 244);
  border-left: 1.9px solid rgb(59, 118, 244);
  border-right: 1.9px solid rgb(59, 118, 244);
}

.handle {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 12px;
  height: 12px;
  background: rgb(59, 88, 253);
  border: 1px solid white;
  z-index: 1;
  background-image: url('@/assets/svg/plus.svg');
  background-size: cover;
  background-position: center;
}

.jsonView {
  word-wrap: break-word;
  overflow: auto;
  max-width: 300px;
  max-height: 500px;
}

.logicNode {
  .commonNode();

  .nodeName {
    font-size: 10px;
    color: black;
  }
  label {
    display: block;
    color: #777;
    font-size: 12px;
  }

  .description {
    font-size: 10px;
  }

  .categorizeAnchorPointText {
    position: absolute;
    top: -4px;
    left: 8px;
    white-space: nowrap;
  }
  .relevantSourceLabel {
    font-size: 10px;
  }
}

.noteNode {
  .commonNode();
  min-width: 140px;
  width: auto;
  height: 100%;
  padding: 8px;
  border-radius: 10px;
  min-height: 128px;
  .noteTitle {
    background-color: #edfcff;
    font-size: 12px;
    padding: 6px 6px 4px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  .noteTitleDark {
    background-color: #edfcff;
    font-size: 12px;
    padding: 6px 6px 4px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  .noteForm {
    margin-top: 4px;
    height: calc(100% - 50px);
  }
  .noteName {
    padding: 0px 4px;
  }
  .noteTextarea {
    resize: none;
    border: 0;
    border-radius: 0;
    height: 100%;
    &:focus {
      border: none;
      box-shadow: none;
    }
  }
}

.iterationNode {
  .commonNodeShadow();
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

.nodeText {
  padding-inline: 0.4em;
  padding-block: 0.2em 0.1em;
  background: @lightBackgroundColor;
  border-radius: 3px;
  min-height: 22px;
  .textEllipsis();
}

.nodeHeader {
  padding-bottom: 12px;
}

.zeroDivider {
  margin: 0 !important;
}

.conditionBlock {
  border-radius: 4px;
  padding: 6px;
  background: @lightBackgroundColor;
}

.conditionLine {
  border-radius: 4px;
  padding: 0 4px;
  background: @darkBackgroundColor;
  .textEllipsis();
}

.conditionKey {
  flex: 1;
}

.conditionOperator {
  padding: 0 2px;
  text-align: center;
}

.relevantLabel {
  text-align: right;
}

.knowledgeNodeName {
  .textEllipsis();
}

.messageNodeContainer {
  overflow-y: auto;
  max-height: 300px;
}

.generateParameters {
  padding-top: 8px;
  label {
    flex: 2;
    .textEllipsis();
  }
  .parameterValue {
    flex: 3;
    .conditionLine;
  }
}

.emailNodeContainer {
  padding: 8px;
  font-size: 12px;

  .emailConfig {
    background: rgba(0, 0, 0, 0.02);
    border-radius: 4px;
    padding: 8px;
    position: relative;
    cursor: pointer;

    &:hover {
      background: rgba(0, 0, 0, 0.04);
    }

    .configItem {
      display: flex;
      align-items: center;
      margin-bottom: 4px;

      &:last-child {
        margin-bottom: 0;
      }

      .configLabel {
        color: #666;
        width: 45px;
        flex-shrink: 0;
      }

      .configValue {
        color: #333;
        word-break: break-all;
      }
    }

    .expandIcon {
      position: absolute;
      right: 8px;
      top: 50%;
      transform: translateY(-50%);
      color: #666;
      font-size: 12px;
    }
  }

  .jsonExample {
    background: #f5f5f5;
    border-radius: 4px;
    padding: 8px;
    margin-top: 4px;
    animation: slideDown 0.2s ease-out;

    .jsonTitle {
      color: #666;
      margin-bottom: 4px;
    }

    .jsonContent {
      margin: 0;
      color: #333;
      font-family: monospace;
    }
  }
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hideScrollbar {
  /* Webkit browsers (Chrome, Safari, Edge) */
  &::-webkit-scrollbar {
    display: none;
  }

  /* Firefox */
  scrollbar-width: none;

  /* IE和Edge */
  -ms-overflow-style: none;
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/index.less`.

Based on the file structure and naming, it appears to be a general file in the repository.

The file contains approximately 299 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 299
- Blank lines: 41 (13.7%)
- Comment lines: ~3 (1.0%)
- Code lines: ~255


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node`.

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

- Other files in `web/src/pages/agent/canvas/node/` directory
- Potential test file: `test_index.less`

## Keywords

Chrome, Edge, Firefox, Safari, Webkit, darkBackgroundColor, keyframes, lightBackgroundColor

---
*Generated by RAGFlow Repository Documentation Generator*
