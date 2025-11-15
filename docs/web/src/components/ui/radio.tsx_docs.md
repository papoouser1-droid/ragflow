# Documentation: web/src/components/ui/radio.tsx

## File Metadata

- **Path**: `web/src/components/ui/radio.tsx`
- **Size**: 3276 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/ui/radio.tsx`.

## Original Source Code

```tsx
import { cn } from '@/lib/utils';
import React, { useContext, useState } from 'react';

const RadioGroupContext = React.createContext<{
  value: string | number;
  onChange: (value: string | number) => void;
  disabled?: boolean;
} | null>(null);

type RadioProps = {
  value: string | number;
  checked?: boolean;
  disabled?: boolean;
  onChange?: (checked: boolean) => void;
  children?: React.ReactNode;
};

function Radio({ value, checked, disabled, onChange, children }: RadioProps) {
  const groupContext = useContext(RadioGroupContext);
  const isControlled = checked !== undefined;
  // const [internalChecked, setInternalChecked] = useState(false);

  const isChecked = isControlled ? checked : groupContext?.value === value;
  const mergedDisabled = disabled || groupContext?.disabled;

  const handleClick = () => {
    if (mergedDisabled) return;

    // if (!isControlled) {
    //   setInternalChecked(!isChecked);
    // }

    if (onChange) {
      onChange(!isChecked);
    }

    if (groupContext && !groupContext.disabled) {
      groupContext.onChange(value);
    }
  };

  return (
    <label
      className={cn(
        'flex items-center cursor-pointer gap-2 text-sm',
        mergedDisabled && 'cursor-not-allowed opacity-50',
      )}
    >
      <span
        className={cn(
          'flex h-4 w-4 items-center justify-center rounded-full border border-border transition-colors',
          'peer outline-none focus-visible:border-border-button',
          isChecked && 'border-primary bg-primary/10',
          mergedDisabled && 'border-muted',
        )}
        onClick={handleClick}
      >
        {isChecked && (
          <div className="h-3 w-3 fill-primary text-primary bg-text-primary rounded-full" />
        )}
      </span>
      {children && <span className="text-foreground">{children}</span>}
    </label>
  );
}

type RadioGroupProps = {
  value?: string | number;
  defaultValue?: string | number;
  onChange?: (value: string | number) => void;
  disabled?: boolean;
  children: React.ReactNode;
  className?: string;
  direction?: 'horizontal' | 'vertical';
};

function Group({
  value,
  defaultValue,
  onChange,
  disabled,
  children,
  className,
  direction = 'horizontal',
}: RadioGroupProps) {
  const [internalValue, setInternalValue] = useState(defaultValue || '');

  const isControlled = value !== undefined;
  const mergedValue = isControlled ? value : internalValue;

  const handleChange = (val: string | number) => {
    if (disabled) return;

    if (!isControlled) {
      setInternalValue(val);
    }

    if (onChange) {
      onChange(val);
    }
  };

  return (
    <RadioGroupContext.Provider
      value={{
        value: mergedValue,
        onChange: handleChange,
        disabled,
      }}
    >
      <div
        className={cn(
          'flex gap-4',
          direction === 'vertical' ? 'flex-col' : 'flex-row',
          className,
        )}
      >
        {React.Children.map(children, (child) =>
          React.cloneElement(child as React.ReactElement, {
            disabled: disabled || child?.props?.disabled,
          }),
        )}
      </div>
    </RadioGroupContext.Provider>
  );
}

const RadioComponent = Object.assign(Radio, {
  Group,
});

export { RadioComponent as Radio };

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/ui/radio.tsx` is located in the `web/src/components/ui` directory.

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
