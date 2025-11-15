# File Documentation: web/src/pages/agent/hooks/use-dropdown-position.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-dropdown-position.ts`
- **Extension**: `.ts`
- **Lines**: 114
- **Characters**: 3,103
- **Size**: 3,103 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { ReactFlowInstance } from '@xyflow/react';
import { useCallback } from 'react';
import {
  DROPDOWN_HORIZONTAL_OFFSET,
  DROPDOWN_VERTICAL_OFFSET,
  HALF_PLACEHOLDER_NODE_WIDTH,
} from '../constant';

/**
 * Dropdown position calculation Hook
 * Responsible for calculating dropdown menu position relative to placeholder node
 */
export const useDropdownPosition = (
  reactFlowInstance?: ReactFlowInstance<any, any>,
) => {
  /**
   * Calculate dropdown menu position
   * @param clientX Mouse click screen X coordinate
   * @param clientY Mouse click screen Y coordinate
   * @returns Dropdown menu screen coordinates
   */
  const calculateDropdownPosition = useCallback(
    (clientX: number, clientY: number) => {
      if (!reactFlowInstance) {
        return { x: clientX, y: clientY };
      }

      // Convert screen coordinates to flow coordinates
      const placeholderNodePosition = reactFlowInstance.screenToFlowPosition({
        x: clientX,
        y: clientY,
      });

      // Calculate dropdown position in flow coordinate system
      const dropdownFlowPosition = {
        x:
          placeholderNodePosition.x +
          HALF_PLACEHOLDER_NODE_WIDTH +
          DROPDOWN_HORIZONTAL_OFFSET,
        y: placeholderNodePosition.y - DROPDOWN_VERTICAL_OFFSET,
      };

      // Convert flow coordinates back to screen coordinates
      const dropdownScreenPosition =
        reactFlowInstance.flowToScreenPosition(dropdownFlowPosition);

      return {
        x: dropdownScreenPosition.x,
        y: dropdownScreenPosition.y,
      };
    },
    [reactFlowInstance],
  );

  /**
   * Calculate placeholder node flow coordinate position
   * @param clientX Mouse click screen X coordinate
   * @param clientY Mouse click screen Y coordinate
   * @returns Placeholder node flow coordinates
   */
  const getPlaceholderNodePosition = useCallback(
    (clientX: number, clientY: number) => {
      if (!reactFlowInstance) {
        return { x: clientX, y: clientY };
      }

      return reactFlowInstance.screenToFlowPosition({
        x: clientX,
        y: clientY,
      });
    },
    [reactFlowInstance],
  );

  /**
   * Convert flow coordinates to screen coordinates
   * @param flowPosition Flow coordinates
   * @returns Screen coordinates
   */
  const flowToScreenPosition = useCallback(
    (flowPosition: { x: number; y: number }) => {
      if (!reactFlowInstance) {
        return flowPosition;
      }

      return reactFlowInstance.flowToScreenPosition(flowPosition);
    },
    [reactFlowInstance],
  );

  /**
   * Convert screen coordinates to flow coordinates
   * @param screenPosition Screen coordinates
   * @returns Flow coordinates
   */
  const screenToFlowPosition = useCallback(
    (screenPosition: { x: number; y: number }) => {
      if (!reactFlowInstance) {
        return screenPosition;
      }

      return reactFlowInstance.screenToFlowPosition(screenPosition);
    },
    [reactFlowInstance],
  );

  return {
    calculateDropdownPosition,
    getPlaceholderNodePosition,
    flowToScreenPosition,
    screenToFlowPosition,
  };
};

```

## High-Level Overview

/**
  /**

## Detailed Walkthrough

### Exports (1)

- `useDropdownPosition`: Exported entity

### Functions (5)

- `useDropdownPosition()`: Function definition
- `calculateDropdownPosition()`: Function definition
- `getPlaceholderNodePosition()`: Function definition
- `flowToScreenPosition()`: Function definition
- `screenToFlowPosition()`: Function definition

### Imports (3)

- `import { ReactFlowInstance } from '@xyflow/react';`
- `import { useCallback } from 'react';`
- `import {`

## Code Structure Analysis

- Total lines: 114
- Blank lines: 13 (11.4%)
- Comment lines: ~29 (25.4%)
- Code lines: ~72


## Dependencies and Imports

- `@xyflow/react`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-dropdown-position.ts`

## Keywords

@xyflow/react, Calculate, Convert, DROPDOWN_HORIZONTAL_OFFSET, DROPDOWN_VERTICAL_OFFSET, Dropdown, Flow, HALF_PLACEHOLDER_NODE_WIDTH, Hook, Mouse, Placeholder, ReactFlowInstance, Responsible, Screen, TypeScript, calculateDropdownPosition, dropdownFlowPosition, dropdownScreenPosition, flowToScreenPosition, getPlaceholderNodePosition, param, placeholderNodePosition, react, returns, screenToFlowPosition, useDropdownPosition, xyflow

---
*Generated by RAGFlow Repository Documentation Generator*
