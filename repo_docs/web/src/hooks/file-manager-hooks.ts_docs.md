# File Documentation: web/src/hooks/file-manager-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/file-manager-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 294
- **Characters**: 8,262
- **Size**: 8,265 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { ResponseType } from '@/interfaces/database/base';
import { IFolder } from '@/interfaces/database/file-manager';
import { IConnectRequestBody } from '@/interfaces/request/file-manager';
import fileManagerService from '@/services/file-manager-service';
import { downloadFileFromBlob } from '@/utils/file-util';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { PaginationProps, UploadFile } from 'antd';
import React, { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { useSearchParams } from 'umi';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import { useSetPaginationParams } from './route-hook';

export const useGetFolderId = () => {
  const [searchParams] = useSearchParams();
  const id = searchParams.get('folderId') as string;

  return id ?? '';
};

export interface IListResult {
  searchString: string;
  handleInputChange: React.ChangeEventHandler<HTMLInputElement>;
  pagination: PaginationProps;
  setPagination: (pagination: { page: number; pageSize: number }) => void;
  loading: boolean;
}

export const useFetchPureFileList = () => {
  const { mutateAsync, isPending: loading } = useMutation({
    mutationKey: ['fetchPureFileList'],
    gcTime: 0,

    mutationFn: async (parentId: string) => {
      const { data } = await fileManagerService.listFile({
        parent_id: parentId,
      });

      return data;
    },
  });

  return { loading, fetchList: mutateAsync };
};

export const useFetchFileList = (): ResponseType<any> & IListResult => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const id = useGetFolderId();

  const { data, isFetching: loading } = useQuery({
    queryKey: [
      'fetchFileList',
      {
        id,
        searchString,
        ...pagination,
      },
    ],
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const { data } = await fileManagerService.listFile({
        parent_id: id,
        keywords: searchString,
        page_size: pagination.pageSize,
        page: pagination.current,
      });

      return data;
    },
  });

  const onInputChange: React.ChangeEventHandler<HTMLInputElement> = useCallback(
    (e) => {
      setPagination({ page: 1 });
      handleInputChange(e);
    },
    [handleInputChange, setPagination],
  );

  return {
    ...data,
    searchString,
    handleInputChange: onInputChange,
    pagination: { ...pagination, total: data?.data?.total },
    setPagination,
    loading,
  };
};

export const useDeleteFile = () => {
  const { setPaginationParams } = useSetPaginationParams();
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteFile'],
    mutationFn: async (params: { fileIds: string[]; parentId: string }) => {
      const { data } = await fileManagerService.removeFile(params);
      if (data.code === 0) {
        setPaginationParams(1); // TODO: There should be a better way to paginate the request list
        queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
      }
      return data.code;
    },
  });

  return { data, loading, deleteFile: mutateAsync };
};

export const useDownloadFile = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['downloadFile'],
    mutationFn: async (params: { id: string; filename?: string }) => {
      const response = await fileManagerService.getFile({}, params.id);
      const blob = new Blob([response.data], { type: response.data.type });
      downloadFileFromBlob(blob, params.filename);
    },
  });
  return { data, loading, downloadFile: mutateAsync };
};

export const useRenameFile = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['renameFile'],
    mutationFn: async (params: { fileId: string; name: string }) => {
      const { data } = await fileManagerService.renameFile(params);
      if (data.code === 0) {
        message.success(t('message.renamed'));
        queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
      }
      return data.code;
    },
  });

  return { data, loading, renameFile: mutateAsync };
};

export const useFetchParentFolderList = (): IFolder[] => {
  const id = useGetFolderId();
  const { data } = useQuery({
    queryKey: ['fetchParentFolderList', id],
    initialData: [],
    enabled: !!id,
    queryFn: async () => {
      const { data } = await fileManagerService.getAllParentFolder({
        fileId: id,
      });

      return data?.data?.parent_folders?.toReversed() ?? [];
    },
  });

  return data;
};

export const useCreateFolder = () => {
  const { setPaginationParams } = useSetPaginationParams();
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['createFolder'],
    mutationFn: async (params: { parentId: string; name: string }) => {
      const { data } = await fileManagerService.createFolder({
        ...params,
        type: 'folder',
      });
      if (data.code === 0) {
        message.success(t('message.created'));
        setPaginationParams(1);
        queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
      }
      return data.code;
    },
  });

  return { data, loading, createFolder: mutateAsync };
};

export const useUploadFile = () => {
  const { setPaginationParams } = useSetPaginationParams();
  const { t } = useTranslation();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['uploadFile'],
    mutationFn: async (params: {
      fileList: UploadFile[];
      parentId: string;
    }) => {
      const fileList = params.fileList;
      const pathList = params.fileList.map(
        (file) => (file as any).webkitRelativePath,
      );
      const formData = new FormData();
      formData.append('parent_id', params.parentId);
      fileList.forEach((file: any, index: number) => {
        formData.append('file', file);
        formData.append('path', pathList[index]);
      });
      try {
        const ret = await fileManagerService.uploadFile(formData);
        if (ret?.data.code === 0) {
          message.success(t('message.uploaded'));
          setPaginationParams(1);
          queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
        }
        return ret?.data?.code;
      } catch (error) {
        console.log('🚀 ~ useUploadFile ~ error:', error);
      }
    },
  });

  return { data, loading, uploadFile: mutateAsync };
};

export const useConnectToKnowledge = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['connectFileToKnowledge'],
    mutationFn: async (params: IConnectRequestBody) => {
      const { data } = await fileManagerService.connectFileToKnowledge(params);
      if (data.code === 0) {
        message.success(t('message.operated'));
        queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
      }
      return data.code;
    },
  });

  return { data, loading, connectFileToKnowledge: mutateAsync };
};

export interface IMoveFileBody {
  src_file_ids: string[];
  dest_file_id: string; // target folder id
}

export const useMoveFile = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['moveFile'],
    mutationFn: async (params: IMoveFileBody) => {
      const { data } = await fileManagerService.moveFile(params);
      if (data.code === 0) {
        message.success(t('message.operated'));
        queryClient.invalidateQueries({ queryKey: ['fetchFileList'] });
      }
      return data.code;
    },
  });

  return { data, loading, moveFile: mutateAsync };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/file-manager-hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 294 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (11)

- `useGetFolderId`: Exported entity
- `useFetchPureFileList`: Exported entity
- `useFetchFileList`: Exported entity
- `useDeleteFile`: Exported entity
- `useDownloadFile`: Exported entity
- `useRenameFile`: Exported entity
- `useFetchParentFolderList`: Exported entity
- `useCreateFolder`: Exported entity
- `useUploadFile`: Exported entity
- `useConnectToKnowledge`: Exported entity
- `useMoveFile`: Exported entity

### Functions (14)

- `useGetFolderId()`: Function definition
- `id()`: Function definition
- `useFetchPureFileList()`: Function definition
- `useFetchFileList()`: Function definition
- `useDeleteFile()`: Function definition
- `useDownloadFile()`: Function definition
- `useRenameFile()`: Function definition
- `useFetchParentFolderList()`: Function definition
- `useCreateFolder()`: Function definition
- `useUploadFile()`: Function definition
- `pathList()`: Function definition
- `formData()`: Function definition
- `useConnectToKnowledge()`: Function definition
- `useMoveFile()`: Function definition

### Imports (13)

- `import message from '@/components/ui/message';`
- `import { ResponseType } from '@/interfaces/database/base';`
- `import { IFolder } from '@/interfaces/database/file-manager';`
- `import { IConnectRequestBody } from '@/interfaces/request/file-manager';`
- `import fileManagerService from '@/services/file-manager-service';`
- `import { downloadFileFromBlob } from '@/utils/file-util';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { PaginationProps, UploadFile } from 'antd';`
- `import React, { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 294
- Blank lines: 34 (11.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~260


## Dependencies and Imports

- `@/components/ui/message`
- `@/interfaces/database/base`
- `@/interfaces/database/file-manager`
- `@/interfaces/request/file-manager`
- `@/services/file-manager-service`
- `@/utils/file-util`
- `@tanstack/react-query`
- `antd`
- `react`
- `react-i18next`
- `umi`
- `./route-hook`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
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

- Other files in `web/src/hooks/` directory
- Potential test file: `test_file-manager-hooks.ts`

## Keywords

./route-hook, @/components/ui/message, @/interfaces/database/base, @/interfaces/database/file-manager, @/interfaces/request/file-manager, @/services/file-manager-service, @/utils/file-util, @tanstack/react-query, Blob, ChangeEventHandler, FormData, HTMLInputElement, IConnectRequestBody, IFolder, IListResult, IMoveFileBody, PaginationProps, React, ResponseType, TODO, There, TypeScript, UploadFile, antd, blob, fileList, formData, id, onInputChange, pathList, queryClient, react, react-i18next, response, ret, tanstack, umi, useConnectToKnowledge, useCreateFolder, useDeleteFile, useDownloadFile, useFetchFileList, useFetchParentFolderList, useFetchPureFileList, useGetFolderId, useMoveFile, useRenameFile, useUploadFile

---
*Generated by RAGFlow Repository Documentation Generator*
