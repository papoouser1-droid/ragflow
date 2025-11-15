# File Documentation: web/src/hooks/logic-hooks/navigate-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/logic-hooks/navigate-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 194
- **Characters**: 5,027
- **Size**: 5,027 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

      // navigate(`${Routes.DatasetBase}${Routes.DataSetOverview}/${id}`);

## Detailed Walkthrough

### Exports (1)

- `useNavigatePage`: Exported entity

### Functions (23)

- `useNavigatePage()`: Function definition
- `navigateToDatasetList()`: Function definition
- `navigateToDataset()`: Function definition
- `navigateToDatasetOverview()`: Function definition
- `navigateToDataFile()`: Function definition
- `navigateToHome()`: Function definition
- `navigateToProfile()`: Function definition
- `navigateToOldProfile()`: Function definition
- `navigateToChatList()`: Function definition
- `navigateToChat()`: Function definition
- `navigateToAgents()`: Function definition
- `navigateToAgentList()`: Function definition
- `navigateToAgent()`: Function definition
- `navigateToAgentLogs()`: Function definition
- `navigateToAgentTemplates()`: Function definition
- `navigateToSearchList()`: Function definition
- `navigateToSearch()`: Function definition
- `navigateToChunkParsedResult()`: Function definition
- `getQueryString()`: Function definition
- `navigateToChunk()`: Function definition

### Imports (5)

- `import { AgentCategory, AgentQuery } from '@/constants/agent';`
- `import { NavigateToDataflowResultProps } from '@/pages/dataflow-result/interface';`
- `import { Routes } from '@/routes';`
- `import { useCallback } from 'react';`
- `import { useNavigate, useParams, useSearchParams } from 'umi';`

## Code Structure Analysis

- Total lines: 194
- Blank lines: 25 (12.9%)
- Comment lines: ~3 (1.5%)
- Code lines: ~166


## Dependencies and Imports

- `@/constants/agent`
- `@/pages/dataflow-result/interface`
- `@/routes`
- `react`
- `umi`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks/logic-hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/hooks/logic-hooks/` directory
- Potential test file: `test_navigate-hooks.ts`

## Keywords

@/constants/agent, @/pages/dataflow-result/interface, @/routes, Agent, AgentCategory, AgentList, AgentLogPage, AgentQuery, AgentTemplates, Agents, Category, Chat, Chats, DataSetOverview, DataSource, DataSourceDetailPage, DataflowResult, Dataset, DatasetBase, Datasets, Files, KnowledgeId, NavigateToDataflowResultProps, Object, ParsedResult, ProfileSetting, QueryStringMap, Root, Routes, Search, Searches, TypeScript, UserSetting, allQueryString, getQueryString, navigate, navigateToAgent, navigateToAgentList, navigateToAgentLogs, navigateToAgentTemplates, navigateToAgents, navigateToChat, navigateToChatList, navigateToChunk, navigateToChunkParsedResult, navigateToDataFile, navigateToDataSourceDetail, navigateToDataflowResult, navigateToDataset, navigateToDatasetList...

---
*Generated by RAGFlow Repository Documentation Generator*
