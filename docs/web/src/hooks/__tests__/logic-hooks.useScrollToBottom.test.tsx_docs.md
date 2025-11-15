# Documentation: web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx

## File Metadata

- **Path**: `web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx`
- **Size**: 4111 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/__tests__/logic-hooks.useScrollToBottom.test.tsx` is located in the `web/src/hooks/__tests__` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to __tests__.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
