# File Documentation: web/src/utils/file-util.ts

## File Metadata

- **Path**: `web/src/utils/file-util.ts`
- **Extension**: `.ts`
- **Lines**: 187
- **Characters**: 4,744
- **Size**: 4,744 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { FileMimeType } from '@/constants/common';
import fileManagerService from '@/services/file-manager-service';
import { UploadFile } from 'antd';

export const transformFile2Base64 = (
  val: any,
  imgSize?: number,
): Promise<any> => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.readAsDataURL(val);
    reader.onload = (): void => {
      // Create image object
      const img = new Image();
      img.src = reader.result as string;

      img.onload = () => {
        // Create canvas
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');

        // Calculate compressed dimensions, set max width/height to 800px
        let width = img.width;
        let height = img.height;
        const maxSize = imgSize ?? 100;

        if (width > height && width > maxSize) {
          height = (height * maxSize) / width;
          width = maxSize;
        } else if (height > maxSize) {
          width = (width * maxSize) / height;
          height = maxSize;
        }

        // Set canvas dimensions
        canvas.width = width;
        canvas.height = height;

        // Draw image
        ctx?.drawImage(img, 0, 0, width, height);

        // Convert to base64, maintain original format and transparency
        const compressedBase64 = canvas.toDataURL('image/png');
        resolve(compressedBase64);
      };

      img.onerror = reject;
    };
    reader.onerror = reject;
  });
};

export const transformBase64ToFile = (
  dataUrl: string,
  filename: string = 'file',
) => {
  let arr = dataUrl.split(','),
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n);

  const mime = arr[0].match(/:(.*?);/);
  const mimeType = mime ? mime[1] : 'image/png';

  while (n--) {
    u8arr[n] = bstr.charCodeAt(n);
  }
  return new File([u8arr], filename, { type: mimeType });
};

export const normFile = (e: any) => {
  if (Array.isArray(e)) {
    return e;
  }
  return e?.fileList;
};

export const getUploadFileListFromBase64 = (avatar: string) => {
  let fileList: UploadFile[] = [];

  if (avatar) {
    fileList = [{ uid: '1', name: 'file', thumbUrl: avatar, status: 'done' }];
  }

  return fileList;
};

export const getBase64FromUploadFileList = async (fileList?: UploadFile[]) => {
  if (Array.isArray(fileList) && fileList.length > 0) {
    const file = fileList[0];
    const originFileObj = file.originFileObj;
    if (originFileObj) {
      const base64 = await transformFile2Base64(originFileObj);
      return base64;
    } else {
      return file.thumbUrl;
    }
    // return fileList[0].thumbUrl; TODO: Even JPG files will be converted to base64 parameters in png format
  }

  return '';
};

async function fetchDocumentBlob(id: string, mimeType?: FileMimeType) {
  const response = await fileManagerService.getDocumentFile({}, id);
  const blob = new Blob([response.data], {
    type: mimeType || response.data.type,
  });

  return blob;
}

export async function previewHtmlFile(id: string) {
  const blob = await fetchDocumentBlob(id, FileMimeType.Html);
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.click();
  URL.revokeObjectURL(url);
}

export const downloadFileFromBlob = (blob: Blob, name?: string) => {
  const url = window.URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  if (name) {
    a.download = name;
  }
  a.click();
  window.URL.revokeObjectURL(url);
};

export const downloadDocument = async ({
  id,
  filename,
}: {
  id: string;
  filename?: string;
}) => {
  const blob = await fetchDocumentBlob(id);
  downloadFileFromBlob(blob, filename);
};

const Units = ['bytes', 'KB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];

export const formatBytes = (x: string | number) => {
  let l = 0,
    n = (typeof x === 'string' ? parseInt(x, 10) : x) || 0;

  while (n >= 1024 && ++l) {
    n = n / 1024;
  }

  return n.toFixed(n < 10 && l > 0 ? 1 : 0) + ' ' + Units[l];
};

export const downloadJsonFile = async (
  data: Record<string, any>,
  fileName: string,
) => {
  const blob = new Blob([JSON.stringify(data)], { type: FileMimeType.Json });
  downloadFileFromBlob(blob, fileName);
};

export function transformBase64ToFileWithPreview(
  dataUrl: string,
  filename: string = 'file',
) {
  const file = transformBase64ToFile(dataUrl, filename);

  (file as any).preview = dataUrl;

  return file;
}

export const getBase64FromFileList = async (fileList?: File[]) => {
  if (Array.isArray(fileList) && fileList.length > 0) {
    const file = fileList[0];
    if (file) {
      const base64 = await transformFile2Base64(file);
      return base64;
    }
  }

  return '';
};

```

## High-Level Overview

      // Create image object

## Detailed Walkthrough

### Exports (11)

- `transformFile2Base64`: Exported entity
- `transformBase64ToFile`: Exported entity
- `normFile`: Exported entity
- `getUploadFileListFromBase64`: Exported entity
- `getBase64FromUploadFileList`: Exported entity
- `downloadFileFromBlob`: Exported entity
- `downloadDocument`: Exported entity
- `formatBytes`: Exported entity
- `downloadJsonFile`: Exported entity
- `transformBase64ToFileWithPreview`: Exported entity
- `getBase64FromFileList`: Exported entity

### Functions (13)

- `transformFile2Base64()`: Function definition
- `transformBase64ToFile()`: Function definition
- `normFile()`: Function definition
- `getUploadFileListFromBase64()`: Function definition
- `getBase64FromUploadFileList()`: Function definition
- `fetchDocumentBlob()`: Function definition
- `previewHtmlFile()`: Function definition
- `downloadFileFromBlob()`: Function definition
- `downloadDocument()`: Function definition
- `formatBytes()`: Function definition
- `downloadJsonFile()`: Function definition
- `transformBase64ToFileWithPreview()`: Function definition
- `getBase64FromFileList()`: Function definition

### Imports (3)

- `import { FileMimeType } from '@/constants/common';`
- `import fileManagerService from '@/services/file-manager-service';`
- `import { UploadFile } from 'antd';`

## Code Structure Analysis

- Total lines: 187
- Blank lines: 33 (17.6%)
- Comment lines: ~7 (3.7%)
- Code lines: ~147


## Dependencies and Imports

- `@/constants/common`
- `@/services/file-manager-service`
- `antd`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/utils`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/utils/` directory
- Potential test file: `test_file-util.ts`

## Keywords

@/constants/common, @/services/file-manager-service, Array, Blob, Calculate, Convert, Create, Draw, Even, File, FileMimeType, FileReader, Html, Image, JPG, JSON, Json, Promise, Record, Set, TODO, TypeScript, URL, Uint8Array, Units, UploadFile, a, antd, arr, base64, blob, canvas, compressedBase64, ctx, downloadDocument, downloadFileFromBlob, downloadJsonFile, fetchDocumentBlob, file, fileList, formatBytes, getBase64FromFileList, getBase64FromUploadFileList, getUploadFileListFromBase64, height, img, l, link, maxSize, mime...

---
*Generated by RAGFlow Repository Documentation Generator*
