# Documentation: web/src/pages/agents/agent-templates.tsx

## File Metadata

- **Path**: `web/src/pages/agents/agent-templates.tsx`
- **Size**: 4257 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agents/agent-templates.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agents/agent-templates.tsx` is located in the `web/src/pages/agents` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to agents.

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

- [agent-card.tsx](agent-card.tsx_docs.md)
- [agent-dropdown.tsx](agent-dropdown.tsx_docs.md)
- [agent-log-detail-modal.tsx](agent-log-detail-modal.tsx_docs.md)
- [agent-log-page.tsx](agent-log-page.tsx_docs.md)
- [constant.ts](constant.ts_docs.md)
- [create-agent-dialog.tsx](create-agent-dialog.tsx_docs.md)
- [create-agent-form.tsx](create-agent-form.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [name-form-field.tsx](name-form-field.tsx_docs.md)
- [template-card.tsx](template-card.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
