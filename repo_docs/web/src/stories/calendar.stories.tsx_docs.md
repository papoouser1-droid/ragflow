# File Documentation: web/src/stories/calendar.stories.tsx

## File Metadata

- **Path**: `web/src/stories/calendar.stories.tsx`
- **Extension**: `.tsx`
- **Lines**: 297
- **Characters**: 6,978
- **Size**: 6,978 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Calendar } from '@/components/originui/calendar';
import type { Meta, StoryObj } from '@storybook/react-webpack5';
import { useState } from 'react';

// More on how to set up stories at: https://storybook.js.org/docs/writing-stories#default-export
const meta = {
  title: 'Example/Calendar',
  component: Calendar,
  parameters: {
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
    layout: 'centered',
    docs: {
      description: {
        component: `
## Calendar Component

Calendar is a date picker component based on react-day-picker that allows users to select dates or date ranges. It provides a clean and customizable interface for date selection with support for various customization options.

### Import Path
\`\`\`typescript
import { Calendar } from '@/components/originui/calendar';
\`\`\`

### Basic Usage
\`\`\`tsx
import { Calendar } from '@/components/originui/calendar';
import { useState } from 'react';

function MyComponent() {
  const [date, setDate] = useState<Date | undefined>(new Date());
  
  return (
    <Calendar
      mode="single"
      selected={date}
      onSelect={setDate}
      className="rounded-md border"
    />
  );
}
\`\`\`

### Features
- Single date selection
- Date range selection
- Customizable styling with className prop
- Navigation between months
- Today highlighting
- Disabled dates support
- Customizable components
- Built with Tailwind CSS
        `,
      },
    },
  },
  // This component will have an automatically generated Autodocs entry: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ['autodocs'],
  // More on argTypes: https://storybook.js.org/docs/api/argtypes
  argTypes: {
    mode: {
      description: 'Selection mode - single date or range',
      control: { type: 'radio' },
      options: ['single', 'range'],
    },
    selected: {
      description: 'Selected date or date range',
      control: false,
    },
    onSelect: {
      description: 'Callback function when date is selected',
      control: false,
    },
    className: {
      description: 'Additional CSS classes for styling',
      control: { type: 'text' },
    },
    classNames: {
      description: 'Custom class names for internal elements',
      control: { type: 'object' },
    },
    showOutsideDays: {
      description: 'Whether to show outside days',
      control: { type: 'boolean' },
    },
    components: {
      description: 'Custom components for calendar elements',
      control: { type: 'object' },
    },
  },
} satisfies Meta<typeof Calendar>;

export default meta;
type Story = StoryObj<typeof meta>;

// More on writing stories with args: https://storybook.js.org/docs/writing-stories/args
export const Default: Story = {
  args: {
    showOutsideDays: true,
    className: 'rounded-md border',
  },
  render: () => {
    // eslint-disable-next-line react-hooks/rules-of-hooks
    const [date, setDate] = useState<Date | undefined>(new Date());

    return (
      <Calendar
        mode="single"
        selected={date}
        onSelect={setDate}
        showOutsideDays={true}
        className="rounded-md border"
      />
    );
  },
  parameters: {
    docs: {
      description: {
        story: `
### Default Calendar

Shows the basic calendar with single date selection mode.

\`\`\`tsx
const [date, setDate] = useState<Date | undefined>(new Date());

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-md border"
  showOutsideDays={true}
/>
\`\`\`
        `,
      },
    },
  },
};

export const RangeSelection: Story = {
  args: {
    showOutsideDays: true,
    className: 'rounded-md border',
  },
  render: () => {
    // eslint-disable-next-line react-hooks/rules-of-hooks
    const [range, setRange] = useState<{
      from: Date | undefined;
      to?: Date | undefined;
    }>({
      from: new Date(),
      to: undefined,
    });

    return (
      <Calendar
        mode="range"
        selected={range}
        onSelect={(range) =>
          setRange(range as { from: Date | undefined; to?: Date | undefined })
        }
        showOutsideDays={true}
        className="rounded-md border"
      />
    );
  },
  parameters: {
    docs: {
      description: {
        story: `
### Range Selection Calendar

Shows the calendar with date range selection mode.

\`\`\`tsx
const [range, setRange] = useState<{ from: Date | undefined; to?: Date | undefined }>({
  from: new Date(),
  to: undefined,
});

<Calendar
  mode="range"
  selected={range}
  onSelect={(date) => {
    if (!range.from) {
      setRange({ from: date });
    } else if (!range.to && date && date > range.from) {
      setRange({ from: range.from, to: date });
    } else {
      setRange({ from: date });
    }
  }}
  className="rounded-md border"
  showOutsideDays={true}
/>
\`\`\`
        `,
      },
    },
  },
};

export const WithoutOutsideDays: Story = {
  args: {
    showOutsideDays: false,
    className: 'rounded-md border',
  },
  render: () => {
    // eslint-disable-next-line react-hooks/rules-of-hooks
    const [date, setDate] = useState<Date | undefined>(new Date());

    return (
      <Calendar
        mode="single"
        selected={date}
        onSelect={setDate}
        showOutsideDays={false}
        className="rounded-md border"
      />
    );
  },
  parameters: {
    docs: {
      description: {
        story: `
### Calendar without Outside Days

Shows the calendar without displaying days from previous/next months.

\`\`\`tsx
const [date, setDate] = useState<Date | undefined>(new Date());

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-md border"
  showOutsideDays={false}
/>
\`\`\`
        `,
      },
    },
  },
};

export const CustomStyling: Story = {
  args: {
    showOutsideDays: true,
  },
  render: () => {
    // eslint-disable-next-line react-hooks/rules-of-hooks
    const [date, setDate] = useState<Date | undefined>(new Date());

    return (
      <Calendar
        mode="single"
        selected={date}
        onSelect={setDate}
        showOutsideDays={true}
        className="rounded-md border-2 border-primary bg-secondary"
        classNames={{
          caption_label: 'text-lg font-bold text-primary',
          day_button: 'size-10 rounded-full hover:bg-primary/20',
        }}
      />
    );
  },
  parameters: {
    docs: {
      description: {
        story: `
### Custom Styled Calendar

Shows the calendar with custom styling using className and classNames props.

\`\`\`tsx
const [date, setDate] = useState<Date | undefined>(new Date());

<Calendar
  mode="single"
  selected={date}
  onSelect={setDate}
  className="rounded-md border-2 border-primary bg-secondary"
  classNames={{
    caption_label: 'text-lg font-bold text-primary',
    day_button: 'size-10 rounded-full hover:bg-primary/20',
  }}
  showOutsideDays={true}
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
    // Optional parameter to center the component in the Canvas. More info: https://storybook.js.org/docs/configure/story-layout
## Calendar Component

## Detailed Walkthrough

### Exports (4)

- `Default`: Exported entity
- `RangeSelection`: Exported entity
- `WithoutOutsideDays`: Exported entity
- `CustomStyling`: Exported entity

### Functions (2)

- `MyComponent()`: Function definition
- `when()`: Function definition

### Imports (6)

- `import { Calendar } from '@/components/originui/calendar';`
- `import type { Meta, StoryObj } from '@storybook/react-webpack5';`
- `import { useState } from 'react';`
- `import { Calendar } from '@/components/originui/calendar';`
- `import { Calendar } from '@/components/originui/calendar';`
- `import { useState } from 'react';`

## Code Structure Analysis

- Total lines: 297
- Blank lines: 29 (9.8%)
- Comment lines: ~17 (5.7%)
- Code lines: ~251


## Dependencies and Imports

- `@/components/originui/calendar`
- `@storybook/react-webpack5`
- `react`
- `@/components/originui/calendar`
- `@/components/originui/calendar`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/stories`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 5 loop(s) - consider algorithmic complexity

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
- Potential test file: `test_calendar.stories.tsx`

## Keywords

@/components/originui/calendar, @storybook/react-webpack5, Additional, Autodocs, Basic, Built, CSS, Calendar, Callback, Canvas, Component, Custom, CustomStyling, Customizable, Date, Days, Default, Disabled, Example, Features, Import, Meta, More, MyComponent, Navigation, Optional, Outside, Path, Range, RangeSelection, Selected, Selection, Shows, Single, Story, StoryObj, Styled, Tailwind, This, Today, TypeScript, Usage, Whether, WithoutOutsideDays, for, meta, names, react, storybook, when

---
*Generated by RAGFlow Repository Documentation Generator*
