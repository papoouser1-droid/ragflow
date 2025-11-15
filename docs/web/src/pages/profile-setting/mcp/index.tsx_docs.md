# Documentation: web/src/pages/profile-setting/mcp/index.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/mcp/index.tsx`
- **Size**: 5270 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/profile-setting/mcp/index.tsx`.

## Original Source Code

```tsx
import { CardContainer } from '@/components/card-container';
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import Spotlight from '@/components/spotlight';
import { Button } from '@/components/ui/button';
import { Checkbox } from '@/components/ui/checkbox';
import { SearchInput } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { useListMcpServer } from '@/hooks/use-mcp-request';
import { pick } from 'lodash';
import {
  Download,
  LayoutList,
  ListChecks,
  Plus,
  Trash2,
  Upload,
} from 'lucide-react';
import { useCallback, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { ProfileSettingWrapperCard } from '../components';
import { EditMcpDialog } from './edit-mcp-dialog';
import { ImportMcpDialog } from './import-mcp-dialog';
import { McpCard } from './mcp-card';
import { useBulkOperateMCP } from './use-bulk-operate-mcp';
import { useEditMcp } from './use-edit-mcp';
import { useImportMcp } from './use-import-mcp';

export default function McpServer() {
  const { data, setPagination, searchString, handleInputChange, pagination } =
    useListMcpServer();
  const { editVisible, showEditModal, hideEditModal, handleOk, id, loading } =
    useEditMcp();
  const {
    selectedList,
    handleSelectChange,
    handleDelete,
    handleExportMcp,
    handleSelectAll,
  } = useBulkOperateMCP(data.mcp_servers);
  const { t } = useTranslation();
  const { importVisible, showImportModal, hideImportModal, onImportOk } =
    useImportMcp();

  const [isSelectionMode, setSelectionMode] = useState(false);

  const handlePageChange = useCallback(
    (page: number, pageSize?: number) => {
      setPagination({ page, pageSize });
    },
    [setPagination],
  );

  const switchSelectionMode = useCallback(() => {
    setSelectionMode((prev) => !prev);
  }, []);

  return (
    <ProfileSettingWrapperCard
      header={
        <>
          <div className="text-text-primary text-2xl font-semibold">
            {t('mcp.mcpServers')}
          </div>
          <section className="flex items-center justify-between">
            <div className="text-text-secondary">
              {t('mcp.customizeTheListOfMcpServers')}
            </div>
            <div className="flex gap-5">
              <SearchInput
                className="w-40"
                value={searchString}
                onChange={handleInputChange}
              ></SearchInput>
              <Button variant={'secondary'} onClick={switchSelectionMode}>
                {isSelectionMode ? (
                  <ListChecks className="size-3.5" />
                ) : (
                  <LayoutList className="size-3.5" />
                )}
                {t(`mcp.${isSelectionMode ? 'exitBulkManage' : 'bulkManage'}`)}
              </Button>
              <Button variant={'secondary'} onClick={showImportModal}>
                <Download className="size-3.5" />
                {t('mcp.import')}
              </Button>
              <Button onClick={showEditModal('')}>
                <Plus className="size-3.5" /> {t('mcp.addMCP')}
              </Button>
            </div>
          </section>
        </>
      }
    >
      {isSelectionMode && (
        <section className="pb-5 flex items-center">
          <Checkbox id="all" onCheckedChange={handleSelectAll} />
          <Label
            className="pl-2 text-text-primary cursor-pointer"
            htmlFor="all"
          >
            {t('common.selectAll')}
          </Label>
          <span className="text-text-secondary pr-10 pl-5">
            {t('mcp.selected')} {selectedList.length}
          </span>
          <div className="flex gap-10 items-center">
            <Button variant={'secondary'} onClick={handleExportMcp}>
              <Upload className="size-3.5"></Upload>
              {t('mcp.export')}
            </Button>
            <ConfirmDeleteDialog onOk={handleDelete}>
              <Button variant={'danger'}>
                <Trash2 className="size-3.5 cursor-pointer" />
                {t('common.delete')}
              </Button>
            </ConfirmDeleteDialog>
          </div>
        </section>
      )}
      <CardContainer>
        {data.mcp_servers.map((item) => (
          <McpCard
            key={item.id}
            data={item}
            selectedList={selectedList}
            handleSelectChange={handleSelectChange}
            showEditModal={showEditModal}
            isSelectionMode={isSelectionMode}
          ></McpCard>
        ))}
      </CardContainer>
      <div className="mt-8">
        <RAGFlowPagination
          {...pick(pagination, 'current', 'pageSize')}
          total={pagination.total || 0}
          onChange={handlePageChange}
        ></RAGFlowPagination>
      </div>
      {editVisible && (
        <EditMcpDialog
          hideModal={hideEditModal}
          onOk={handleOk}
          id={id}
          loading={loading}
        ></EditMcpDialog>
      )}
      {importVisible && (
        <ImportMcpDialog
          hideModal={hideImportModal}
          onOk={onImportOk}
        ></ImportMcpDialog>
      )}
      <Spotlight />
    </ProfileSettingWrapperCard>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/profile-setting/mcp/index.tsx` is located in the `web/src/pages/profile-setting/mcp` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to mcp.

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

- [edit-mcp-dialog.tsx](edit-mcp-dialog.tsx_docs.md)
- [edit-mcp-form.tsx](edit-mcp-form.tsx_docs.md)
- [mcp-card.tsx](mcp-card.tsx_docs.md)
- [mcp-operation.tsx](mcp-operation.tsx_docs.md)
- [tool-card.tsx](tool-card.tsx_docs.md)
- [use-bulk-operate-mcp.tsx](use-bulk-operate-mcp.tsx_docs.md)
- [use-edit-mcp.ts](use-edit-mcp.ts_docs.md)
- [use-export-mcp.ts](use-export-mcp.ts_docs.md)
- [use-import-mcp.ts](use-import-mcp.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
