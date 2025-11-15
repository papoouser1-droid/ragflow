# File Documentation: web/src/pages/agent/debug-content/uploader.tsx

## File Metadata

- **Path**: `web/src/pages/agent/debug-content/uploader.tsx`
- **Extension**: `.tsx`
- **Lines**: 117
- **Characters**: 3,591
- **Size**: 3,591 bytes
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
import { useUploadCanvasFile } from '@/hooks/use-agent-request';
import { Upload, X } from 'lucide-react';
import * as React from 'react';
import { toast } from 'sonner';

type FileUploadDirectUploadProps = {
  value: Record<string, any>;
  onChange(value: Record<string, any>): void;
};

export function FileUploadDirectUpload({
  onChange,
}: FileUploadDirectUploadProps) {
  const [files, setFiles] = React.useState<File[]>([]);

  const { uploadCanvasFile } = useUploadCanvasFile();

  const onUpload: NonNullable<FileUploadProps['onUpload']> = React.useCallback(
    async (files, { onSuccess, onError }) => {
      try {
        const uploadPromises = files.map(async (file) => {
          const handleError = (error?: any) => {
            onError(
              file,
              error instanceof Error ? error : new Error('Upload failed'),
            );
          };
          try {
            const ret = await uploadCanvasFile([file]);
            if (ret.code === 0) {
              onSuccess(file);
              onChange(ret.data);
            } else {
              handleError();
            }
          } catch (error) {
            handleError(error);
          }
        });

        // Wait for all uploads to complete
        await Promise.all(uploadPromises);
      } catch (error) {
        // This handles any error that might occur outside the individual upload processes
        console.error('Unexpected error during upload:', error);
      }
    },
    [onChange, uploadCanvasFile],
  );

  const onFileReject = React.useCallback((file: File, message: string) => {
    toast(message, {
      description: `"${file.name.length > 20 ? `${file.name.slice(0, 20)}...` : file.name}" has been rejected`,
    });
  }, []);

  return (
    <FileUpload
      value={files}
      onValueChange={setFiles}
      onUpload={onUpload}
      onFileReject={onFileReject}
      maxFiles={1}
      className="w-full"
      multiple={false}
    >
      <FileUploadDropzone>
        <div className="flex flex-col items-center gap-1 text-center">
          <div className="flex items-center justify-center rounded-full border p-2.5">
            <Upload className="size-6 text-muted-foreground" />
          </div>
          <p className="font-medium text-sm">Drag & drop files here</p>
          <p className="text-muted-foreground text-xs">
            Or click to browse (max 1 files)
          </p>
        </div>
        <FileUploadTrigger asChild>
          <Button variant="outline" size="sm" className="mt-2 w-fit">
            Browse files
          </Button>
        </FileUploadTrigger>
      </FileUploadDropzone>
      <FileUploadList>
        {files.map((file, index) => (
          <FileUploadItem key={index} value={file} className="flex-col">
            <div className="flex w-full items-center gap-2">
              <FileUploadItemPreview />
              <FileUploadItemMetadata />
              <FileUploadItemDelete asChild>
                <Button variant="ghost" size="icon" className="size-7">
                  <X />
                </Button>
              </FileUploadItemDelete>
            </div>
            <FileUploadItemProgress />
          </FileUploadItem>
        ))}
      </FileUploadList>
    </FileUpload>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/debug-content/uploader.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 117 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FileUploadDirectUpload`: Exported entity

### Functions (4)

- `FileUploadDirectUpload()`: Function definition
- `uploadPromises()`: Function definition
- `handleError()`: Function definition
- `onFileReject()`: Function definition

### Imports (6)

- `import {`
- `import { Button } from '@/components/ui/button';`
- `import { useUploadCanvasFile } from '@/hooks/use-agent-request';`
- `import { Upload, X } from 'lucide-react';`
- `import * as React from 'react';`
- `import { toast } from 'sonner';`

## Code Structure Analysis

- Total lines: 117
- Blank lines: 9 (7.7%)
- Comment lines: ~2 (1.7%)
- Code lines: ~106


## Dependencies and Imports

- `@/components/ui/button`
- `@/hooks/use-agent-request`
- `lucide-react`
- `react`
- `sonner`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/debug-content`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 1 loop(s) - consider algorithmic complexity
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

- Other files in `web/src/pages/agent/debug-content/` directory
- Potential test file: `test_uploader.tsx`

## Keywords

@/components/ui/button, @/hooks/use-agent-request, Browse, Button, Drag, Error, File, FileUpload, FileUploadDirectUpload, FileUploadDirectUploadProps, FileUploadDropzone, FileUploadItem, FileUploadItemDelete, FileUploadItemMetadata, FileUploadItemPreview, FileUploadItemProgress, FileUploadList, FileUploadProps, FileUploadTrigger, NonNullable, Promise, React, Record, This, TypeScript, Unexpected, Upload, Wait, handleError, lucide-react, onFileReject, onUpload, react, ret, sonner, uploadPromises

---
*Generated by RAGFlow Repository Documentation Generator*
