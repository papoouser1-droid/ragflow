# File Documentation: web/src/pages/user-setting/mcp/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/mcp/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 158
- **Characters**: 5,290
- **Size**: 5,290 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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
import { ProfileSettingWrapperCard } from '../components/user-setting-header';
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/mcp/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 158 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `McpServer`: Exported entity

### Functions (3)

- `McpServer()`: Function definition
- `handlePageChange()`: Function definition
- `switchSelectionMode()`: Function definition

### Imports (20)

- `import { CardContainer } from '@/components/card-container';`
- `import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';`
- `import Spotlight from '@/components/spotlight';`
- `import { Button } from '@/components/ui/button';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import { SearchInput } from '@/components/ui/input';`
- `import { Label } from '@/components/ui/label';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import { useListMcpServer } from '@/hooks/use-mcp-request';`
- `import { pick } from 'lodash';`

## Code Structure Analysis

- Total lines: 158
- Blank lines: 6 (3.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~152


## Dependencies and Imports

- `@/components/card-container`
- `@/components/confirm-delete-dialog`
- `@/components/spotlight`
- `@/components/ui/button`
- `@/components/ui/checkbox`
- `@/components/ui/input`
- `@/components/ui/label`
- `@/components/ui/ragflow-pagination`
- `@/hooks/use-mcp-request`
- `lodash`
- `react`
- `react-i18next`
- `../components/user-setting-header`
- `./edit-mcp-dialog`
- `./import-mcp-dialog`
- `./mcp-card`
- `./use-bulk-operate-mcp`
- `./use-edit-mcp`
- `./use-import-mcp`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/mcp`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/mcp/` directory
- Potential test file: `test_index.tsx`

## Keywords

../components/user-setting-header, ./edit-mcp-dialog, ./import-mcp-dialog, ./mcp-card, ./use-bulk-operate-mcp, ./use-edit-mcp, ./use-import-mcp, @/components/card-container, @/components/confirm-delete-dialog, @/components/spotlight, @/components/ui/button, @/components/ui/checkbox, @/components/ui/input, @/components/ui/label, @/components/ui/ragflow-pagination, @/hooks/use-mcp-request, Button, CardContainer, Checkbox, ConfirmDeleteDialog, Download, EditMcpDialog, ImportMcpDialog, Label, LayoutList, ListChecks, McpCard, McpServer, Plus, ProfileSettingWrapperCard, RAGFlowPagination, SearchInput, Spotlight, Trash2, TypeScript, Upload, handlePageChange, lodash, react, react-i18next, switchSelectionMode

---
*Generated by RAGFlow Repository Documentation Generator*
