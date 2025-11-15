# File Documentation: web/src/pages/agents/agent-templates.tsx

## File Metadata

- **Path**: `web/src/pages/agents/agent-templates.tsx`
- **Extension**: `.tsx`
- **Lines**: 148
- **Characters**: 4,257
- **Size**: 4,257 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { PageHeader } from '@/components/page-header';
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/components/ui/breadcrumb';
import { useSetModalState } from '@/hooks/common-hooks';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { useFetchAgentTemplates, useSetAgent } from '@/hooks/use-agent-request';

import { CardContainer } from '@/components/card-container';
import { AgentCategory } from '@/constants/agent';
import { IFlowTemplate } from '@/interfaces/database/agent';
import { useCallback, useEffect, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { CreateAgentDialog } from './create-agent-dialog';
import { TemplateCard } from './template-card';
import { MenuItemKey, SideBar } from './template-sidebar';

export default function AgentTemplates() {
  const { navigateToAgents } = useNavigatePage();
  const { t } = useTranslation();
  const list = useFetchAgentTemplates();
  const { loading, setAgent } = useSetAgent();
  const [templateList, setTemplateList] = useState<IFlowTemplate[]>([]);
  const [selectMenuItem, setSelectMenuItem] = useState<string>(
    MenuItemKey.Recommended,
  );

  useEffect(() => {
    setTemplateList(list);
  }, [list]);

  const {
    visible: creatingVisible,
    hideModal: hideCreatingModal,
    showModal: showCreatingModal,
  } = useSetModalState();

  const [template, setTemplate] = useState<IFlowTemplate>();

  const showModal = useCallback(
    (record: IFlowTemplate) => {
      setTemplate(record);
      showCreatingModal();
    },
    [showCreatingModal],
  );

  const { navigateToAgent } = useNavigatePage();

  const handleOk = useCallback(
    async (payload: any) => {
      let dsl = template?.dsl;
      const canvasCategory = template?.canvas_category;

      const ret = await setAgent({
        title: payload.name,
        dsl,
        avatar: template?.avatar,
        canvas_category: canvasCategory,
      });

      if (ret?.code === 0) {
        hideCreatingModal();
        if (canvasCategory === AgentCategory.DataflowCanvas) {
          navigateToAgent(ret.data.id, AgentCategory.DataflowCanvas)();
        } else {
          navigateToAgent(ret.data.id)();
        }
      }
    },
    [
      hideCreatingModal,
      navigateToAgent,
      setAgent,
      template?.avatar,
      template?.canvas_category,
      template?.dsl,
    ],
  );
  const handleSiderBarChange = (keyword: string) => {
    setSelectMenuItem(keyword);
  };

  const tempListFilter = useMemo(() => {
    if (!selectMenuItem) {
      return templateList;
    }
    return templateList.filter(
      (item) =>
        item.canvas_type?.toLocaleLowerCase() ===
        selectMenuItem?.toLocaleLowerCase(),
    );
  }, [selectMenuItem, templateList]);

  return (
    <section>
      <PageHeader>
        <Breadcrumb>
          <BreadcrumbList>
            <BreadcrumbItem>
              <BreadcrumbLink onClick={navigateToAgents}>
                {t('flow.agent')}
              </BreadcrumbLink>
            </BreadcrumbItem>
            <BreadcrumbSeparator />
            <BreadcrumbItem>
              <BreadcrumbPage>{t('flow.createGraph')}</BreadcrumbPage>
            </BreadcrumbItem>
          </BreadcrumbList>
        </Breadcrumb>
      </PageHeader>
      <div className="flex flex-1 h-dvh">
        <SideBar
          change={handleSiderBarChange}
          selected={selectMenuItem}
        ></SideBar>

        <main className="flex-1 bg-text-title-invert/50 h-dvh">
          <CardContainer className="max-h-[94vh] overflow-auto px-8 pt-8">
            {tempListFilter?.map((x) => {
              return (
                <TemplateCard
                  key={x.id}
                  data={x}
                  showModal={showModal}
                ></TemplateCard>
              );
            })}
          </CardContainer>
          {creatingVisible && (
            <CreateAgentDialog
              loading={loading}
              visible={creatingVisible}
              hideModal={hideCreatingModal}
              onOk={handleOk}
            ></CreateAgentDialog>
          )}
        </main>
      </div>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agents/agent-templates.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 148 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `AgentTemplates`: Exported entity

### Functions (5)

- `AgentTemplates()`: Function definition
- `showModal()`: Function definition
- `handleOk()`: Function definition
- `handleSiderBarChange()`: Function definition
- `tempListFilter()`: Function definition

### Imports (13)

- `import { PageHeader } from '@/components/page-header';`
- `import {`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { useFetchAgentTemplates, useSetAgent } from '@/hooks/use-agent-request';`
- `import { CardContainer } from '@/components/card-container';`
- `import { AgentCategory } from '@/constants/agent';`
- `import { IFlowTemplate } from '@/interfaces/database/agent';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 148
- Blank lines: 14 (9.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~134


## Dependencies and Imports

- `@/components/page-header`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks/navigate-hooks`
- `@/hooks/use-agent-request`
- `@/components/card-container`
- `@/constants/agent`
- `@/interfaces/database/agent`
- `react`
- `react-i18next`
- `./create-agent-dialog`
- `./template-card`
- `./template-sidebar`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agents`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agents/` directory
- Potential test file: `test_agent-templates.tsx`

## Keywords

./create-agent-dialog, ./template-card, ./template-sidebar, @/components/card-container, @/components/page-header, @/constants/agent, @/hooks/common-hooks, @/hooks/logic-hooks/navigate-hooks, @/hooks/use-agent-request, @/interfaces/database/agent, AgentCategory, AgentTemplates, Breadcrumb, BreadcrumbItem, BreadcrumbLink, BreadcrumbList, BreadcrumbPage, BreadcrumbSeparator, CardContainer, CreateAgentDialog, DataflowCanvas, IFlowTemplate, MenuItemKey, PageHeader, Recommended, SideBar, TemplateCard, TypeScript, canvasCategory, dsl, handleOk, handleSiderBarChange, list, react, react-i18next, ret, showModal, tempListFilter

---
*Generated by RAGFlow Repository Documentation Generator*
