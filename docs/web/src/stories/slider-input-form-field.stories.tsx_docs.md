# Documentation: web/src/stories/slider-input-form-field.stories.tsx

## File Metadata

- **Path**: `web/src/stories/slider-input-form-field.stories.tsx`
- **Size**: 3670 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/stories/slider-input-form-field.stories.tsx`.

## Original Source Code

```tsx
import { Form } from '@/components/ui/form';
import type { Meta, StoryObj } from '@storybook/react-webpack5';
import { useForm } from 'react-hook-form';

import { SliderInputFormField } from '@/components/slider-input-form-field';
import { FormLayout } from '@/constants/form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/SliderInputFormField',
  component: SliderInputFormField,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: `
## Component Description

SliderInputFormField is a form field component that combines a slider and a numeric input field.
It provides a user-friendly way to select numeric values within a specified range.        `,
      },
    },
  },
  tags: ['autodocs'],
  argTypes: {
    name: { control: 'text' },
    label: { control: 'text' },
    min: { control: 'number' },
    max: { control: 'number' },
    step: { control: 'number' },
    defaultValue: { control: 'number' },
    layout: {
      control: 'select',
      options: [FormLayout.Vertical, FormLayout.Horizontal],
    },
  },
  args: {
    name: 'sliderValue',
    label: 'Slider Value',
    min: 0,
    max: 100,
    step: 1,
    defaultValue: 50,
  },
} satisfies Meta<typeof SliderInputFormField>;

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
    name: 'sliderValue',
    label: 'Slider Value',
    min: 0,
    max: 100,
    step: 1,
    defaultValue: 50,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Basic Usage

\`\`\`tsx
import { SliderInputFormField } from '@/components/slider-input-form-field';

<SliderInputFormField
  name="sliderValue"
  label="Slider Value"
  min={0}
  max={100}
  step={1}
  defaultValue={50}
/>
\`\`\`
        `,
      },
    },
  },
};

export const HorizontalLayout: Story = {
  decorators: [withFormProvider],
  args: {
    name: 'horizontalSlider',
    label: 'Horizontal Slider',
    min: 0,
    max: 200,
    step: 5,
    defaultValue: 100,
    layout: FormLayout.Horizontal,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Horizontal Layout

\`\`\`tsx
import { SliderInputFormField } from '@/components/slider-input-form-field';
import { FormLayout } from '@/constants/form';

<SliderInputFormField
  name="horizontalSlider"
  label="Horizontal Slider"
  min={0}
  max={200}
  step={5}
  defaultValue={100}
  layout={FormLayout.Horizontal}
/>
\`\`\`
        `,
      },
    },
  },
};

export const CustomRange: Story = {
  decorators: [withFormProvider],
  args: {
    name: 'customRange',
    label: 'Custom Range (0-1000)',
    min: 0,
    max: 1000,
    step: 10,
    defaultValue: 500,
  },
  parameters: {
    docs: {
      description: {
        story: `
### Custom Range

\`\`\`tsx
import { SliderInputFormField } from '@/components/slider-input-form-field';

<SliderInputFormField
  name="customRange"
  label="Custom Range (0-1000)"
  min={0}
  max={1000}
  step={10}
  defaultValue={500}
/>
\`\`\`
        `,
      },
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/stories/slider-input-form-field.stories.tsx` is located in the `web/src/stories` directory.

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
- [ragflow-avatar.stories.ts](ragflow-avatar.stories.ts_docs.md)
- [ragflow-form.stories.tsx](ragflow-form.stories.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
