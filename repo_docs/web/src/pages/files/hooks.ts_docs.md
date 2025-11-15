# File Documentation: web/src/pages/files/hooks.ts

## File Metadata

- **Path**: `web/src/pages/files/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 146
- **Characters**: 3,764
- **Size**: 3,764 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/files/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 146 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `useGetFolderId`: Exported entity
- `useGetRowSelection`: Exported entity
- `useRenameCurrentFile`: Exported entity
- `useHandleConnectToKnowledge`: Exported entity
- `useHandleBreadcrumbClick`: Exported entity

### Functions (11)

- `useGetFolderId()`: Function definition
- `useGetRowSelection()`: Function definition
- `useRenameCurrentFile()`: Function definition
- `onFileRenameOk()`: Function definition
- `handleShowFileRenameModal()`: Function definition
- `useHandleConnectToKnowledge()`: Function definition
- `initialValue()`: Function definition
- `onConnectToKnowledgeOk()`: Function definition
- `handleShowConnectToKnowledgeModal()`: Function definition
- `useHandleBreadcrumbClick()`: Function definition
- `handleBreadcrumbClick()`: Function definition

### Imports (6)

- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import {`
- `import { IFile } from '@/interfaces/database/file-manager';`
- `import { TableRowSelection } from 'antd/es/table/interface';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { useNavigate, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 146
- Blank lines: 22 (15.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~124


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/interfaces/database/file-manager`
- `antd/es/table/interface`
- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/files`.

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

- Other files in `web/src/pages/files/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/hooks/common-hooks, @/interfaces/database/file-manager, Array, IFile, Key, React, ReturnType, TableRowSelection, TypeScript, UseHandleConnectToKnowledgeReturnType, UseRenameCurrentFileReturnType, antd/es/table/interface, handleBreadcrumbClick, handleShowConnectToKnowledgeModal, handleShowFileRenameModal, id, initialValue, navigate, onConnectToKnowledgeOk, onFileRenameOk, react, ret, rowSelection, umi, useGetFolderId, useGetRowSelection, useHandleBreadcrumbClick, useHandleConnectToKnowledge, useRenameCurrentFile

---
*Generated by RAGFlow Repository Documentation Generator*
