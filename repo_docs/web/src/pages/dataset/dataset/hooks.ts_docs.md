# File Documentation: web/src/pages/dataset/dataset/hooks.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 106
- **Characters**: 3,217
- **Size**: 3,217 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState } from '@/hooks/common-hooks';
import { useNextWebCrawl } from '@/hooks/document-hooks';
import { useGetKnowledgeSearchParams } from '@/hooks/route-hook';
import { IDocumentInfo } from '@/interfaces/database/document';
import { formatDate, formatSecondsToHumanReadable } from '@/utils/date';
import { formatBytes } from '@/utils/file-util';
import { useCallback, useMemo, useState } from 'react';
import { useNavigate } from 'umi';
import { ILogInfo } from '../process-log-modal';
import { RunningStatus } from './constant';

export const useNavigateToOtherPage = () => {
  const navigate = useNavigate();
  const { knowledgeId } = useGetKnowledgeSearchParams();

  const linkToUploadPage = useCallback(() => {
    navigate(`/knowledge/dataset/upload?id=${knowledgeId}`);
  }, [navigate, knowledgeId]);

  const toChunk = useCallback((id: string) => {}, []);

  return { linkToUploadPage, toChunk };
};

export const useGetRowSelection = () => {
  const [selectedRowKeys, setSelectedRowKeys] = useState<React.Key[]>([]);

  const rowSelection = {
    selectedRowKeys,
    onChange: (newSelectedRowKeys: React.Key[]) => {
      setSelectedRowKeys(newSelectedRowKeys);
    },
  };

  return rowSelection;
};

export const useHandleWebCrawl = () => {
  const {
    visible: webCrawlUploadVisible,
    hideModal: hideWebCrawlUploadModal,
    showModal: showWebCrawlUploadModal,
  } = useSetModalState();
  const { webCrawl, loading } = useNextWebCrawl();

  const onWebCrawlUploadOk = useCallback(
    async (name: string, url: string) => {
      const ret = await webCrawl({ name, url });
      if (ret === 0) {
        hideWebCrawlUploadModal();
        return 0;
      }
      return -1;
    },
    [webCrawl, hideWebCrawlUploadModal],
  );

  return {
    webCrawlUploadLoading: loading,
    onWebCrawlUploadOk,
    webCrawlUploadVisible,
    hideWebCrawlUploadModal,
    showWebCrawlUploadModal,
  };
};

export const useShowLog = (documents: IDocumentInfo[]) => {
  const { showModal, hideModal, visible } = useSetModalState();
  const [record, setRecord] = useState<IDocumentInfo>();
  const logInfo = useMemo(() => {
    const findRecord = documents.find(
      (item: IDocumentInfo) => item.id === record?.id,
    );
    let log: ILogInfo = {
      taskId: record?.id,
      fileName: record?.name || '-',
      details: record?.progress_msg || '-',
    };
    if (findRecord) {
      log = {
        fileType: findRecord?.suffix,
        uploadedBy: findRecord?.nickname,
        fileName: findRecord?.name,
        uploadDate: formatDate(findRecord.create_date),
        fileSize: formatBytes(findRecord.size || 0),
        processBeginAt: formatDate(findRecord.process_begin_at),
        chunkNumber: findRecord.chunk_num,
        duration: formatSecondsToHumanReadable(
          findRecord.process_duration || 0,
        ),
        status: findRecord.run as RunningStatus,
        details: findRecord.progress_msg,
      };
    }
    return log;
  }, [record, documents]);
  const showLog = useCallback(
    (data: IDocumentInfo) => {
      setRecord(data);
      showModal();
    },
    [showModal],
  );
  return { showLog, hideLog: hideModal, logVisible: visible, logInfo };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 106 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `useNavigateToOtherPage`: Exported entity
- `useGetRowSelection`: Exported entity
- `useHandleWebCrawl`: Exported entity
- `useShowLog`: Exported entity

### Functions (11)

- `useNavigateToOtherPage()`: Function definition
- `linkToUploadPage()`: Function definition
- `toChunk()`: Function definition
- `useGetRowSelection()`: Function definition
- `rowSelection()`: Function definition
- `useHandleWebCrawl()`: Function definition
- `onWebCrawlUploadOk()`: Function definition
- `useShowLog()`: Function definition
- `logInfo()`: Function definition
- `findRecord()`: Function definition
- `showLog()`: Function definition

### Imports (10)

- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useNextWebCrawl } from '@/hooks/document-hooks';`
- `import { useGetKnowledgeSearchParams } from '@/hooks/route-hook';`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import { formatDate, formatSecondsToHumanReadable } from '@/utils/date';`
- `import { formatBytes } from '@/utils/file-util';`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { useNavigate } from 'umi';`
- `import { ILogInfo } from '../process-log-modal';`
- `import { RunningStatus } from './constant';`

## Code Structure Analysis

- Total lines: 106
- Blank lines: 12 (11.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~94


## Dependencies and Imports

- `@/hooks/common-hooks`
- `@/hooks/document-hooks`
- `@/hooks/route-hook`
- `@/interfaces/database/document`
- `@/utils/date`
- `@/utils/file-util`
- `react`
- `umi`
- `../process-log-modal`
- `./constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset/` directory
- Potential test file: `test_hooks.ts`

## Keywords

../process-log-modal, ./constant, @/hooks/common-hooks, @/hooks/document-hooks, @/hooks/route-hook, @/interfaces/database/document, @/utils/date, @/utils/file-util, IDocumentInfo, ILogInfo, Key, React, RunningStatus, TypeScript, findRecord, linkToUploadPage, log, logInfo, navigate, onWebCrawlUploadOk, react, ret, rowSelection, showLog, toChunk, umi, useGetRowSelection, useHandleWebCrawl, useNavigateToOtherPage, useShowLog

---
*Generated by RAGFlow Repository Documentation Generator*
