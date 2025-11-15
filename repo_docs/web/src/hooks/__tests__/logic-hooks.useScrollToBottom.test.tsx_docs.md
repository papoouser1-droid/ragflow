# File Documentation: web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx

## File Metadata

- **Path**: `web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx`
- **Extension**: `.tsx`
- **Lines**: 128
- **Characters**: 4,111
- **Size**: 4,111 bytes
- **Purpose**: Testing - Contains unit tests, integration tests, or test utilities

## Original Source

```tsx
jest.mock('eventsource-parser/stream', () => ({}));

import { act, renderHook } from '@testing-library/react';
import { useScrollToBottom } from '../logic-hooks';

function createMockContainer({ atBottom = true } = {}) {
  const scrollTop = atBottom ? 100 : 0;
  const clientHeight = 100;
  const scrollHeight = 200;
  const listeners = {};
  return {
    current: {
      scrollTop,
      clientHeight,
      scrollHeight,
      addEventListener: jest.fn((event, cb) => {
        listeners[event] = cb;
      }),
      removeEventListener: jest.fn(),
    },
    listeners,
  } as any;
}

// Helper to flush all timers and microtasks
async function flushAll() {
  jest.runAllTimers();
  // Flush microtasks
  await Promise.resolve();
  // Sometimes, effects queue more timers, so run again
  jest.runAllTimers();
  await Promise.resolve();
}

describe('useScrollToBottom', () => {
  beforeEach(() => {
    jest.useFakeTimers();
  });
  afterEach(() => {
    jest.useRealTimers();
  });

  it('should set isAtBottom true when user is at bottom', () => {
    const containerRef = createMockContainer({ atBottom: true });
    const { result } = renderHook(() => useScrollToBottom([], containerRef));
    expect(result.current.isAtBottom).toBe(true);
  });

  it('should set isAtBottom false when user is not at bottom', () => {
    const containerRef = createMockContainer({ atBottom: false });
    const { result } = renderHook(() => useScrollToBottom([], containerRef));
    expect(result.current.isAtBottom).toBe(false);
  });

  it('should scroll to bottom when isAtBottom is true and messages change', async () => {
    const containerRef = createMockContainer({ atBottom: true });
    const mockScroll = jest.fn();

    function useTestScrollToBottom(messages: any, containerRef: any) {
      const hook = useScrollToBottom(messages, containerRef);
      hook.scrollRef.current = { scrollIntoView: mockScroll } as any;
      return hook;
    }

    const { rerender } = renderHook(
      ({ messages }) => useTestScrollToBottom(messages, containerRef),
      { initialProps: { messages: [] } },
    );

    rerender({ messages: ['msg1'] });
    await flushAll();

    expect(mockScroll).toHaveBeenCalled();
  });

  it('should NOT scroll to bottom when isAtBottom is false and messages change', async () => {
    const containerRef = createMockContainer({ atBottom: false });
    const mockScroll = jest.fn();

    function useTestScrollToBottom(messages: any, containerRef: any) {
      const hook = useScrollToBottom(messages, containerRef);
      hook.scrollRef.current = { scrollIntoView: mockScroll } as any;
      console.log('HOOK: isAtBottom:', hook.isAtBottom);
      return hook;
    }

    const { result, rerender } = renderHook(
      ({ messages }) => useTestScrollToBottom(messages, containerRef),
      { initialProps: { messages: [] } },
    );

    // Simulate user scrolls up before messages change
    await act(async () => {
      containerRef.current.scrollTop = 0;
      containerRef.current.addEventListener.mock.calls[0][1]();
      await flushAll();
      // Advance fake timers by 10ms instead of real setTimeout
      jest.advanceTimersByTime(10);
      console.log('AFTER SCROLL: isAtBottom:', result.current.isAtBottom);
    });

    rerender({ messages: ['msg1'] });
    await flushAll();

    console.log('AFTER RERENDER: isAtBottom:', result.current.isAtBottom);

    expect(mockScroll).not.toHaveBeenCalled();

    // Optionally, flush again after the assertion to see if it gets called late
    await flushAll();
  });

  it('should indicate button should appear when user is not at bottom', () => {
    const containerRef = createMockContainer({ atBottom: false });
    const { result } = renderHook(() => useScrollToBottom([], containerRef));
    // The button should appear in the UI when isAtBottom is false
    expect(result.current.isAtBottom).toBe(false);
  });
});

const originalRAF = global.requestAnimationFrame;
beforeAll(() => {
  global.requestAnimationFrame = (cb) => setTimeout(cb, 0);
});
afterAll(() => {
  global.requestAnimationFrame = originalRAF;
});

```

## High-Level Overview

// Helper to flush all timers and microtasks
  // Flush microtasks
  // Sometimes, effects queue more timers, so run again

## Detailed Walkthrough


### Functions (6)

- `createMockContainer()`: Function definition
- `listeners()`: Function definition
- `flushAll()`: Function definition
- `useTestScrollToBottom()`: Function definition
- `useTestScrollToBottom()`: Function definition
- `originalRAF()`: Function definition

### Imports (2)

- `import { act, renderHook } from '@testing-library/react';`
- `import { useScrollToBottom } from '../logic-hooks';`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 22 (17.2%)
- Comment lines: ~7 (5.5%)
- Code lines: ~99


## Dependencies and Imports

- `@testing-library/react`
- `../logic-hooks`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks/__tests__`.

This is a test file, contributing to the quality assurance and validation of the codebase.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

This is a test file. Run it using the project's test framework (pytest, jest, etc.).

## Related Files

- Other files in `web/src/hooks/__tests__/` directory

## Keywords

../logic-hooks, @testing-library/react, AFTER, Advance, Flush, HOOK, Helper, NOT, Optionally, Promise, RERENDER, SCROLL, Simulate, Sometimes, The, TypeScript, clientHeight, containerRef, createMockContainer, flushAll, hook, listeners, mockScroll, originalRAF, scrollHeight, scrollTop, testing, useTestScrollToBottom

---
*Generated by RAGFlow Repository Documentation Generator*
