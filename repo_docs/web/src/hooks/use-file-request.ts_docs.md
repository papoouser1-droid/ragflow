# File Documentation: web/src/hooks/use-file-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-file-request.ts`
- **Extension**: `.ts`
- **Lines**: 232
- **Characters**: 6,536
- **Size**: 6,536 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import {
  IFetchFileListResult,
  IFolder,
} from '@/interfaces/database/file-manager';
import fileManagerService from '@/services/file-manager-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { useDebounce } from 'ahooks';
import { PaginationProps } from 'antd';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { useSearchParams } from 'umi';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import { useSetPaginationParams } from './route-hook';

export const enum FileApiAction {
  UploadFile = 'uploadFile',
  FetchFileList = 'fetchFileList',
  MoveFile = 'moveFile',
  CreateFolder = 'createFolder',
  FetchParentFolderList = 'fetchParentFolderList',
  DeleteFile = 'deleteFile',
}

export const useGetFolderId = () => {
  const [searchParams] = useSearchParams();
  const id = searchParams.get('folderId') as string;

  return id ?? '';
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
    mutationKey: [FileApiAction.UploadFile],
    mutationFn: async (params: { fileList: File[]; parentId: string }) => {
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
          queryClient.invalidateQueries({
            queryKey: [FileApiAction.FetchFileList],
          });
        }
        return ret?.data?.code;
      } catch (error) {}
    },
  });

  return { data, loading, uploadFile: mutateAsync };
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
    mutationKey: [FileApiAction.MoveFile],
    mutationFn: async (params: IMoveFileBody) => {
      const { data } = await fileManagerService.moveFile(params);
      if (data.code === 0) {
        message.success(t('message.operated'));
        queryClient.invalidateQueries({
          queryKey: [FileApiAction.FetchFileList],
        });
      }
      return data.code;
    },
  });

  return { data, loading, moveFile: mutateAsync };
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
    mutationKey: [FileApiAction.CreateFolder],
    mutationFn: async (params: { parentId: string; name: string }) => {
      const { data } = await fileManagerService.createFolder({
        ...params,
        type: 'folder',
      });
      if (data.code === 0) {
        message.success(t('message.created'));
        setPaginationParams(1);
        queryClient.invalidateQueries({
          queryKey: [FileApiAction.FetchFileList],
        });
      }
      return data.code;
    },
  });

  return { data, loading, createFolder: mutateAsync };
};

export const useFetchParentFolderList = () => {
  const id = useGetFolderId();
  const { data } = useQuery<IFolder[]>({
    queryKey: [FileApiAction.FetchParentFolderList, id],
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

export interface IListResult {
  searchString: string;
  handleInputChange: React.ChangeEventHandler<HTMLInputElement>;
  pagination: PaginationProps;
  setPagination: (pagination: { page: number; pageSize: number }) => void;
  loading: boolean;
}

export const useFetchFileList = () => {
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const id = useGetFolderId();
  const debouncedSearchString = useDebounce(searchString, { wait: 500 });

  const { data, isFetching: loading } = useQuery<IFetchFileListResult>({
    queryKey: [
      FileApiAction.FetchFileList,
      {
        id,
        debouncedSearchString,
        ...pagination,
      },
    ],
    initialData: { files: [], parent_folder: {} as IFolder, total: 0 },
    gcTime: 0,
    queryFn: async () => {
      const { data } = await fileManagerService.listFile({
        parent_id: id,
        keywords: debouncedSearchString,
        page_size: pagination.pageSize,
        page: pagination.current,
      });

      return data?.data;
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
    pagination: { ...pagination, total: data?.total },
    setPagination,
    loading,
  };
};

export const useDeleteFile = () => {
  const { setPaginationParams } = useSetPaginationParams();
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [FileApiAction.DeleteFile],
    mutationFn: async (params: { fileIds: string[]; parentId: string }) => {
      const { data } = await fileManagerService.removeFile(params);
      if (data.code === 0) {
        message.success(t('message.deleted'));
        setPaginationParams(1); // TODO: There should be a better way to paginate the request list
        queryClient.invalidateQueries({
          queryKey: [FileApiAction.FetchFileList],
        });
      }
      return data.code;
    },
  });

  return { data, loading, deleteFile: mutateAsync };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/use-file-request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 232 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (8)

- `enum`: Exported entity
- `useGetFolderId`: Exported entity
- `useUploadFile`: Exported entity
- `useMoveFile`: Exported entity
- `useCreateFolder`: Exported entity
- `useFetchParentFolderList`: Exported entity
- `useFetchFileList`: Exported entity
- `useDeleteFile`: Exported entity

### Functions (9)

- `useGetFolderId()`: Function definition
- `useUploadFile()`: Function definition
- `pathList()`: Function definition
- `formData()`: Function definition
- `useMoveFile()`: Function definition
- `useCreateFolder()`: Function definition
- `useFetchParentFolderList()`: Function definition
- `useFetchFileList()`: Function definition
- `useDeleteFile()`: Function definition

### Imports (11)

- `import message from '@/components/ui/message';`
- `import {`
- `import fileManagerService from '@/services/file-manager-service';`
- `import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';`
- `import { useDebounce } from 'ahooks';`
- `import { PaginationProps } from 'antd';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { useSearchParams } from 'umi';`
- `import {`

## Code Structure Analysis

- Total lines: 232
- Blank lines: 25 (10.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~207


## Dependencies and Imports

- `@/components/ui/message`
- `@/services/file-manager-service`
- `@tanstack/react-query`
- `ahooks`
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
- Potential test file: `test_use-file-request.ts`

## Keywords

./route-hook, @/components/ui/message, @/services/file-manager-service, @tanstack/react-query, ChangeEventHandler, CreateFolder, DeleteFile, FetchFileList, FetchParentFolderList, File, FileApiAction, FormData, HTMLInputElement, IFetchFileListResult, IFolder, IListResult, IMoveFileBody, MoveFile, PaginationProps, React, TODO, There, TypeScript, UploadFile, ahooks, antd, debouncedSearchString, enum, fileList, formData, id, onInputChange, pathList, queryClient, react, react-i18next, ret, tanstack, umi, useCreateFolder, useDeleteFile, useFetchFileList, useFetchParentFolderList, useGetFolderId, useMoveFile, useUploadFile

---
*Generated by RAGFlow Repository Documentation Generator*
