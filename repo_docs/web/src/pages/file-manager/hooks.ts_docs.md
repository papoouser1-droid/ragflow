# File Documentation: web/src/pages/file-manager/hooks.ts

## File Metadata

- **Path**: `web/src/pages/file-manager/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 295
- **Characters**: 7,374
- **Size**: 7,374 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';
import {
  useConnectToKnowledge,
  useCreateFolder,
  useDeleteFile,
  useFetchParentFolderList,
  useMoveFile,
  useRenameFile,
  useUploadFile,
} from '@/hooks/file-manager-hooks';
import { IFile } from '@/interfaces/database/file-manager';
import { TableRowSelection } from 'antd/es/table/interface';
import { UploadFile } from 'antd/lib';
import { useCallback, useMemo, useState } from 'react';
import { useNavigate, useSearchParams } from 'umi';

export const useGetFolderId = () => {
  const [searchParams] = useSearchParams();
  const id = searchParams.get('folderId') as string;

  return id ?? '';
};

export const useGetRowSelection = () => {
  const [selectedRowKeys, setSelectedRowKeys] = useState<React.Key[]>([]);

  const rowSelection: TableRowSelection<IFile> = {
    selectedRowKeys,
    getCheckboxProps: (record) => {
      return { disabled: record.source_type === 'knowledgebase' };
    },
    onChange: (newSelectedRowKeys: React.Key[]) => {
      setSelectedRowKeys(newSelectedRowKeys);
    },
  };

  return { rowSelection, setSelectedRowKeys };
};

export const useNavigateToOtherFolder = () => {
  const navigate = useNavigate();
  const navigateToOtherFolder = useCallback(
    (folderId: string) => {
      navigate(`/file?folderId=${folderId}`);
    },
    [navigate],
  );

  return navigateToOtherFolder;
};

export const useRenameCurrentFile = () => {
  const [file, setFile] = useState<IFile>({} as IFile);
  const {
    visible: fileRenameVisible,
    hideModal: hideFileRenameModal,
    showModal: showFileRenameModal,
  } = useSetModalState();
  const { renameFile, loading } = useRenameFile();

  const onFileRenameOk = useCallback(
    async (name: string) => {
      const ret = await renameFile({
        fileId: file.id,
        name,
      });

      if (ret === 0) {
        hideFileRenameModal();
      }
    },
    [renameFile, file, hideFileRenameModal],
  );

  const handleShowFileRenameModal = useCallback(
    async (record: IFile) => {
      setFile(record);
      showFileRenameModal();
    },
    [showFileRenameModal],
  );

  return {
    fileRenameLoading: loading,
    initialFileName: file.name,
    onFileRenameOk,
    fileRenameVisible,
    hideFileRenameModal,
    showFileRenameModal: handleShowFileRenameModal,
  };
};

export const useSelectBreadcrumbItems = () => {
  const parentFolderList = useFetchParentFolderList();

  return parentFolderList.length === 1
    ? []
    : parentFolderList.map((x) => ({
        title: x.name === '/' ? 'root' : x.name,
        path: `/file?folderId=${x.id}`,
      }));
};

export const useHandleCreateFolder = () => {
  const {
    visible: folderCreateModalVisible,
    hideModal: hideFolderCreateModal,
    showModal: showFolderCreateModal,
  } = useSetModalState();
  const { createFolder, loading } = useCreateFolder();
  const id = useGetFolderId();

  const onFolderCreateOk = useCallback(
    async (name: string) => {
      const ret = await createFolder({ parentId: id, name });

      if (ret === 0) {
        hideFolderCreateModal();
      }
    },
    [createFolder, hideFolderCreateModal, id],
  );

  return {
    folderCreateLoading: loading,
    onFolderCreateOk,
    folderCreateModalVisible,
    hideFolderCreateModal,
    showFolderCreateModal,
  };
};

export const useHandleDeleteFile = (
  fileIds: string[],
  setSelectedRowKeys: (keys: string[]) => void,
) => {
  const { deleteFile: removeDocument } = useDeleteFile();
  const showDeleteConfirm = useShowDeleteConfirm();
  const parentId = useGetFolderId();

  const handleRemoveFile = () => {
    showDeleteConfirm({
      onOk: async () => {
        const code = await removeDocument({ fileIds, parentId });
        if (code === 0) {
          setSelectedRowKeys([]);
        }
        return;
      },
    });
  };

  return { handleRemoveFile };
};

export const useHandleUploadFile = () => {
  const {
    visible: fileUploadVisible,
    hideModal: hideFileUploadModal,
    showModal: showFileUploadModal,
  } = useSetModalState();
  const { uploadFile, loading } = useUploadFile();
  const id = useGetFolderId();

  const onFileUploadOk = useCallback(
    async (fileList: UploadFile[]): Promise<number | undefined> => {
      if (fileList.length > 0) {
        const ret: number = await uploadFile({ fileList, parentId: id });
        if (ret === 0) {
          hideFileUploadModal();
        }
        return ret;
      }
    },
    [uploadFile, hideFileUploadModal, id],
  );

  return {
    fileUploadLoading: loading,
    onFileUploadOk,
    fileUploadVisible,
    hideFileUploadModal,
    showFileUploadModal,
  };
};

export const useHandleConnectToKnowledge = () => {
  const {
    visible: connectToKnowledgeVisible,
    hideModal: hideConnectToKnowledgeModal,
    showModal: showConnectToKnowledgeModal,
  } = useSetModalState();
  const { connectFileToKnowledge: connectToKnowledge, loading } =
    useConnectToKnowledge();
  const [record, setRecord] = useState<IFile>({} as IFile);

  const initialValue = useMemo(() => {
    return Array.isArray(record?.kbs_info)
      ? record?.kbs_info?.map((x) => x.kb_id)
      : [];
  }, [record?.kbs_info]);

  const onConnectToKnowledgeOk = useCallback(
    async (knowledgeIds: string[]) => {
      const ret = await connectToKnowledge({
        fileIds: [record.id],
        kbIds: knowledgeIds,
      });

      if (ret === 0) {
        hideConnectToKnowledgeModal();
      }
      return ret;
    },
    [connectToKnowledge, hideConnectToKnowledgeModal, record.id],
  );

  const handleShowConnectToKnowledgeModal = useCallback(
    (record: IFile) => {
      setRecord(record);
      showConnectToKnowledgeModal();
    },
    [showConnectToKnowledgeModal],
  );

  return {
    initialValue,
    connectToKnowledgeLoading: loading,
    onConnectToKnowledgeOk,
    connectToKnowledgeVisible,
    hideConnectToKnowledgeModal,
    showConnectToKnowledgeModal: handleShowConnectToKnowledgeModal,
  };
};

export const useHandleBreadcrumbClick = () => {
  const navigate = useNavigate();

  const handleBreadcrumbClick = useCallback(
    (path?: string) => {
      if (path) {
        navigate(path);
      }
    },
    [navigate],
  );

  return { handleBreadcrumbClick };
};

export const useHandleMoveFile = (
  setSelectedRowKeys: (keys: string[]) => void,
) => {
  const {
    visible: moveFileVisible,
    hideModal: hideMoveFileModal,
    showModal: showMoveFileModal,
  } = useSetModalState();
  const { moveFile, loading } = useMoveFile();
  const [sourceFileIds, setSourceFileIds] = useState<string[]>([]);

  const onMoveFileOk = useCallback(
    async (targetFolderId: string) => {
      const ret = await moveFile({
        src_file_ids: sourceFileIds,
        dest_file_id: targetFolderId,
      });

      if (ret === 0) {
        setSelectedRowKeys([]);
        hideMoveFileModal();
      }
      return ret;
    },
    [moveFile, hideMoveFileModal, sourceFileIds, setSelectedRowKeys],
  );

  const handleShowMoveFileModal = useCallback(
    (ids: string[]) => {
      setSourceFileIds(ids);
      showMoveFileModal();
    },
    [showMoveFileModal],
  );

  return {
    initialValue: '',
    moveFileLoading: loading,
    onMoveFileOk,
    moveFileVisible,
    hideMoveFileModal,
    showMoveFileModal: handleShowMoveFileModal,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/file-manager/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 295 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (11)

- `useGetFolderId`: Exported entity
- `useGetRowSelection`: Exported entity
- `useNavigateToOtherFolder`: Exported entity
- `useRenameCurrentFile`: Exported entity
- `useSelectBreadcrumbItems`: Exported entity
- `useHandleCreateFolder`: Exported entity
- `useHandleDeleteFile`: Exported entity
- `useHandleUploadFile`: Exported entity
- `useHandleConnectToKnowledge`: Exported entity
- `useHandleBreadcrumbClick`: Exported entity
- `useHandleMoveFile`: Exported entity

### Functions (23)

- `useGetFolderId()`: Function definition
- `useGetRowSelection()`: Function definition
- `useNavigateToOtherFolder()`: Function definition
- `navigateToOtherFolder()`: Function definition
- `useRenameCurrentFile()`: Function definition
- `onFileRenameOk()`: Function definition
- `handleShowFileRenameModal()`: Function definition
- `useSelectBreadcrumbItems()`: Function definition
- `useHandleCreateFolder()`: Function definition
- `onFolderCreateOk()`: Function definition
- `useHandleDeleteFile()`: Function definition
- `handleRemoveFile()`: Function definition
- `useHandleUploadFile()`: Function definition
- `onFileUploadOk()`: Function definition
- `useHandleConnectToKnowledge()`: Function definition
- `initialValue()`: Function definition
- `onConnectToKnowledgeOk()`: Function definition
- `handleShowConnectToKnowledgeModal()`: Function definition
- `useHandleBreadcrumbClick()`: Function definition
- `handleBreadcrumbClick()`: Function definition

### Imports (7)

- `import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';`
- `import {`
- `import { IFile } from '@/interfaces/database/file-manager';`
- `import { TableRowSelection } from 'antd/es/table/interface';`
- `import { UploadFile } from 'antd/lib';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { useNavigate, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 295
- Blank lines: 39 (13.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~256


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/database/file-manager`
- `antd/es/table/interface`
- `antd/lib`
- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/file-manager`.

This appears to be a UI component or frontend module.

## Performance & Complexity

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

- Other files in `web/src/pages/file-manager/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/hooks/common-hooks, @/interfaces/database/file-manager, Array, IFile, Key, Promise, React, TableRowSelection, TypeScript, UploadFile, antd/es/table/interface, antd/lib, code, handleBreadcrumbClick, handleRemoveFile, handleShowConnectToKnowledgeModal, handleShowFileRenameModal, handleShowMoveFileModal, id, initialValue, navigate, navigateToOtherFolder, onConnectToKnowledgeOk, onFileRenameOk, onFileUploadOk, onFolderCreateOk, onMoveFileOk, parentFolderList, parentId, react, ret, rowSelection, showDeleteConfirm, umi, useGetFolderId, useGetRowSelection, useHandleBreadcrumbClick, useHandleConnectToKnowledge, useHandleCreateFolder, useHandleDeleteFile, useHandleMoveFile, useHandleUploadFile, useNavigateToOtherFolder, useRenameCurrentFile, useSelectBreadcrumbItems

---
*Generated by RAGFlow Repository Documentation Generator*
