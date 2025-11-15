# Documentation: web/src/stories/number-input.stories.ts

## File Metadata

- **Path**: `web/src/stories/number-input.stories.ts`
- **Size**: 4120 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/stories/number-input.stories.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/stories/number-input.stories.ts` is located in the `web/src/stories` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to stories.

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

- [avatar-upload.stories.ts](avatar-upload.stories.ts_docs.md)
- [button-loading.stories.ts](button-loading.stories.ts_docs.md)
- [calendar.stories.tsx](calendar.stories.tsx_docs.md)
- [collapse.stories.tsx](collapse.stories.tsx_docs.md)
- [confirm-delete-dialog.stories.tsx](confirm-delete-dialog.stories.tsx_docs.md)
- [modal.stories.tsx](modal.stories.tsx_docs.md)
- [node-collapsible.stories.tsx](node-collapsible.stories.tsx_docs.md)
- [ragflow-avatar.stories.ts](ragflow-avatar.stories.ts_docs.md)
- [ragflow-form.stories.tsx](ragflow-form.stories.tsx_docs.md)
- [ragflow-pagination.stories.ts](ragflow-pagination.stories.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
