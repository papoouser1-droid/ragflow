# Documentation: web/src/pages/agent/canvas/node/note-node/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/canvas/node/note-node/index.tsx`
- **Size**: 3623 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/canvas/node/note-node/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/canvas/node/note-node/index.tsx` is located in the `web/src/pages/agent/canvas/node/note-node` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to note-node.

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

- [use-watch-change.ts](use-watch-change.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
