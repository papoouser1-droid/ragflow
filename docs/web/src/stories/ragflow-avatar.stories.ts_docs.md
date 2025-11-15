# Documentation: web/src/stories/ragflow-avatar.stories.ts

## File Metadata

- **Path**: `web/src/stories/ragflow-avatar.stories.ts`
- **Size**: 4822 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/stories/ragflow-avatar.stories.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/stories/ragflow-avatar.stories.ts` is located in the `web/src/stories` directory.

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
- [number-input.stories.ts](number-input.stories.ts_docs.md)
- [ragflow-form.stories.tsx](ragflow-form.stories.tsx_docs.md)
- [ragflow-pagination.stories.ts](ragflow-pagination.stories.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
