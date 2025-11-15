# Documentation: web/src/hooks/logic-hooks/navigate-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/logic-hooks/navigate-hooks.ts`
- **Size**: 5027 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/logic-hooks/navigate-hooks.ts`.

## Original Source Code

```ts
import { AgentCategory, AgentQuery } from '@/constants/agent';
import { NavigateToDataflowResultProps } from '@/pages/dataflow-result/interface';
import { Routes } from '@/routes';
import { useCallback } from 'react';
import { useNavigate, useParams, useSearchParams } from 'umi';

export enum QueryStringMap {
  KnowledgeId = 'knowledgeId',
  id = 'id',
}

export const useNavigatePage = () => {
  const navigate = useNavigate();
  const [searchParams] = useSearchParams();
  const { id } = useParams();

  const navigateToDatasetList = useCallback(() => {
    navigate(Routes.Datasets);
  }, [navigate]);

  const navigateToDataset = useCallback(
    (id: string) => () => {
      // navigate(`${Routes.DatasetBase}${Routes.DataSetOverview}/${id}`);
      navigate(`${Routes.Dataset}/${id}`);
    },
    [navigate],
  );
  const navigateToDatasetOverview = useCallback(
    (id: string) => () => {
      navigate(`${Routes.DatasetBase}${Routes.DataSetOverview}/${id}`);
    },
    [navigate],
  );

  const navigateToDataFile = useCallback(
    (id: string) => () => {
      navigate(`${Routes.DatasetBase}${Routes.DatasetBase}/${id}`);
    },
    [navigate],
  );

  const navigateToHome = useCallback(() => {
    navigate(Routes.Root);
  }, [navigate]);

  const navigateToProfile = useCallback(() => {
    navigate(Routes.ProfileSetting);
  }, [navigate]);

  const navigateToOldProfile = useCallback(() => {
    navigate(Routes.UserSetting);
  }, [navigate]);

  const navigateToChatList = useCallback(() => {
    navigate(Routes.Chats);
  }, [navigate]);

  const navigateToChat = useCallback(
    (id: string) => () => {
      navigate(`${Routes.Chat}/${id}`);
    },
    [navigate],
  );

  const navigateToAgents = useCallback(() => {
    navigate(Routes.Agents);
  }, [navigate]);

  const navigateToAgentList = useCallback(() => {
    navigate(Routes.AgentList);
  }, [navigate]);

  const navigateToAgent = useCallback(
    (id: string, category?: AgentCategory) => () => {
      navigate(`${Routes.Agent}/${id}?${AgentQuery.Category}=${category}`);
    },
    [navigate],
  );

  const navigateToAgentLogs = useCallback(
    (id: string) => () => {
      navigate(`${Routes.AgentLogPage}/${id}`);
    },
    [navigate],
  );

  const navigateToAgentTemplates = useCallback(() => {
    navigate(Routes.AgentTemplates);
  }, [navigate]);

  const navigateToSearchList = useCallback(() => {
    navigate(Routes.Searches);
  }, [navigate]);

  const navigateToSearch = useCallback(
    (id: string) => () => {
      navigate(`${Routes.Search}/${id}`);
    },
    [navigate],
  );

  const navigateToChunkParsedResult = useCallback(
    (id: string, knowledgeId?: string) => () => {
      navigate(
        `${Routes.ParsedResult}/chunks?id=${knowledgeId}&doc_id=${id}`,
        // `${Routes.DataflowResult}?id=${knowledgeId}&doc_id=${id}&type=chunk`,
      );
    },
    [navigate],
  );

  const getQueryString = useCallback(
    (queryStringKey?: QueryStringMap) => {
      const allQueryString = {
        [QueryStringMap.KnowledgeId]: searchParams.get(
          QueryStringMap.KnowledgeId,
        ),
        [QueryStringMap.id]: searchParams.get(QueryStringMap.id),
      };
      if (queryStringKey) {
        return allQueryString[queryStringKey];
      }
      return allQueryString;
    },
    [searchParams],
  );

  const navigateToChunk = useCallback(
    (route: Routes) => {
      navigate(
        `${route}/${id}?${QueryStringMap.KnowledgeId}=${getQueryString(QueryStringMap.KnowledgeId)}`,
      );
    },
    [getQueryString, id, navigate],
  );

  const navigateToFiles = useCallback(
    (folderId?: string) => {
      navigate(`${Routes.Files}?folderId=${folderId}`);
    },
    [navigate],
  );

  const navigateToDataSourceDetail = useCallback(
    (id?: string) => {
      navigate(
        `${Routes.UserSetting}${Routes.DataSource}${Routes.DataSourceDetailPage}?id=${id}`,
      );
    },
    [navigate],
  );

  const navigateToDataflowResult = useCallback(
    (props: NavigateToDataflowResultProps) => () => {
      let params: string[] = [];
      Object.keys(props).forEach((key) => {
        if (props[key as keyof typeof props]) {
          params.push(`${key}=${props[key as keyof typeof props]}`);
        }
      });
      navigate(
        // `${Routes.ParsedResult}/${id}?${QueryStringMap.KnowledgeId}=${knowledgeId}`,
        `${Routes.DataflowResult}?${params.join('&')}`,
      );
    },
    [navigate],
  );

  return {
    navigateToDatasetList,
    navigateToDataset,
    navigateToDatasetOverview,
    navigateToHome,
    navigateToProfile,
    navigateToChatList,
    navigateToChat,
    navigateToChunkParsedResult,
    getQueryString,
    navigateToChunk,
    navigateToAgents,
    navigateToAgent,
    navigateToAgentLogs,
    navigateToAgentTemplates,
    navigateToSearchList,
    navigateToSearch,
    navigateToFiles,
    navigateToAgentList,
    navigateToOldProfile,
    navigateToDataflowResult,
    navigateToDataFile,
    navigateToDataSourceDetail,
  };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/logic-hooks/navigate-hooks.ts` is located in the `web/src/hooks/logic-hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to logic-hooks.

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

- [use-build-operator-options.tsx](use-build-operator-options.tsx_docs.md)
- [use-change-search.ts](use-change-search.ts_docs.md)
- [use-pagination.ts](use-pagination.ts_docs.md)
- [use-row-selection.ts](use-row-selection.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
