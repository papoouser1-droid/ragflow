# File Documentation: web/src/stories/node-collapsible.stories.tsx

## File Metadata

- **Path**: `web/src/stories/node-collapsible.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 166
- **Characters**: 4,184
- **Size**: 4,184 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Form } from '@/components/ui/form';
import type { Meta, StoryObj } from '@storybook/react-webpack5';
import { useForm } from 'react-hook-form';

import { NodeCollapsible } from '@/components/collapse';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/NodeCollapsible',
  component: NodeCollapsible,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: `
## Component Description

NodeCollapsible is a specialized component for displaying collapsible content within nodes. 
It automatically shows only the first 3 items and provides a toggle button to expand/collapse the rest.
The component is designed to work within the application's node-based UI, such as in agent or data flow canvases.

The toggle button is displayed as a small circle at the bottom center of the component when there are more than 3 items.
        `,
      },
    },
  },
  tags: ['autodocs'],
  argTypes: {
    items: {
      control: 'object',
      description: 'Array of items to display in the collapsible component',
    },
    children: {
      control: false,
      description: 'Function to render each item',
    },
    className: {
      control: 'text',
      description: 'Additional CSS classes to apply to the component',
    },
  },
} satisfies Meta<typeof NodeCollapsible>;

// Form wrapper decorator
const WithFormProvider = ({ children }: { children: React.ReactNode }) => {
  const form = useForm({
    defaultValues: {},
    resolver: zodResolver(z.object({})),
  });
  return <Form {...form}>{children}</Form>;
};

const withFormProvider = (Story: any) => (
  <WithFormProvider>
    <Story />
  </WithFormProvider>
);

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Default: Story = {
  decorators: [withFormProvider],
  args: {
    items: [
      'Document Analysis Parser',
      'Web Search Parser',
      'Database Query Parser',
      'Image Recognition Parser',
      'Audio Transcription Parser',
      'Video Processing Parser',
      'Code Analysis Parser',
      'Spreadsheet Parser',
    ],
    children: (item: string) => (
      <div className="px-4 py-2 border rounded-md bg-bg-component">{item}</div>
    ),
  },
  parameters: {
    docs: {
      description: {
        story: `
### Basic Usage

By default, the NodeCollapsible component shows the first 3 items and collapses the rest.
A toggle button appears at the bottom when there are more than 3 items.

\`\`\`tsx
import { NodeCollapsible } from '@/components/collapse';

<NodeCollapsible 
  items={[
    'Document Analysis Parser',
    'Web Search Parser', 
    'Database Query Parser',
    'Image Recognition Parser',
    'Audio Transcription Parser',
    'Video Processing Parser',
    'Code Analysis Parser',
    'Spreadsheet Parser'
  ]}
>
  {(item) => (
    <div className="px-4 py-2 border rounded-md bg-bg-component">
      {item}
    </div>
  )}
</NodeCollapsible>
\`\`\`
        `,
      },
    },
  },
};

export const WithFewItems: Story = {
  decorators: [withFormProvider],
  args: {
    items: ['Single Item'],
    children: (item: string) => (
      <div className="px-4 py-2 border rounded-md bg-bg-component">{item}</div>
    ),
  },
  parameters: {
    docs: {
      description: {
        story: `
When there are 3 or fewer items, no toggle button is shown.
        `,
      },
    },
  },
};

export const WithManyItems: Story = {
  decorators: [withFormProvider],
  args: {
    items: [
      'Item 1',
      'Item 2',
      'Item 3',
      'Item 4',
      'Item 5',
      'Item 6',
      'Item 7',
      'Item 8',
    ],
    children: (item: string) => (
      <div className="px-4 py-2 border rounded-md bg-bg-component">{item}</div>
    ),
  },
  parameters: {
    docs: {
      description: {
        story: `
When there are more than 3 items, a toggle button is shown at the bottom center.
Clicking it will expand to show all items.
        `,
      },
    },
  },
};

```

## High-Level Overview

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
## Component Description

## Detailed Walkthrough

### Exports (3)

- `Default`: Exported entity
- `WithFewItems`: Exported entity
- `WithManyItems`: Exported entity

### Functions (2)

- `WithFormProvider()`: Function definition
- `withFormProvider()`: Function definition

### Imports (7)

- `import { Form } from '@/components/ui/form';`
- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { useForm } from 'react-hook-form';`
- `import { NodeCollapsible } from '@/components/collapse';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { z } from 'zod';`
- `import { NodeCollapsible } from '@/components/collapse';`

## Code Structure Analysis

- Total lines: 166
- Blank lines: 14 (8.4%)
- Comment lines: ~5 (3.0%)
- Code lines: ~147


## Dependencies and Imports

- `@/components/ui/form`
- `@storybook/react-webpack5`
- `react-hook-form`
- `@/components/collapse`
- `@hookform/resolvers/zod`
- `zod`
- `@/components/collapse`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/stories/` directory
- Potential test file: `test_node-collapsible.stories.tsx`

## Keywords

@/components/collapse, @/components/ui/form, @hookform/resolvers/zod, @storybook/react-webpack5, Additional, Analysis, Array, Audio, Basic, CSS, Clicking, Code, Component, Database, Default, Description, Document, Example, Form, Function, Image, Item, Meta, More, NodeCollapsible, Parser, Processing, Query, React, ReactNode, Recognition, Search, Single, Spreadsheet, Story, StoryObj, The, Transcription, TypeScript, Usage, Video, Web, When, WithFewItems, WithFormProvider, WithManyItems, form, hookform, meta, react-hook-form...

---
*Generated by RAGFlow Repository Documentation Generator*
