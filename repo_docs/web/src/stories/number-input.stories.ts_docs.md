# File Documentation: web/src/stories/number-input.stories.ts

## File Metadata

- **Path**: `web/src/stories/number-input.stories.ts`
- **Extension**: `.ts`
- **Lines**: 185
- **Characters**: 4,120
- **Size**: 4,120 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import type { Meta, StoryObj } from '@storybook/react-webpack5';

import { fn } from 'storybook/test';

import NumberInput from '@/components/originui/number-input';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/NumberInput',
  component: NumberInput,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## NumberInput Component

NumberInput is a numeric input component with increment/decrement buttons. It provides a user-friendly interface for entering numeric values with built-in validation and keyboard controls.

### Import Path
\`\`\`typescript
import NumberInput from '@/components/originui/number-input';
\`\`\`

### Basic Usage
\`\`\`tsx
import { useState } from 'react';
import NumberInput from '@/components/originui/number-input';

function MyComponent() {
  const [value, setValue] = useState(0);

  return (
    <NumberInput
      value={value}
      onChange={(newValue) => setValue(newValue)}
    />
  );
}
\`\`\`

### Features
- Increment/decrement buttons for easy value adjustment
- Keyboard input validation (only allows numeric input)
- Customizable height and styling
- Non-negative number validation
- Responsive design with Tailwind CSS
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ['autodocs'],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    value: {
      description: 'The current numeric value',
      control: { type: 'number' },
    },
    onChange: {
      description: 'Callback function called when value changes',
      control: false,
    },
    height: {
      description: 'Custom height for the input component',
      control: { type: 'text' },
    },
    className: {
      description: 'Additional CSS classes for styling',
      control: { type: 'text' },
    },
  },
  // Use `fn` to spy on the onChange arg, which will appear in the actions panel once invoked: https://storybook.js.org/docs/essentials/actions#action-args
  args: { onChange: fn() },
} satisfies Meta<typeof NumberInput>;

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Default: Story = {
  args: {
    value: 0,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Default Number Input

Shows the basic number input with default styling and zero value.

\`\`\`tsx
<NumberInput
  value={0}
  onChange={(value) => console.log('Value changed:', value)}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const WithInitialValue: Story = {
  args: {
    value: 10,
  },
  parameters: {
    docs: {
      description: {
        story: `
### With Initial Value

Shows the number input with a predefined initial value.

\`\`\`tsx
<NumberInput
  value={10}
  onChange={(value) => console.log('Value changed:', value)}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const CustomHeight: Story = {
  args: {
    value: 5,
    height: '60px',
  },
  parameters: {
    docs: {
      description: {
        story: `
### Custom Height

Shows the number input with custom height styling.

\`\`\`tsx
<NumberInput
  value={5}
  height="60px"
  onChange={(value) => console.log('Value changed:', value)}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const WithCustomClass: Story = {
  args: {
    value: 3,
    className: 'border-blue-500 bg-blue-50',
  },
  parameters: {
    docs: {
      description: {
        story: `
### With Custom Styling

Shows the number input with custom CSS classes for styling.

\`\`\`tsx
<NumberInput
  value={3}
  className="border-blue-500 bg-blue-50"
  onChange={(value) => console.log('Value changed:', value)}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

```

## High-Level Overview

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
## NumberInput Component

## Detailed Walkthrough

### Exports (4)

- `Default`: Exported entity
- `WithInitialValue`: Exported entity
- `CustomHeight`: Exported entity
- `WithCustomClass`: Exported entity

### Functions (2)

- `MyComponent()`: Function definition
- `called()`: Function definition

### Imports (6)

- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { fn } from 'storybook/test';`
- `import NumberInput from '@/components/originui/number-input';`
- `import NumberInput from '@/components/originui/number-input';`
- `import { useState } from 'react';`
- `import NumberInput from '@/components/originui/number-input';`

## Code Structure Analysis

- Total lines: 185
- Blank lines: 23 (12.4%)
- Comment lines: ~14 (7.6%)
- Code lines: ~148


## Dependencies and Imports

- `@storybook/react-webpack5`
- `storybook/test`
- `@/components/originui/number-input`
- `@/components/originui/number-input`
- `react`
- `@/components/originui/number-input`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/stories/` directory
- Potential test file: `test_number-input.stories.ts`

## Keywords

@/components/originui/number-input, @storybook/react-webpack5, Additional, Autodocs, Basic, CSS, Callback, Canvas, Component, Custom, CustomHeight, Customizable, Default, Example, Features, Height, Import, Increment, Initial, Input, Keyboard, Meta, More, MyComponent, Non, Number, NumberInput, Optional, Path, Responsive, Shows, Story, StoryObj, Styling, Tailwind, The, This, TypeScript, Usage, Use, Value, With, WithCustomClass, WithInitialValue, called, for, meta, react, storybook, storybook/test

---
*Generated by RAGFlow Repository Documentation Generator*
