# Documentation: web/src/pages/file-manager/hooks.ts

## File Metadata

- **Path**: `web/src/pages/file-manager/hooks.ts`
- **Size**: 7374 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/file-manager/hooks.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/file-manager/hooks.ts` is located in the `web/src/pages/file-manager` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to file-manager.

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

- [file-toolbar.tsx](file-toolbar.tsx_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
