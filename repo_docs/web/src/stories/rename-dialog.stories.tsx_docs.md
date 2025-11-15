# File Documentation: web/src/stories/rename-dialog.stories.tsx

## File Metadata

- **Path**: `web/src/stories/rename-dialog.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 259
- **Characters**: 6,304
- **Size**: 6,304 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import type { Meta, StoryObj } from '@storybook/react-webpack5';
import { useState } from 'react';
import { fn } from 'storybook/test';

import { RenameDialog } from '@/components/rename-dialog';
import { Button } from '@/components/ui/button';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/RenameDialog',
  component: RenameDialog,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## Component Description

RenameDialog is a modal dialog component for renaming items. It provides a form with input validation and loading states, commonly used in chat applications for renaming conversations or creating new ones.

### Features
- Modal dialog with form input
- Loading state support
- Customizable title
- Initial name pre-filling
- Form validation and submission

### Import Path
\`\`\`tsx
import { RenameDialog } from '@/components/rename-dialog';
\`\`\`

### Basic Usage
\`\`\`tsx
import { RenameDialog } from '@/components/rename-dialog';
import { Button } from '@/components/ui/button';
import { useState } from 'react';

function MyComponent() {
  const [visible, setVisible] = useState(false);
  const [loading, setLoading] = useState(false);
  
  return (
    <div>
      <Button onClick={() => setVisible(true)}>
        Open Rename Dialog
      </Button>
      {visible && (
        <RenameDialog
          hideModal={() => setVisible(false)}
          onOk={async (name) => {
            setLoading(true);
            // Handle save logic
            console.log('New name:', name);
            setLoading(false);
            setVisible(false);
          }}
          initialName=""
          loading={loading}
        />
      )}
    </div>
  );
}
\`\`\`
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ['autodocs'],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    initialName: {
      control: 'text',
      description: 'Initial name value for the input field',
    },
    title: {
      control: 'text',
      description: 'Custom title for the dialog',
    },
    loading: {
      control: 'boolean',
      description: 'Loading state of the save button',
    },
  },
  // Use `fn` to spy on the args, which will appear in the actions panel once invoked: https://storybook.js.org/docs/essentials/actions#action-args
  args: {
    hideModal: fn(),
    onOk: fn(),
  },
} satisfies Meta<typeof RenameDialog>;

export default meta;
type Story = StoryObj<typeof meta>;

// Story components to handle useState hooks
const DefaultStoryComponent = (args: any) => {
  const [visible, setVisible] = useState(false);

  return (
    <div>
      <Button onClick={() => setVisible(true)}>Open Rename Dialog</Button>
      {visible && (
        <RenameDialog
          {...args}
          hideModal={() => setVisible(false)}
          onOk={(name) => {
            args.onOk?.(name);
            setVisible(false);
          }}
        />
      )}
    </div>
  );
};

const WithInitialNameStoryComponent = (args: any) => {
  const [visible, setVisible] = useState(false);

  return (
    <div>
      <Button onClick={() => setVisible(true)}>
        Open Rename Dialog (with initial name)
      </Button>
      {visible && (
        <RenameDialog
          {...args}
          hideModal={() => setVisible(false)}
          onOk={(name) => {
            args.onOk?.(name);
            setVisible(false);
          }}
        />
      )}
    </div>
  );
};

const CreateNewChatStoryComponent = (args: any) => {
  const [visible, setVisible] = useState(false);

  return (
    <div>
      <Button onClick={() => setVisible(true)}>Create New Chat</Button>
      {visible && (
        <RenameDialog
          {...args}
          hideModal={() => setVisible(false)}
          onOk={(name) => {
            args.onOk?.(name);
            setVisible(false);
          }}
        />
      )}
    </div>
  );
};

const LoadingStateStoryComponent = (args: any) => {
  const [visible, setVisible] = useState(false);

  return (
    <div>
      <Button onClick={() => setVisible(true)}>
        Open Dialog (Loading State)
      </Button>
      {visible && (
        <RenameDialog
          {...args}
          hideModal={() => setVisible(false)}
          onOk={(name) => {
            args.onOk?.(name);
          }}
        />
      )}
    </div>
  );
};

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Default: Story = {
  render: (args) => <DefaultStoryComponent {...args} />,
  args: {
    initialName: '',
    loading: false,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Default Rename Dialog

Basic rename dialog without initial name value. Click the button to open the dialog.
        `,
      },
    },
  },
};

export const WithInitialName: Story = {
  render: (args) => <WithInitialNameStoryComponent {...args} />,
  args: {
    initialName: 'My Chat Session',
    loading: false,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Rename Dialog with Initial Name

Rename dialog pre-filled with an existing name for editing. Click the button to open the dialog.
        `,
      },
    },
  },
};

export const CreateNewChat: Story = {
  render: (args) => <CreateNewChatStoryComponent {...args} />,
  args: {
    initialName: '',
    title: 'Create Chat',
    loading: false,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Create New Chat Dialog

Dialog for creating a new chat with custom title. Click the button to open the dialog.
        `,
      },
    },
  },
};

export const LoadingState: Story = {
  render: (args) => <LoadingStateStoryComponent {...args} />,
  args: {
    initialName: 'Saving changes...',
    loading: true,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Loading State

Dialog showing loading state during save operation. The dialog remains open while loading.
        `,
      },
    },
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
- `WithInitialName`: Exported entity
- `CreateNewChat`: Exported entity
- `LoadingState`: Exported entity

### Functions (5)

- `MyComponent()`: Function definition
- `DefaultStoryComponent()`: Function definition
- `WithInitialNameStoryComponent()`: Function definition
- `CreateNewChatStoryComponent()`: Function definition
- `LoadingStateStoryComponent()`: Function definition

### Imports (9)

- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { useState } from 'react';`
- `import { fn } from 'storybook/test';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import { useState } from 'react';`

## Code Structure Analysis

- Total lines: 259
- Blank lines: 26 (10.0%)
- Comment lines: ~16 (6.2%)
- Code lines: ~217


## Dependencies and Imports

- `@storybook/react-webpack5`
- `react`
- `storybook/test`
- `@/components/rename-dialog`
- `@/components/ui/button`
- `@/components/rename-dialog`
- `@/components/rename-dialog`
- `@/components/ui/button`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 7 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

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
- Potential test file: `test_rename-dialog.stories.tsx`

## Keywords

@/components/rename-dialog, @/components/ui/button, @storybook/react-webpack5, Autodocs, Basic, Button, Canvas, Chat, Click, Component, Create, CreateNewChat, CreateNewChatStoryComponent, Custom, Customizable, Default, DefaultStoryComponent, Description, Dialog, Example, Features, Form, Handle, Import, Initial, Loading, LoadingState, LoadingStateStoryComponent, Meta, Modal, More, MyComponent, Name, New, Open, Optional, Path, Rename, RenameDialog, Saving, Session, State, Story, StoryObj, The, This, TypeScript, Usage, Use, WithInitialName...

---
*Generated by RAGFlow Repository Documentation Generator*
