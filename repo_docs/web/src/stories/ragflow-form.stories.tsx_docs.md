# File Documentation: web/src/stories/ragflow-form.stories.tsx

## File Metadata

- **Path**: `web/src/stories/ragflow-form.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 232
- **Characters**: 5,771
- **Size**: 5,771 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { zodResolver } from '@hookform/resolvers/zod';
import type { Meta, StoryObj } from '@storybook/react-webpack5';
import { useForm } from 'react-hook-form';
import { z } from 'zod';

import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Form } from '@/components/ui/form';
import { Input } from '@/components/ui/input';

// Define form schema
const FormSchema = z.object({
  username: z.string().min(2, {
    message: 'Username must be at least 2 characters.',
  }),
  email: z.string().email({
    message: 'Please enter a valid email address.',
  }),
  description: z.string().optional(),
});

// Create a wrapper component to demonstrate RAGFlowFormItem
function FormExample({
  horizontal = false,
  fieldName = 'username',
  label = 'Username',
  tooltip = 'Please enter your username',
  placeholder = 'Enter username',
}: {
  horizontal?: boolean;
  fieldName?: string;
  label?: string;
  tooltip?: string;
  placeholder?: string;
}) {
  const form = useForm({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      username: '',
      email: '',
      description: '',
    },
  });

  return (
    <div className="w-full p-4 border rounded-lg">
      <Form {...form}>
        <form className="space-y-4">
          <RAGFlowFormItem
            name={fieldName}
            label={label}
            tooltip={tooltip}
            horizontal={horizontal}
          >
            <Input placeholder={placeholder} />
          </RAGFlowFormItem>
        </form>
      </Form>
    </div>
  );
}

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/RAGFlowForm',
  component: FormExample,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## RAGFlowFormItem Component

RAGFlowFormItem is a wrapper component built on top of shadcn/ui Form components, providing unified form item styling and layout.

### Import Path
\`\`\`typescript
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Form } from '@/components/ui/form';
\`\`\`

### Basic Usage
\`\`\`tsx
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const FormSchema = z.object({
  username: z.string(),
});

function MyForm() {
  const form = useForm({
    resolver: zodResolver(FormSchema),
    defaultValues: { username: '' },
  });

  return (
    <Form {...form}>
      <form>
        <RAGFlowFormItem
          name="username"
          label="Username"
          tooltip="Please enter your username"
        >
          <Input placeholder="Enter username" />
        </RAGFlowFormItem>
      </form>
    </Form>
  );
}
\`\`\`

### Features
- Built-in FormField, FormItem, FormLabel, FormControl and FormMessage
- Supports both horizontal and vertical layouts
- Supports tooltip hints
- Fully compatible with react-hook-form
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ['autodocs'],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    horizontal: {
      description: 'Whether to display the form item horizontally',
      control: { type: 'boolean' },
      type: { name: 'boolean', required: false },
      defaultValue: false,
    },
    fieldName: {
      description: 'The name of the form field',
      control: { type: 'text' },
      type: { name: 'string', required: true },
    },
    label: {
      description: 'The label of the form field',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
    tooltip: {
      description: 'The tooltip text for the form field',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
    placeholder: {
      description: 'The placeholder text for the input',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
  },
  args: {
    horizontal: false,
    fieldName: 'username',
    label: 'Username',
    tooltip: 'Please enter your username',
    placeholder: 'Enter username',
  },
} satisfies Meta<typeof FormExample>;

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const VerticalLayout: Story = {
  args: {
    horizontal: false,
    fieldName: 'username',
    label: 'Username',
    tooltip: 'Please enter your username',
    placeholder: 'Enter username',
  },
  parameters: {
    docs: {
      description: {
        story: `
### Vertical Layout Example

Default vertical layout with label above the input field.

\`\`\`tsx
<RAGFlowFormItem
  name="username"
  label="Username"
  tooltip="Please enter your username"
  horizontal={false}
>
  <Input placeholder="Enter username" />
</RAGFlowFormItem>
\`\`\`
        `,
      },
    },
  },
  // tags: ['!dev'],
};

export const HorizontalLayout: Story = {
  args: {
    horizontal: true,
    fieldName: 'email',
    label: 'Email Address',
    tooltip: 'Please enter a valid email address',
    placeholder: 'Enter email',
  },
  parameters: {
    docs: {
      description: {
        story: `
### Horizontal Layout Example

Horizontal layout with label and input field on the same row.

\`\`\`tsx
<RAGFlowFormItem
  name="email"
  label="Email Address"
  tooltip="Please enter a valid email address"
  horizontal={true}
>
  <Input type="email" placeholder="Enter email" />
</RAGFlowFormItem>
\`\`\`
        `,
      },
    },
  },
  // tags: ['!dev'],
};

```

## High-Level Overview

// Define form schema

## Detailed Walkthrough

### Exports (2)

- `VerticalLayout`: Exported entity
- `HorizontalLayout`: Exported entity

### Functions (2)

- `FormExample()`: Function definition
- `MyForm()`: Function definition

### Imports (12)

- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { useForm } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Form } from '@/components/ui/form';`
- `import { Input } from '@/components/ui/input';`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Form } from '@/components/ui/form';`
- `import { useForm } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 232
- Blank lines: 20 (8.6%)
- Comment lines: ~15 (6.5%)
- Code lines: ~197


## Dependencies and Imports

- `@hookform/resolvers/zod`
- `@storybook/react-webpack5`
- `react-hook-form`
- `zod`
- `@/components/ragflow-form`
- `@/components/ui/form`
- `@/components/ui/input`
- `@/components/ragflow-form`
- `@/components/ui/form`
- `react-hook-form`
- `@hookform/resolvers/zod`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_ragflow-form.stories.tsx`

## Keywords

@/components/ragflow-form, @/components/ui/form, @/components/ui/input, @hookform/resolvers/zod, @storybook/react-webpack5, Address, Autodocs, Basic, Built, Canvas, Component, Create, Default, Define, Email, Enter, Example, Features, Form, FormControl, FormExample, FormField, FormItem, FormLabel, FormMessage, FormSchema, Fully, Horizontal, HorizontalLayout, Import, Input, Layout, Meta, More, MyForm, Optional, Path, Please, RAGFlowForm, RAGFlowFormItem, Story, StoryObj, Supports, The, This, TypeScript, Usage, Username, Vertical, VerticalLayout...

---
*Generated by RAGFlow Repository Documentation Generator*
