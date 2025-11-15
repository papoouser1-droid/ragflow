# File Documentation: web/src/stories/collapse.stories.tsx

## File Metadata

- **Path**: `web/src/stories/collapse.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 154
- **Characters**: 4,273
- **Size**: 4,273 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import type { Meta, StoryObj } from '@storybook/react-webpack5';

import { fn } from 'storybook/test';

import { Collapse } from '@/components/collapse';
import { Button } from '@/components/ui/button';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/Collapse',
  component: Collapse,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## Component Description

Collapse is a component that allows you to show or hide content with a smooth animation. It can be controlled or uncontrolled and supports custom titles and right-aligned content.

The component uses a trigger element (typically with an icon) to toggle the visibility of its content. It's built on top of Radix UI's Collapsible primitive.
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    title: {
      control: 'text',
      description: 'The title text or element to display in the trigger',
    },
    open: {
      control: 'boolean',
      description: 'Controlled open state of the collapse',
    },
    defaultOpen: {
      control: 'boolean',
      description: 'Initial open state of the collapse',
    },
    disabled: {
      control: 'boolean',
      description: 'Whether the collapse is disabled',
    },
    rightContent: {
      control: 'text',
      description: 'Content to display on the right side of the trigger',
    },
    onOpenChange: {
      action: 'onOpenChange',
      description: 'Callback function when the open state changes',
    },
  },
  // Use `fn` to spy on the onClick arg, which will appear in the actions panel once invoked: https://storybook.js.org/docs/essentials/actions#action-args
  args: { onOpenChange: fn() },
} satisfies Meta<typeof Collapse>;

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Default: Story = {
  args: {
    title: 'Collapse Title',
    children: (
      <div className="p-4 border border-gray-200 rounded-md">
        <p>This is the collapsible content. It can be any React node.</p>
        <p>You can put any content here, including other components.</p>
      </div>
    ),
  },
  parameters: {
    docs: {
      description: {
        story: `
### Usage Examples

\`\`\`tsx
import { Collapse } from '@/components/collapse';

<Collapse title="Collapse Title">
  <div className="p-4 border border-gray-200 rounded-md">
    <p>This is the collapsible content.</p>
  </div>
</Collapse>
\`\`\`
        `,
      },
    },
  },
};

export const WithRightContent: Story = {
  args: {
    title: 'Collapse with Right Content',
    rightContent: <Button size="sm">Action</Button>,
    children: (
      <div className="p-4 border border-gray-200 rounded-md">
        <p>
          This collapse has additional content on the right side of the trigger.
        </p>
      </div>
    ),
  },
  parameters: {
    docs: {
      description: {
        story: `
### Usage Examples

\`\`\`tsx
import { Collapse } from '@/components/collapse';
import { Button } from '@/components/ui/button';

<Collapse 
  title="Collapse Title" 
  rightContent={<Button size="sm">Action</Button>}
>
  <div className="p-4 border border-gray-200 rounded-md">
    <p>Content with right-aligned action button.</p>
  </div>
</Collapse>
\`\`\`
        `,
      },
    },
  },
};

export const InitiallyClosed: Story = {
  args: {
    title: 'Initially Closed Collapse',
    defaultOpen: false,
    children: (
      <div className="p-4 border border-gray-200 rounded-md">
        <p>This collapse is initially closed.</p>
      </div>
    ),
  },
};

export const Disabled: Story = {
  args: {
    title: 'Disabled Collapse',
    disabled: true,
    children: (
      <div className="p-4 border border-gray-200 rounded-md">
        <p>This collapse is disabled and cannot be toggled.</p>
      </div>
    ),
  },
};

```

## High-Level Overview

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
## Component Description

## Detailed Walkthrough

### Exports (4)

- `Default`: Exported entity
- `WithRightContent`: Exported entity
- `InitiallyClosed`: Exported entity
- `Disabled`: Exported entity

### Functions (1)

- `when()`: Function definition

### Imports (7)

- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { fn } from 'storybook/test';`
- `import { Collapse } from '@/components/collapse';`
- `import { Button } from '@/components/ui/button';`
- `import { Collapse } from '@/components/collapse';`
- `import { Collapse } from '@/components/collapse';`
- `import { Button } from '@/components/ui/button';`

## Code Structure Analysis

- Total lines: 154
- Blank lines: 15 (9.7%)
- Comment lines: ~9 (5.8%)
- Code lines: ~130


## Dependencies and Imports

- `@storybook/react-webpack5`
- `storybook/test`
- `@/components/collapse`
- `@/components/ui/button`
- `@/components/collapse`
- `@/components/collapse`
- `@/components/ui/button`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

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

- Other files in `web/src/stories/` directory
- Potential test file: `test_collapse.stories.tsx`

## Keywords

@/components/collapse, @/components/ui/button, @storybook/react-webpack5, Action, Autodocs, Button, Callback, Canvas, Closed, Collapse, Collapsible, Component, Content, Controlled, Default, Description, Disabled, Example, Examples, Initial, Initially, InitiallyClosed, Meta, More, Optional, Radix, React, Right, Story, StoryObj, The, This, Title, TypeScript, Usage, Use, Whether, WithRightContent, You, meta, storybook, storybook/test, when

---
*Generated by RAGFlow Repository Documentation Generator*
