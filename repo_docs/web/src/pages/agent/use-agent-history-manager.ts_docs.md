# File Documentation: web/src/pages/agent/use-agent-history-manager.ts

## File Metadata

- **Path**: `web/src/pages/agent/use-agent-history-manager.ts`
- **Extension**: `.ts`
- **Lines**: 164
- **Characters**: 4,644
- **Size**: 4,644 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useEffect, useRef } from 'react';
import useGraphStore from './store';

// History management class
export class HistoryManager {
  private history: { nodes: any[]; edges: any[] }[] = [];
  private currentIndex: number = -1;
  private readonly maxSize: number = 50; // Limit maximum number of history records
  private setNodes: (nodes: any[]) => void;
  private setEdges: (edges: any[]) => void;
  private lastSavedState: string = ''; // Used to compare if state has changed

  constructor(
    setNodes: (nodes: any[]) => void,
    setEdges: (edges: any[]) => void,
  ) {
    this.setNodes = setNodes;
    this.setEdges = setEdges;
  }

  // Compare if two states are equal
  private statesEqual(
    state1: { nodes: any[]; edges: any[] },
    state2: { nodes: any[]; edges: any[] },
  ): boolean {
    return JSON.stringify(state1) === JSON.stringify(state2);
  }

  push(nodes: any[], edges: any[]) {
    const currentState = {
      nodes: JSON.parse(JSON.stringify(nodes)),
      edges: JSON.parse(JSON.stringify(edges)),
    };

    // If state hasn't changed, don't save
    if (
      this.history.length > 0 &&
      this.statesEqual(currentState, this.history[this.currentIndex])
    ) {
      return;
    }

    // If current index is not at the end of history, remove subsequent states
    if (this.currentIndex < this.history.length - 1) {
      this.history.splice(this.currentIndex + 1);
    }

    // Add current state
    this.history.push(currentState);

    // Limit history record size
    if (this.history.length > this.maxSize) {
      this.history.shift();
      this.currentIndex = this.history.length - 1;
    } else {
      this.currentIndex = this.history.length - 1;
    }

    // Update last saved state
    this.lastSavedState = JSON.stringify(currentState);
  }

  undo() {
    if (this.canUndo()) {
      this.currentIndex--;
      const prevState = this.history[this.currentIndex];
      this.setNodes(JSON.parse(JSON.stringify(prevState.nodes)));
      this.setEdges(JSON.parse(JSON.stringify(prevState.edges)));
      return true;
    }
    return false;
  }

  redo() {
    console.log('redo');
    if (this.canRedo()) {
      this.currentIndex++;
      const nextState = this.history[this.currentIndex];
      this.setNodes(JSON.parse(JSON.stringify(nextState.nodes)));
      this.setEdges(JSON.parse(JSON.stringify(nextState.edges)));
      return true;
    }
    return false;
  }

  canUndo() {
    return this.currentIndex > 0;
  }

  canRedo() {
    return this.currentIndex < this.history.length - 1;
  }

  // Reset history records
  reset() {
    this.history = [];
    this.currentIndex = -1;
    this.lastSavedState = '';
  }
}

export const useAgentHistoryManager = () => {
  // Get current state and history state
  const nodes = useGraphStore((state) => state.nodes);
  const edges = useGraphStore((state) => state.edges);
  const setNodes = useGraphStore((state) => state.setNodes);
  const setEdges = useGraphStore((state) => state.setEdges);

  // Use useRef to keep HistoryManager instance unchanged
  const historyManagerRef = useRef<HistoryManager | null>(null);

  // Initialize HistoryManager
  if (!historyManagerRef.current) {
    historyManagerRef.current = new HistoryManager(setNodes, setEdges);
  }

  const historyManager = historyManagerRef.current;

  // Save state history - use useEffect instead of useMemo to avoid re-rendering
  useEffect(() => {
    historyManager.push(nodes, edges);
  }, [nodes, edges, historyManager]);

  // Keyboard event handling
  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      // Check if focused on an input element
      const activeElement = document.activeElement;
      const isInputFocused =
        activeElement instanceof HTMLInputElement ||
        activeElement instanceof HTMLTextAreaElement ||
        activeElement?.hasAttribute('contenteditable');

      // Skip keyboard shortcuts if typing in an input field
      if (isInputFocused) {
        return;
      }
      // Ctrl+Z or Cmd+Z undo
      if (
        (e.ctrlKey || e.metaKey) &&
        (e.key === 'z' || e.key === 'Z') &&
        !e.shiftKey
      ) {
        e.preventDefault();
        historyManager.undo();
      }
      // Ctrl+Shift+Z or Cmd+Shift+Z redo
      else if (
        (e.ctrlKey || e.metaKey) &&
        (e.key === 'z' || e.key === 'Z') &&
        e.shiftKey
      ) {
        e.preventDefault();
        historyManager.redo();
      }
    };

    document.addEventListener('keydown', handleKeyDown);
    return () => {
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [historyManager]);
};

```

## High-Level Overview

// History management class

## Detailed Walkthrough

### Exports (2)

- `HistoryManager`: Exported entity
- `useAgentHistoryManager`: Exported entity

### Functions (7)

- `useAgentHistoryManager()`: Function definition
- `nodes()`: Function definition
- `edges()`: Function definition
- `setNodes()`: Function definition
- `setEdges()`: Function definition
- `historyManager()`: Function definition
- `handleKeyDown()`: Function definition

### Imports (2)

- `import { useEffect, useRef } from 'react';`
- `import useGraphStore from './store';`

## Code Structure Analysis

- Total lines: 164
- Blank lines: 23 (14.0%)
- Comment lines: ~17 (10.4%)
- Code lines: ~124


## Dependencies and Imports

- `react`
- `./store`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent`.

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

- Other files in `web/src/pages/agent/` directory
- Potential test file: `test_use-agent-history-manager.ts`

## Keywords

./store, Add, Check, Cmd, Compare, Ctrl, Get, HTMLInputElement, HTMLTextAreaElement, History, HistoryManager, Initialize, JSON, Keyboard, KeyboardEvent, Limit, Reset, Save, Shift, Skip, TypeScript, Update, Use, Used, activeElement, currentState, edges, export, handleKeyDown, historyManager, historyManagerRef, isInputFocused, nextState, nodes, prevState, react, setEdges, setNodes, useAgentHistoryManager

---
*Generated by RAGFlow Repository Documentation Generator*
