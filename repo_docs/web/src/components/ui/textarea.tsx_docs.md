# File Documentation: web/src/components/ui/textarea.tsx

## File Metadata

- **Path**: `web/src/components/ui/textarea.tsx`
- **Extension**: `.tsx`
- **Lines**: 116
- **Characters**: 3,143
- **Size**: 3,143 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { cn } from '@/lib/utils';
import {
  ChangeEventHandler,
  ComponentProps,
  FocusEventHandler,
  forwardRef,
  TextareaHTMLAttributes,
  useCallback,
  useEffect,
  useRef,
  useState,
} from 'react';
interface TextareaProps
  extends Omit<TextareaHTMLAttributes<HTMLTextAreaElement>, 'autoSize'> {
  autoSize?: {
    minRows?: number;
    maxRows?: number;
  };
}
const Textarea = forwardRef<HTMLTextAreaElement, TextareaProps>(
  ({ className, autoSize, ...props }, ref) => {
    const textareaRef = useRef<HTMLTextAreaElement>(null);
    const getLineHeight = (element: HTMLElement): number => {
      const style = window.getComputedStyle(element);
      return parseInt(style.lineHeight, 10) || 20;
    };
    const adjustHeight = useCallback(() => {
      if (!textareaRef.current) return;
      const lineHeight = getLineHeight(textareaRef.current);
      const maxHeight = (autoSize?.maxRows || 3) * lineHeight;
      textareaRef.current.style.height = 'auto';

      requestAnimationFrame(() => {
        if (!textareaRef.current) return;

        const scrollHeight = textareaRef.current.scrollHeight;
        textareaRef.current.style.height = `${Math.min(scrollHeight, maxHeight)}px`;
      });
    }, [autoSize]);

    useEffect(() => {
      if (autoSize) {
        adjustHeight();
      }
    }, [textareaRef, autoSize, adjustHeight]);

    useEffect(() => {
      if (typeof ref === 'function') {
        ref(textareaRef.current);
      } else if (ref) {
        ref.current = textareaRef.current;
      }
    }, [ref]);
    return (
      <textarea
        className={cn(
          'flex min-h-[80px] w-full bg-bg-input rounded-md border border-input px-3 py-2 text-base ring-offset-background placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 md:text-sm overflow-hidden',
          className,
        )}
        rows={autoSize?.minRows ?? props.rows ?? undefined}
        style={{
          maxHeight: autoSize?.maxRows
            ? `${autoSize.maxRows * 20}px`
            : undefined,
          overflow: autoSize ? 'auto' : undefined,
        }}
        ref={textareaRef}
        {...props}
      />
    );
  },
);
Textarea.displayName = 'Textarea';

export { Textarea };

type Value = string | readonly string[] | number | undefined;

export const BlurTextarea = forwardRef<
  HTMLTextAreaElement,
  ComponentProps<'textarea'> & {
    value: Value;
    onChange(value: Value): void;
  }
>(({ value, onChange, ...props }, ref) => {
  const [val, setVal] = useState<Value>();

  const handleChange: ChangeEventHandler<HTMLTextAreaElement> = useCallback(
    (e) => {
      setVal(e.target.value);
    },
    [],
  );

  const handleBlur: FocusEventHandler<HTMLTextAreaElement> = useCallback(
    (e) => {
      onChange?.(e.target.value);
    },
    [onChange],
  );

  useEffect(() => {
    setVal(value);
  }, [value]);

  return (
    <Textarea
      {...props}
      value={val}
      onBlur={handleBlur}
      onChange={handleChange}
      ref={ref}
    ></Textarea>
  );
});

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/ui/textarea.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 116 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `BlurTextarea`: Exported entity

### Functions (4)

- `Textarea()`: Function definition
- `getLineHeight()`: Function definition
- `adjustHeight()`: Function definition
- `BlurTextarea()`: Function definition

### Imports (2)

- `import { cn } from '@/lib/utils';`
- `import {`

## Code Structure Analysis

- Total lines: 116
- Blank lines: 12 (10.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~104


## Dependencies and Imports

- `@/lib/utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui`.

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

- Other files in `web/src/components/ui/` directory
- Potential test file: `test_textarea.tsx`

## Keywords

@/lib/utils, BlurTextarea, ChangeEventHandler, ComponentProps, FocusEventHandler, HTMLElement, HTMLTextAreaElement, Math, Omit, Textarea, TextareaHTMLAttributes, TextareaProps, TypeScript, Value, adjustHeight, getLineHeight, handleBlur, handleChange, lineHeight, maxHeight, scrollHeight, style, textareaRef

---
*Generated by RAGFlow Repository Documentation Generator*
