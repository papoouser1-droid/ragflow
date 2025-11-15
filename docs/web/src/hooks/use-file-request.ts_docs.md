# Documentation: web/src/hooks/use-file-request.ts

## File Metadata

- **Path**: `web/src/hooks/use-file-request.ts`
- **Size**: 6536 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/use-file-request.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/use-file-request.ts` is located in the `web/src/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [auth-hooks.ts](auth-hooks.ts_docs.md)
- [chat-hooks.ts](chat-hooks.ts_docs.md)
- [chunk-hooks.ts](chunk-hooks.ts_docs.md)
- [common-hooks.tsx](common-hooks.tsx_docs.md)
- [document-hooks.ts](document-hooks.ts_docs.md)
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
