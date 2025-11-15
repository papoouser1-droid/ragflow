# Documentation: web/src/components/ui/input.tsx

## File Metadata

- **Path**: `web/src/components/ui/input.tsx`
- **Size**: 5252 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/input.tsx`.

## Original Source Code

```tsx
import * as React from 'react';

import { cn } from '@/lib/utils';
import { Eye, EyeOff, Search } from 'lucide-react';
import { useState } from 'react';
import { Button } from './button';

export interface InputProps
  extends Omit<React.InputHTMLAttributes<HTMLInputElement>, 'prefix'> {
  value?: string | number | readonly string[] | undefined;
  prefix?: React.ReactNode;
  suffix?: React.ReactNode;
}

const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ className, type, value, onChange, prefix, suffix, ...props }, ref) => {
    const isControlled = value !== undefined;
    const { defaultValue, ...restProps } = props;
    const inputValue = isControlled ? value : defaultValue;
    const [showPassword, setShowPassword] = useState(false);
    const handleChange: React.ChangeEventHandler<HTMLInputElement> = (e) => {
      if (type === 'number') {
        const numValue = e.target.value === '' ? '' : Number(e.target.value);
        onChange?.({
          ...e,
          target: {
            ...e.target,
            value: numValue,
          },
        } as React.ChangeEvent<HTMLInputElement>);
      } else {
        onChange?.(e);
      }
    };

    const isPasswordInput = type === 'password';

    const inputEl = (
      <input
        ref={ref}
        type={isPasswordInput && showPassword ? 'text' : type}
        className={cn(
          'peer/input',
          'flex h-8 w-full rounded-md border-0.5 border-input bg-bg-input px-3 py-2 outline-none text-sm text-text-primary',
          'file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-text-disabled',
          'focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-accent-primary',
          'disabled:cursor-not-allowed disabled:opacity-50 transition-colors',
          {
            'pl-12': !!prefix,
            'pr-12': !!suffix || isPasswordInput,
            'pr-24': !!suffix && isPasswordInput,
          },
          className,
        )}
        value={inputValue ?? ''}
        onChange={handleChange}
        {...restProps}
      />
    );

    if (prefix || suffix || isPasswordInput) {
      return (
        <div className="relative">
          {prefix && (
            <span className="absolute left-0 top-[50%] translate-y-[-50%]">
              {prefix}
            </span>
          )}
          {inputEl}
          {suffix && (
            <span
              className={cn('absolute right-0 top-[50%] translate-y-[-50%]', {
                'right-14': isPasswordInput,
              })}
            >
              {suffix}
            </span>
          )}
          {isPasswordInput && (
            <Button
              variant="transparent"
              type="button"
              className="
                absolute border-0 right-1 top-[50%] translate-y-[-50%]
                dark:peer-autofill/input:text-text-secondary-inverse
                dark:peer-autofill/input:hover:text-text-primary-inverse
                dark:peer-autofill/input:focus-visible:text-text-primary-inverse
              "
              onClick={() => setShowPassword(!showPassword)}
            >
              {showPassword ? (
                <EyeOff className="size-[1em]" />
              ) : (
                <Eye className="size-[1em]" />
              )}
            </Button>
          )}
        </div>
      );
    }

    return inputEl;
  },
);

Input.displayName = 'Input';

// eslint-disable-next-line @typescript-eslint/no-empty-interface
export interface ExpandedInputProps extends InputProps {}

const ExpandedInput = Input;

const SearchInput = (props: InputProps) => {
  return <Input {...props} prefix={<Search className="ml-3 size-[1em]" />} />;
};

type Value = string | readonly string[] | number | undefined;

export const InnerBlurInput = React.forwardRef<
  HTMLInputElement,
  InputProps & { value: Value; onChange(value: Value): void }
>(({ value, onChange, ...props }, ref) => {
  const [val, setVal] = React.useState<Value>();

  const handleChange: React.ChangeEventHandler<HTMLInputElement> =
    React.useCallback((e) => {
      setVal(e.target.value);
    }, []);

  const handleBlur: React.FocusEventHandler<HTMLInputElement> =
    React.useCallback(
      (e) => {
        onChange?.(e.target.value);
      },
      [onChange],
    );

  React.useEffect(() => {
    setVal(value);
  }, [value]);

  return (
    <Input
      {...props}
      value={val}
      onBlur={handleBlur}
      onChange={handleChange}
      ref={ref}
    ></Input>
  );
});

if (process.env.NODE_ENV !== 'production') {
  InnerBlurInput.whyDidYouRender = true;
}

export const BlurInput = React.memo(InnerBlurInput);

export { ExpandedInput, Input, SearchInput };

type NumberInputProps = { onChange?(value: number): void } & InputProps;

export const NumberInput = React.forwardRef<
  HTMLInputElement,
  NumberInputProps & { value: Value; onChange(value: Value): void }
>(function NumberInput({ onChange, ...props }, ref) {
  return (
    <Input
      type="number"
      onChange={(ev) => {
        const value = ev.target.value;
        onChange?.(value === '' ? 0 : Number(value)); // convert to number
      }}
      {...props}
      ref={ref}
    ></Input>
  );
});

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/input.tsx` is located in the `web/src/components/ui` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to ui.

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

- [accordion.tsx](accordion.tsx_docs.md)
- [alert-dialog.tsx](alert-dialog.tsx_docs.md)
- [aspect-ratio.tsx](aspect-ratio.tsx_docs.md)
- [async-tree-select.tsx](async-tree-select.tsx_docs.md)
- [avatar.tsx](avatar.tsx_docs.md)
- [badge.tsx](badge.tsx_docs.md)
- [breadcrumb.tsx](breadcrumb.tsx_docs.md)
- [button.tsx](button.tsx_docs.md)
- [card.tsx](card.tsx_docs.md)
- [checkbox.tsx](checkbox.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
