# File Documentation: web/src/stories/slider-input-form-field.stories.tsx

## File Metadata

- **Path**: `web/src/stories/slider-input-form-field.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 172
- **Characters**: 3,670
- **Size**: 3,670 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
## Component Description

## Detailed Walkthrough

### Exports (3)

- `Default`: Exported entity
- `HorizontalLayout`: Exported entity
- `CustomRange`: Exported entity

### Functions (2)

- `WithFormProvider()`: Function definition
- `withFormProvider()`: Function definition

### Imports (11)

- `import { Form } from '@/components/ui/form';`
- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { useForm } from 'react-hook-form';`
- `import { SliderInputFormField } from '@/components/slider-input-form-field';`
- `import { FormLayout } from '@/constants/form';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { z } from 'zod';`
- `import { SliderInputFormField } from '@/components/slider-input-form-field';`
- `import { SliderInputFormField } from '@/components/slider-input-form-field';`
- `import { FormLayout } from '@/constants/form';`

## Code Structure Analysis

- Total lines: 172
- Blank lines: 16 (9.3%)
- Comment lines: ~7 (4.1%)
- Code lines: ~149


## Dependencies and Imports

- `@/components/ui/form`
- `@storybook/react-webpack5`
- `react-hook-form`
- `@/components/slider-input-form-field`
- `@/constants/form`
- `@hookform/resolvers/zod`
- `zod`
- `@/components/slider-input-form-field`
- `@/components/slider-input-form-field`
- `@/constants/form`
- `@/components/slider-input-form-field`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

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
- Potential test file: `test_slider-input-form-field.stories.tsx`

## Keywords

@/components/slider-input-form-field, @/components/ui/form, @/constants/form, @hookform/resolvers/zod, @storybook/react-webpack5, Basic, Component, Custom, CustomRange, Default, Description, Example, Form, FormLayout, Horizontal, HorizontalLayout, Layout, Meta, More, Range, React, ReactNode, Slider, SliderInputFormField, Story, StoryObj, TypeScript, Usage, Value, Vertical, WithFormProvider, form, hookform, meta, react-hook-form, storybook, withFormProvider, zod

---
*Generated by RAGFlow Repository Documentation Generator*
