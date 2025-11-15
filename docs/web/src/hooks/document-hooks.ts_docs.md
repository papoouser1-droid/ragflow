# Documentation: web/src/hooks/document-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/document-hooks.ts`
- **Size**: 13291 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/document-hooks.ts`.

## Original Source Code

```ts
import { IReferenceChunk } from '@/interfaces/database/chat';
import { IDocumentInfo } from '@/interfaces/database/document';
import { IChunk } from '@/interfaces/database/knowledge';
import {
  IChangeParserConfigRequestBody,
  IDocumentMetaRequestBody,
} from '@/interfaces/request/document';
import i18n from '@/locales/config';
import chatService from '@/services/chat-service';
import kbService, { listDocument } from '@/services/knowledge-service';
import api, { api_host } from '@/utils/api';
import { buildChunkHighlights } from '@/utils/document-util';
import { post } from '@/utils/request';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { UploadFile, message } from 'antd';
import { get } from 'lodash';
import { useCallback, useMemo, useState } from 'react';
import { IHighlight } from 'react-pdf-highlighter';
import { useParams } from 'umi';
import {
  useGetPaginationWithRouter,
  useHandleSearchChange,
} from './logic-hooks';
import {
  useGetKnowledgeSearchParams,
  useSetPaginationParams,
} from './route-hook';

export const useGetDocumentUrl = (documentId?: string) => {
  const getDocumentUrl = useCallback(
    (id?: string) => {
      return `${api_host}/document/get/${documentId || id}`;
    },
    [documentId],
  );

  return getDocumentUrl;
};

export const useGetChunkHighlights = (
  selectedChunk: IChunk | IReferenceChunk,
) => {
  const [size, setSize] = useState({ width: 849, height: 1200 });

  const highlights: IHighlight[] = useMemo(() => {
    return buildChunkHighlights(selectedChunk, size);
  }, [selectedChunk, size]);

  const setWidthAndHeight = (width: number, height: number) => {
    setSize((pre) => {
      if (pre.height !== height || pre.width !== width) {
        return { height, width };
      }
      return pre;
    });
  };

  return { highlights, setWidthAndHeight };
};

export const useFetchNextDocumentList = () => {
  const { knowledgeId } = useGetKnowledgeSearchParams();
  const { searchString, handleInputChange } = useHandleSearchChange();
  const { pagination, setPagination } = useGetPaginationWithRouter();
  const { id } = useParams();

  const { data, isFetching: loading } = useQuery<{
    docs: IDocumentInfo[];
    total: number;
  }>({
    queryKey: ['fetchDocumentList', searchString, pagination],
    initialData: { docs: [], total: 0 },
    refetchInterval: 15000,
    enabled: !!knowledgeId || !!id,
    queryFn: async () => {
      const ret = await listDocument({
        kb_id: knowledgeId || id,
        keywords: searchString,
        page_size: pagination.pageSize,
        page: pagination.current,
      });
      if (ret.data.code === 0) {
        return ret.data.data;
      }

      return {
        docs: [],
        total: 0,
      };
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
    loading,
    searchString,
    documents: data.docs,
    pagination: { ...pagination, total: data?.total },
    handleInputChange: onInputChange,
    setPagination,
  };
};

export const useSetNextDocumentStatus = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['updateDocumentStatus'],
    mutationFn: async ({
      status,
      documentId,
    }: {
      status: boolean;
      documentId: string | string[];
    }) => {
      const ids = Array.isArray(documentId) ? documentId : [documentId];
      const { data } = await kbService.document_change_status({
        doc_ids: ids,
        status: Number(status),
      });
      if (data.code === 0) {
        message.success(i18n.t('message.modified'));
        queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
      }
      return data;
    },
  });

  return { setDocumentStatus: mutateAsync, data, loading };
};

export const useSaveNextDocumentName = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['saveDocumentName'],
    mutationFn: async ({
      name,
      documentId,
    }: {
      name: string;
      documentId: string;
    }) => {
      const { data } = await kbService.document_rename({
        doc_id: documentId,
        name: name,
      });
      if (data.code === 0) {
        message.success(i18n.t('message.renamed'));
        queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
      }
      return data.code;
    },
  });

  return { loading, saveName: mutateAsync, data };
};

export const useCreateNextDocument = () => {
  const { knowledgeId } = useGetKnowledgeSearchParams();
  const { setPaginationParams, page } = useSetPaginationParams();
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['createDocument'],
    mutationFn: async (name: string) => {
      const { data } = await kbService.document_create({
        name,
        kb_id: knowledgeId,
      });
      if (data.code === 0) {
        if (page === 1) {
          queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
        } else {
          setPaginationParams(); // fetch document list
        }

        message.success(i18n.t('message.created'));
      }
      return data.code;
    },
  });

  return { createDocument: mutateAsync, loading, data };
};

export const useSetNextDocumentParser = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['setDocumentParser'],
    mutationFn: async ({
      parserId,
      documentId,
      parserConfig,
    }: {
      parserId: string;
      documentId: string;
      parserConfig: IChangeParserConfigRequestBody;
    }) => {
      const { data } = await kbService.document_change_parser({
        parser_id: parserId,
        doc_id: documentId,
        parser_config: parserConfig,
      });
      if (data.code === 0) {
        queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });

        message.success(i18n.t('message.modified'));
      }
      return data.code;
    },
  });

  return { setDocumentParser: mutateAsync, data, loading };
};

export const useUploadNextDocument = () => {
  const queryClient = useQueryClient();
  const { knowledgeId } = useGetKnowledgeSearchParams();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['uploadDocument'],
    mutationFn: async (fileList: UploadFile[]) => {
      const formData = new FormData();
      formData.append('kb_id', knowledgeId);
      fileList.forEach((file: any) => {
        formData.append('file', file);
      });

      try {
        const ret = await kbService.document_upload(formData);
        const code = get(ret, 'data.code');

        if (code === 0 || code === 500) {
          queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
        }
        return ret?.data;
      } catch (error) {
        console.warn(error);
        return {
          code: 500,
          message: error + '',
        };
      }
    },
  });

  return { uploadDocument: mutateAsync, loading, data };
};

export const useNextWebCrawl = () => {
  const { knowledgeId } = useGetKnowledgeSearchParams();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['webCrawl'],
    mutationFn: async ({ name, url }: { name: string; url: string }) => {
      const formData = new FormData();
      formData.append('name', name);
      formData.append('url', url);
      formData.append('kb_id', knowledgeId);

      const ret = await kbService.web_crawl(formData);
      const code = get(ret, 'data.code');
      if (code === 0) {
        message.success(i18n.t('message.uploaded'));
      }

      return code;
    },
  });

  return {
    data,
    loading,
    webCrawl: mutateAsync,
  };
};

export const useRunNextDocument = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['runDocumentByIds'],
    mutationFn: async ({
      documentIds,
      run,
      shouldDelete,
    }: {
      documentIds: string[];
      run: number;
      shouldDelete: boolean;
    }) => {
      queryClient.invalidateQueries({
        queryKey: ['fetchDocumentList'],
      });

      const ret = await kbService.document_run({
        doc_ids: documentIds,
        run,
        delete: shouldDelete,
      });
      const code = get(ret, 'data.code');
      if (code === 0) {
        queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
        message.success(i18n.t('message.operated'));
      }

      return code;
    },
  });

  return { runDocumentByIds: mutateAsync, loading, data };
};

export const useFetchDocumentInfosByIds = () => {
  const [ids, setDocumentIds] = useState<string[]>([]);

  const idList = useMemo(() => {
    return ids.filter((x) => typeof x === 'string' && x !== '');
  }, [ids]);

  const { data } = useQuery<IDocumentInfo[]>({
    queryKey: ['fetchDocumentInfos', idList],
    enabled: idList.length > 0,
    initialData: [],
    queryFn: async () => {
      const { data } = await kbService.document_infos({ doc_ids: idList });
      if (data.code === 0) {
        return data.data;
      }

      return [];
    },
  });

  return { data, setDocumentIds };
};

export const useFetchDocumentThumbnailsByIds = () => {
  const [ids, setDocumentIds] = useState<string[]>([]);
  const { data } = useQuery<Record<string, string>>({
    queryKey: ['fetchDocumentThumbnails', ids],
    enabled: ids.length > 0,
    initialData: {},
    queryFn: async () => {
      const { data } = await kbService.document_thumbnails({ doc_ids: ids });
      if (data.code === 0) {
        return data.data;
      }
      return {};
    },
  });

  return { data, setDocumentIds };
};

export const useRemoveNextDocument = () => {
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['removeDocument'],
    mutationFn: async (documentIds: string | string[]) => {
      const { data } = await kbService.document_rm({ doc_id: documentIds });
      if (data.code === 0) {
        message.success(i18n.t('message.deleted'));
        queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });
      }
      return data.code;
    },
  });

  return { data, loading, removeDocument: mutateAsync };
};

export const useDeleteDocument = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['deleteDocument'],
    mutationFn: async (documentIds: string[]) => {
      const data = await kbService.document_delete({ doc_ids: documentIds });

      return data;
    },
  });

  return { data, loading, deleteDocument: mutateAsync };
};

export const useUploadAndParseDocument = (uploadMethod: string) => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['uploadAndParseDocument'],
    mutationFn: async ({
      conversationId,
      fileList,
    }: {
      conversationId: string;
      fileList: UploadFile[];
    }) => {
      try {
        const formData = new FormData();
        formData.append('conversation_id', conversationId);
        fileList.forEach((file: UploadFile) => {
          formData.append('file', file as any);
        });
        if (uploadMethod === 'upload_and_parse') {
          const data = await kbService.upload_and_parse(formData);
          return data?.data;
        }
        const data = await chatService.uploadAndParseExternal(formData);
        return data?.data;
      } catch (error) {}
    },
  });

  return { data, loading, uploadAndParseDocument: mutateAsync };
};

export const useParseDocument = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['parseDocument'],
    mutationFn: async (url: string) => {
      try {
        const data = await post(api.parse, { url });
        if (data?.code === 0) {
          message.success(i18n.t('message.uploaded'));
        }
        return data;
      } catch (error) {
        message.error('error');
      }
    },
  });

  return { parseDocument: mutateAsync, data, loading };
};

export const useSetDocumentMeta = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['setDocumentMeta'],
    mutationFn: async (params: IDocumentMetaRequestBody) => {
      try {
        const { data } = await kbService.setMeta({
          meta: params.meta,
          doc_id: params.documentId,
        });

        if (data?.code === 0) {
          queryClient.invalidateQueries({ queryKey: ['fetchDocumentList'] });

          message.success(i18n.t('message.modified'));
        }
        return data?.code;
      } catch (error) {
        message.error('error');
      }
    },
  });

  return { setDocumentMeta: mutateAsync, data, loading };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/document-hooks.ts` is located in the `web/src/hooks` directory.

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
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)
- [login-hooks.ts](login-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
