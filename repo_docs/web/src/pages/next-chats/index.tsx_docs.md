# File Documentation: web/src/pages/next-chats/index.tsx

## File Metadata

- **Path**: `web/src/pages/next-chats/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 85
- **Characters**: 2,656
- **Size**: 2,656 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { CardContainer } from '@/components/card-container';
import ListFilterBar from '@/components/list-filter-bar';
import { RenameDialog } from '@/components/rename-dialog';
import { Button } from '@/components/ui/button';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { useFetchDialogList } from '@/hooks/use-chat-request';
import { pick } from 'lodash';
import { Plus } from 'lucide-react';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { ChatCard } from './chat-card';
import { useRenameChat } from './hooks/use-rename-chat';

export default function ChatList() {
  const { data, setPagination, pagination, handleInputChange, searchString } =
    useFetchDialogList();
  const { t } = useTranslation();
  const {
    initialChatName,
    chatRenameVisible,
    showChatRenameModal,
    hideChatRenameModal,
    onChatRenameOk,
    chatRenameLoading,
  } = useRenameChat();

  const handlePageChange = useCallback(
    (page: number, pageSize?: number) => {
      setPagination({ page, pageSize });
    },
    [setPagination],
  );

  const handleShowCreateModal = useCallback(() => {
    showChatRenameModal();
  }, [showChatRenameModal]);

  return (
    <section className="flex flex-col w-full flex-1">
      <div className="px-8 pt-8">
        <ListFilterBar
          title={t('chat.chatApps')}
          icon="chats"
          onSearchChange={handleInputChange}
          searchString={searchString}
        >
          <Button onClick={handleShowCreateModal}>
            <Plus className="size-2.5" />
            {t('chat.createChat')}
          </Button>
        </ListFilterBar>
      </div>
      <div className="flex-1 overflow-auto">
        <CardContainer className="max-h-[calc(100dvh-280px)] overflow-auto px-8">
          {data.dialogs.map((x) => {
            return (
              <ChatCard
                key={x.id}
                data={x}
                showChatRenameModal={showChatRenameModal}
              ></ChatCard>
            );
          })}
        </CardContainer>
      </div>
      <div className="mt-8 px-8 pb-8">
        <RAGFlowPagination
          {...pick(pagination, 'current', 'pageSize')}
          total={pagination.total}
          onChange={handlePageChange}
        ></RAGFlowPagination>
      </div>
      {chatRenameVisible && (
        <RenameDialog
          hideModal={hideChatRenameModal}
          onOk={onChatRenameOk}
          initialName={initialChatName}
          loading={chatRenameLoading}
          title={initialChatName || t('chat.createChat')}
        ></RenameDialog>
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 85 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ChatList`: Exported entity

### Functions (3)

- `ChatList()`: Function definition
- `handlePageChange()`: Function definition
- `handleShowCreateModal()`: Function definition

### Imports (12)

- `import { CardContainer } from '@/components/card-container';`
- `import ListFilterBar from '@/components/list-filter-bar';`
- `import { RenameDialog } from '@/components/rename-dialog';`
- `import { Button } from '@/components/ui/button';`
- `import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';`
- `import { useFetchDialogList } from '@/hooks/use-chat-request';`
- `import { pick } from 'lodash';`
- `import { Plus } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 85
- Blank lines: 5 (5.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~80


## Dependencies and Imports

- `@/components/card-container`
- `@/components/list-filter-bar`
- `@/components/rename-dialog`
- `@/components/ui/button`
- `@/components/ui/ragflow-pagination`
- `@/hooks/use-chat-request`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`
- `./chat-card`
- `./hooks/use-rename-chat`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats`.

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

- Other files in `web/src/pages/next-chats/` directory
- Potential test file: `test_index.tsx`

## Keywords

./chat-card, ./hooks/use-rename-chat, @/components/card-container, @/components/list-filter-bar, @/components/rename-dialog, @/components/ui/button, @/components/ui/ragflow-pagination, @/hooks/use-chat-request, Button, CardContainer, ChatCard, ChatList, ListFilterBar, Plus, RAGFlowPagination, RenameDialog, TypeScript, handlePageChange, handleShowCreateModal, lodash, lucide-react, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
