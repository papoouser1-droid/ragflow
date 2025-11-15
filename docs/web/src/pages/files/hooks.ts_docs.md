# Documentation: web/src/pages/files/hooks.ts

## File Metadata

- **Path**: `web/src/pages/files/hooks.ts`
- **Size**: 3764 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/files/hooks.ts`.

## Original Source Code

```ts
import { useSetModalState } from '@/hooks/common-hooks';
import {
  useConnectToKnowledge,
  useRenameFile,
} from '@/hooks/file-manager-hooks';
import { IFile } from '@/interfaces/database/file-manager';
import { TableRowSelection } from 'antd/es/table/interface';
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

export type UseRenameCurrentFileReturnType = ReturnType<
  typeof useRenameCurrentFile
>;

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
    initialConnectedIds: initialValue,
    connectToKnowledgeLoading: loading,
    onConnectToKnowledgeOk,
    connectToKnowledgeVisible,
    hideConnectToKnowledgeModal,
    showConnectToKnowledgeModal: handleShowConnectToKnowledgeModal,
  };
};

export type UseHandleConnectToKnowledgeReturnType = ReturnType<
  typeof useHandleConnectToKnowledge
>;

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

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/files/hooks.ts` is located in the `web/src/pages/files` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to files.

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

- [action-cell.tsx](action-cell.tsx_docs.md)
- [file-breadcrumb.tsx](file-breadcrumb.tsx_docs.md)
- [files-table.tsx](files-table.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [knowledge-cell.tsx](knowledge-cell.tsx_docs.md)
- [link-to-dataset-dialog.tsx](link-to-dataset-dialog.tsx_docs.md)
- [move-dialog.tsx](move-dialog.tsx_docs.md)
- [use-bulk-operate-file.tsx](use-bulk-operate-file.tsx_docs.md)
- [use-create-folder.ts](use-create-folder.ts_docs.md)
- [use-delete-file.ts](use-delete-file.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
