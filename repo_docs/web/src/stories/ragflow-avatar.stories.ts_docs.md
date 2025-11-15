# File Documentation: web/src/stories/ragflow-avatar.stories.ts

## File Metadata

- **Path**: `web/src/stories/ragflow-avatar.stories.ts`
- **Extension**: `.ts`
- **Lines**: 193
- **Characters**: 4,822
- **Size**: 4,822 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import type { Meta, StoryObj } from '@storybook/react-webpack5';

import { RAGFlowAvatar } from '@/components/ragflow-avatar';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/RAGFlowAvatar',
  component: RAGFlowAvatar,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## RAGFlowAvatar Component

RAGFlowAvatar is a customizable avatar component that displays user avatars with intelligent fallbacks. When an image is not available, it generates colorful gradient backgrounds with user initials.

### Import Path
\`\`\`typescript
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
\`\`\`

### Basic Usage
\`\`\`tsx
import { RAGFlowAvatar } from '@/components/ragflow-avatar';

function MyComponent() {
  return (
    <RAGFlowAvatar
      name="John Doe"
      avatar="https://example.com/avatar.jpg"
      isPerson={true}
    />
  );
}
\`\`\`

### Features
- Displays user avatar images when available
- Generates colorful gradient fallbacks with initials
- Supports both person (circular) and non-person (rounded) styles
- Automatic font size calculation based on container size
- Color generation based on name for consistent appearance
- Responsive design with resize observer
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ['autodocs'],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    name: {
      description:
        'The name to display initials for when no avatar is available',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
    avatar: {
      description: 'The URL of the avatar image',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
    isPerson: {
      description: 'Whether this avatar represents a person (affects styling)',
      control: { type: 'boolean' },
      type: { name: 'boolean', required: false },
      defaultValue: false,
    },
    className: {
      description: 'Additional CSS classes to apply',
      control: { type: 'text' },
      type: { name: 'string', required: false },
    },
  },
  args: {
    name: 'John Doe',
    isPerson: false,
  },
} satisfies Meta<typeof RAGFlowAvatar>;

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const WithInitials: Story = {
  args: {
    name: 'John Doe',
    isPerson: false,
  },
  parameters: {
    docs: {
      description: {
        story: `
### With Initials Only

Shows the avatar component with only a name, displaying generated initials with a gradient background.

\`\`\`tsx
<RAGFlowAvatar
  name="John Doe"
  isPerson={false}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const WithAvatar: Story = {
  args: {
    name: 'Jane Smith',
    avatar:
      'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHJlY3Qgd2lkdGg9IjY0IiBoZWlnaHQ9IjY0IiByeD0iMzIiIGZpbGw9IiM0RjZERUUiLz4KPGNpcmNsZSBjeD0iMzIiIGN5PSIyNCIgcj0iOCIgZmlsbD0id2hpdGUiLz4KPHBhdGggZD0iTTQ4IDQ4QzQ4IDQxLjM3MyA0MS45NDA2IDM2IDM0LjUgMzZIMjkuNUMyMi4wNTk0IDM2IDE2IDQxLjM3MyAxNiA0OCIgZmlsbD0id2hpdGUiLz4KPC9zdmc+',
    isPerson: true,
  },
  parameters: {
    docs: {
      description: {
        story: `
### With Avatar Image

Shows the avatar component with an actual image. When isPerson is true, the avatar will be circular.

\`\`\`tsx
<RAGFlowAvatar
  name="Jane Smith"
  avatar="https://example.com/avatar.jpg"
  isPerson={true}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const PersonStyle: Story = {
  args: {
    name: 'Alice Johnson',
    isPerson: true,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Person Style (Circular)

Shows the avatar component with isPerson set to true, which makes it circular.

\`\`\`tsx
<RAGFlowAvatar
  name="Alice Johnson"
  isPerson={true}
/>
\`\`\`
        `,
      },
    },
  },
  tags: ['!dev'],
};

export const NonPersonStyle: Story = {
  args: {
    name: 'Bot Assistant',
    isPerson: false,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Non-Person Style (Rounded Rectangle)

Shows the avatar component with isPerson set to false, which makes it a rounded rectangle.

\`\`\`tsx
<RAGFlowAvatar
  name="Bot Assistant"
  isPerson={false}
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
## RAGFlowAvatar Component

## Detailed Walkthrough

### Exports (4)

- `WithInitials`: Exported entity
- `WithAvatar`: Exported entity
- `PersonStyle`: Exported entity
- `NonPersonStyle`: Exported entity

### Functions (1)

- `MyComponent()`: Function definition

### Imports (4)

- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`

## Code Structure Analysis

- Total lines: 193
- Blank lines: 21 (10.9%)
- Comment lines: ~13 (6.7%)
- Code lines: ~159


## Dependencies and Imports

- `@storybook/react-webpack5`
- `@/components/ragflow-avatar`
- `@/components/ragflow-avatar`
- `@/components/ragflow-avatar`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_ragflow-avatar.stories.ts`

## Keywords

@/components/ragflow-avatar, @storybook/react-webpack5, Additional, Alice, Assistant, Autodocs, Automatic, Avatar, Basic, Bot, CSS, Canvas, Circular, Color, Component, Displays, Doe, Example, Features, Generates, Image, Import, Initials, Jane, John, Johnson, Meta, More, MyComponent, Non, NonPersonStyle, Only, Optional, PHN2ZyB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHJlY3Qgd2lkdGg9IjY0IiBoZWlnaHQ9IjY0IiByeD0iMzIiIGZpbGw9IiM0RjZERUUiLz4KPGNpcmNsZSBjeD0iMzIiIGN5PSIyNCIgcj0iOCIgZmlsbD0id2hpdGUiLz4KPHBhdGggZD0iTTQ4IDQ4QzQ4IDQxLjM3MyA0MS45NDA2IDM2IDM0LjUgMzZIMjkuNUMyMi4wNTk0IDM2IDE2IDQxLjM3MyAxNiA0OCIgZmlsbD0id2hpdGUiLz4KPC9zdmc, Path, Person, PersonStyle, RAGFlowAvatar, Rectangle, Responsive, Rounded, Shows, Smith, Story, StoryObj, Style, Supports, The, This, TypeScript...

---
*Generated by RAGFlow Repository Documentation Generator*
