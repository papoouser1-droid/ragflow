# Documentation: web/src/pages/agent/canvas/node/index.less

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/index.less`
- **Size**: 4809 bytes
- **Type**: .less
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/canvas/node/index.less`.

## Original Source Code

```less
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/canvas/node/index.less` is located in the `web/src/pages/agent/canvas/node` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to node.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [agent-node.tsx](agent-node.tsx_docs.md)
- [begin-node.tsx](begin-node.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [categorize-node.tsx](categorize-node.tsx_docs.md)
- [data-operations-node.tsx](data-operations-node.tsx_docs.md)
- [dropdown.tsx](dropdown.tsx_docs.md)
- [email-node.tsx](email-node.tsx_docs.md)
- [extractor-node.tsx](extractor-node.tsx_docs.md)
- [file-node.tsx](file-node.tsx_docs.md)
- [handle-icon.tsx](handle-icon.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
