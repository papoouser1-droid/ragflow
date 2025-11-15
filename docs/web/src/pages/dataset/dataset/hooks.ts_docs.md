# Documentation: web/src/pages/dataset/dataset/hooks.ts

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/hooks.ts`
- **Size**: 3217 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset/hooks.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset/hooks.ts` is located in the `web/src/pages/dataset/dataset` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to dataset.

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

- [constant.ts](constant.ts_docs.md)
- [dataset-action-cell.tsx](dataset-action-cell.tsx_docs.md)
- [dataset-table.tsx](dataset-table.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [parsing-card.tsx](parsing-card.tsx_docs.md)
- [parsing-status-cell.tsx](parsing-status-cell.tsx_docs.md)
- [set-meta-dialog.tsx](set-meta-dialog.tsx_docs.md)
- [use-bulk-operate-dataset.tsx](use-bulk-operate-dataset.tsx_docs.md)
- [use-change-document-parser.ts](use-change-document-parser.ts_docs.md)
- [use-create-empty-document.ts](use-create-empty-document.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
