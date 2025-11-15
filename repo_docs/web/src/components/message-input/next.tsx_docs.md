# File Documentation: web/src/components/message-input/next.tsx

## File Metadata

- **Path**: `web/src/components/message-input/next.tsx`
- **Extension**: `.tsx`
- **Lines**: 189
- **Characters**: 6,062
- **Size**: 6,062 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import {
  FileUpload,
  FileUploadDropzone,
  FileUploadItem,
  FileUploadItemDelete,
  FileUploadItemMetadata,
  FileUploadItemPreview,
  FileUploadItemProgress,
  FileUploadList,
  FileUploadTrigger,
  type FileUploadProps,
} from '@/components/file-upload';
import { Button } from '@/components/ui/button';
import { Textarea } from '@/components/ui/textarea';
import { cn } from '@/lib/utils';
import { t } from 'i18next';
import { CircleStop, Paperclip, Send, Upload, X } from 'lucide-react';
import * as React from 'react';
import { toast } from 'sonner';

interface IProps {
  disabled: boolean;
  value: string;
  sendDisabled: boolean;
  sendLoading: boolean;
  conversationId: string;
  uploadMethod?: string;
  isShared?: boolean;
  showUploadIcon?: boolean;
  isUploading?: boolean;
  onPressEnter(...prams: any[]): void;
  onInputChange: React.ChangeEventHandler<HTMLTextAreaElement>;
  createConversationBeforeUploadDocument?(message: string): Promise<any>;
  stopOutputMessage?(): void;
  onUpload?: NonNullable<FileUploadProps['onUpload']>;
  removeFile?(file: File): void;
}

export function NextMessageInput({
  isUploading = false,
  value,
  sendDisabled,
  sendLoading,
  disabled,
  showUploadIcon = true,
  onUpload,
  onInputChange,
  stopOutputMessage,
  onPressEnter,
  removeFile,
}: IProps) {
  const [files, setFiles] = React.useState<File[]>([]);

  const onFileReject = React.useCallback((file: File, message: string) => {
    toast(message, {
      description: `"${file.name.length > 20 ? `${file.name.slice(0, 20)}...` : file.name}" has been rejected`,
    });
  }, []);

  const submit = React.useCallback(() => {
    if (isUploading) return;
    onPressEnter();
    setFiles([]);
  }, [isUploading, onPressEnter]);

  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      submit();
    }
  };

  const onSubmit = React.useCallback(
    (event: React.FormEvent<HTMLFormElement>) => {
      event.preventDefault();
      submit();
    },
    [submit],
  );

  const handleRemoveFile = React.useCallback(
    (file: File) => () => {
      removeFile?.(file);
    },
    [removeFile],
  );

  return (
    <FileUpload
      value={files}
      onValueChange={setFiles}
      onUpload={onUpload}
      onFileReject={onFileReject}
      className="relative w-full items-center "
      disabled={isUploading || disabled}
    >
      <FileUploadDropzone
        tabIndex={-1}
        // Prevents the dropzone from triggering on click
        onClick={(event) => event.preventDefault()}
        className="absolute top-0 left-0 z-0 flex size-full items-center justify-center rounded-none border-none bg-background/50 p-0 opacity-0 backdrop-blur transition-opacity duration-200 ease-out data-[dragging]:z-10 data-[dragging]:opacity-100"
      >
        <div className="flex flex-col items-center gap-1 text-center">
          <div className="flex items-center justify-center rounded-full border p-2.5">
            <Upload className="size-6 text-muted-foreground" />
          </div>
          <p className="font-medium text-sm">Drag & drop files here</p>
          <p className="text-muted-foreground text-xs">
            Upload max 5 files each up to 5MB
          </p>
        </div>
      </FileUploadDropzone>
      <form
        onSubmit={onSubmit}
        className="relative flex w-full flex-col gap-2.5 rounded-md border border-input px-3 py-2 outline-none focus-within:ring-1 focus-within:ring-ring/50"
      >
        <FileUploadList
          orientation="horizontal"
          className="overflow-x-auto px-0 py-1"
        >
          {files.map((file, index) => (
            <FileUploadItem key={index} value={file} className="max-w-52 p-1.5">
              <FileUploadItemPreview className="size-8 [&>svg]:size-5">
                <FileUploadItemProgress variant="fill" />
              </FileUploadItemPreview>
              <FileUploadItemMetadata size="sm" />
              <FileUploadItemDelete asChild>
                <Button
                  variant="secondary"
                  size="icon"
                  className="-top-1 -right-1 absolute size-4 shrink-0 cursor-pointer rounded-full"
                  onClick={handleRemoveFile(file)}
                >
                  <X className="size-2.5" />
                </Button>
              </FileUploadItemDelete>
            </FileUploadItem>
          ))}
        </FileUploadList>
        <Textarea
          value={value}
          onChange={onInputChange}
          placeholder={t('chat.messagePlaceholder')}
          className="field-sizing-content min-h-10 w-full resize-none border-0 bg-transparent p-0 shadow-none focus-visible:ring-0 dark:bg-transparent"
          disabled={isUploading || disabled || sendLoading}
          onKeyDown={handleKeyDown}
        />
        <div
          className={cn('flex items-center justify-between gap-1.5', {
            'justify-end': !showUploadIcon,
          })}
        >
          {showUploadIcon && (
            <FileUploadTrigger asChild>
              <Button
                type="button"
                size="icon"
                variant="ghost"
                className="size-7 rounded-sm"
                disabled={isUploading || sendLoading}
              >
                <Paperclip className="size-3.5" />
                <span className="sr-only">Attach file</span>
              </Button>
            </FileUploadTrigger>
          )}
          {sendLoading ? (
            <Button onClick={stopOutputMessage} className="size-5 rounded-sm">
              <CircleStop />
            </Button>
          ) : (
            <Button
              className="size-5 rounded-sm"
              disabled={
                sendDisabled || isUploading || sendLoading || !value.trim()
              }
            >
              <Send />
              <span className="sr-only">Send message</span>
            </Button>
          )}
        </div>
      </form>
    </FileUpload>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/message-input/next.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 189 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `NextMessageInput`: Exported entity

### Functions (6)

- `NextMessageInput()`: Function definition
- `onFileReject()`: Function definition
- `submit()`: Function definition
- `handleKeyDown()`: Function definition
- `onSubmit()`: Function definition
- `handleRemoveFile()`: Function definition

### Imports (8)

- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { cn } from '@/lib/utils';`
- `import { t } from 'i18next';`
- `import { CircleStop, Paperclip, Send, Upload, X } from 'lucide-react';`
- `import * as React from 'react';`
- `import { toast } from 'sonner';`

## Code Structure Analysis

- Total lines: 189
- Blank lines: 10 (5.3%)
- Comment lines: ~1 (0.5%)
- Code lines: ~178


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/textarea`
- `@/lib/utils`
- `i18next`
- `lucide-react`
- `react`
- `sonner`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/message-input`.

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

- Other files in `web/src/components/message-input/` directory
- Potential test file: `test_next.tsx`

## Keywords

@/components/ui/button, @/components/ui/textarea, @/lib/utils, Attach, Button, ChangeEventHandler, CircleStop, Drag, Enter, File, FileUpload, FileUploadDropzone, FileUploadItem, FileUploadItemDelete, FileUploadItemMetadata, FileUploadItemPreview, FileUploadItemProgress, FileUploadList, FileUploadProps, FileUploadTrigger, FormEvent, HTMLFormElement, HTMLTextAreaElement, IProps, KeyboardEvent, NextMessageInput, NonNullable, Paperclip, Prevents, Promise, React, Send, Textarea, TypeScript, Upload, handleKeyDown, handleRemoveFile, i18next, lucide-react, onFileReject, onSubmit, react, sonner, submit

---
*Generated by RAGFlow Repository Documentation Generator*
