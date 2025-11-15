# File Documentation: web/src/components/ui/radio.tsx

## File Metadata

- **Path**: `web/src/components/ui/radio.tsx`
- **Extension**: `.tsx`
- **Lines**: 133
- **Characters**: 3,276
- **Size**: 3,276 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

  // const [internalChecked, setInternalChecked] = useState(false);

## Detailed Walkthrough


### Functions (5)

- `RadioGroupContext()`: Function definition
- `Radio()`: Function definition
- `handleClick()`: Function definition
- `Group()`: Function definition
- `handleChange()`: Function definition

### Imports (2)

- `import { cn } from '@/lib/utils';`
- `import React, { useContext, useState } from 'react';`

## Code Structure Analysis

- Total lines: 133
- Blank lines: 19 (14.3%)
- Comment lines: ~4 (3.0%)
- Code lines: ~110


## Dependencies and Imports

- `@/lib/utils`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/ui`.

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

- Other files in `web/src/components/ui/` directory
- Potential test file: `test_radio.tsx`

## Keywords

@/lib/utils, Children, Group, Object, Provider, Radio, RadioComponent, RadioGroupContext, RadioGroupProps, RadioProps, React, ReactElement, ReactNode, TypeScript, groupContext, handleChange, handleClick, isChecked, isControlled, mergedDisabled, mergedValue, react

---
*Generated by RAGFlow Repository Documentation Generator*
