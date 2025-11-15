# File Documentation: web/src/pages/agent/canvas/node/note-node/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/note-node/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 116
- **Characters**: 3,623
- **Size**: 3,623 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { NodeProps, NodeResizeControl } from '@xyflow/react';

import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { INoteNode } from '@/interfaces/database/flow';
import { zodResolver } from '@hookform/resolvers/zod';
import { NotebookPen } from 'lucide-react';
import { memo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { NodeWrapper } from '../node-wrapper';
import { ResizeIcon, controlStyle } from '../resize-icon';
import { useWatchFormChange, useWatchNameFormChange } from './use-watch-change';

const FormSchema = z.object({
  text: z.string(),
});

const NameFormSchema = z.object({
  name: z.string(),
});

type NoteNodeProps = NodeProps<INoteNode> & {
  useWatchNoteFormChange?: typeof useWatchFormChange;
  useWatchNoteNameFormChange?: typeof useWatchNameFormChange;
};

function NoteNode({
  data,
  id,
  selected,
  useWatchNoteFormChange,
  useWatchNoteNameFormChange,
}: NoteNodeProps) {
  const { t } = useTranslation();

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: data.form,
  });

  const nameForm = useForm<z.infer<typeof NameFormSchema>>({
    resolver: zodResolver(NameFormSchema),
    defaultValues: { name: data.name },
  });

  (useWatchNoteFormChange || useWatchFormChange)(id, form);

  (useWatchNoteNameFormChange || useWatchNameFormChange)(id, nameForm);

  return (
    <NodeWrapper
      className="p-0  w-full h-full flex flex-col bg-bg-component border border-state-warning rounded-lg shadow-md pb-1"
      selected={selected}
    >
      <NodeResizeControl minWidth={190} minHeight={128} style={controlStyle}>
        <ResizeIcon />
      </NodeResizeControl>
      <section className="px-2 py-1 flex gap-2 items-center note-drag-handle rounded-t border-t-2 border-state-warning">
        <NotebookPen className="size-4" />
        <Form {...nameForm}>
          <form className="flex-1">
            <FormField
              control={nameForm.control}
              name="name"
              render={({ field }) => (
                <FormItem className="h-full">
                  <FormControl>
                    <Input
                      placeholder={t('flow.notePlaceholder')}
                      {...field}
                      type="text"
                      className="bg-transparent border-none focus-visible:outline focus-visible:outline-text-sub-title p-1"
                    />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          </form>
        </Form>
      </section>
      <Form {...form}>
        <form className="flex-1 px-1 min-h-1">
          <FormField
            control={form.control}
            name="text"
            render={({ field }) => (
              <FormItem className="h-full">
                <FormControl>
                  <Textarea
                    placeholder={t('flow.notePlaceholder')}
                    className="resize-none rounded-none p-1 py-0 overflow-auto bg-transparent focus-visible:ring-0 border-none text-text-secondary focus-visible:ring-offset-0 !text-xs"
                    {...field}
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        </form>
      </Form>
    </NodeWrapper>
  );
}

export default memo(NoteNode);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/canvas/node/note-node/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 116 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `NoteNode()`: Function definition

### Imports (14)

- `import { NodeProps, NodeResizeControl } from '@xyflow/react';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { INoteNode } from '@/interfaces/database/flow';`
- `import { zodResolver } from '@hookform/resolvers/zod';`
- `import { NotebookPen } from 'lucide-react';`
- `import { memo } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 116
- Blank lines: 12 (10.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~104


## Dependencies and Imports

- `@xyflow/react`
- `@/components/ui/input`
- `@/components/ui/textarea`
- `@/interfaces/database/flow`
- `@hookform/resolvers/zod`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../node-wrapper`
- `../resize-icon`
- `./use-watch-change`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/canvas/node/note-node`.

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

- Other files in `web/src/pages/agent/canvas/node/note-node/` directory
- Potential test file: `test_index.tsx`

## Keywords

../node-wrapper, ../resize-icon, ./use-watch-change, @/components/ui/input, @/components/ui/textarea, @/interfaces/database/flow, @hookform/resolvers/zod, @xyflow/react, Form, FormControl, FormField, FormItem, FormMessage, FormSchema, INoteNode, Input, NameFormSchema, NodeProps, NodeResizeControl, NodeWrapper, NoteNode, NoteNodeProps, NotebookPen, ResizeIcon, Textarea, TypeScript, form, hookform, lucide-react, nameForm, react, react-hook-form, react-i18next, xyflow, zod

---
*Generated by RAGFlow Repository Documentation Generator*
