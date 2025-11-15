# File Documentation: web/src/pages/user-setting/setting-team/hooks.ts

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-team/hooks.ts`
- **Extension**: `.ts`
- **Lines**: 89
- **Characters**: 2,302
- **Size**: 2,302 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';
import {
  useAddTenantUser,
  useAgreeTenant,
  useDeleteTenantUser,
  useFetchUserInfo,
} from '@/hooks/user-setting-hooks';
import { useCallback } from 'react';
import { useTranslation } from 'react-i18next';

export const useAddUser = () => {
  const { addTenantUser } = useAddTenantUser();
  const {
    visible: addingTenantModalVisible,
    hideModal: hideAddingTenantModal,
    showModal: showAddingTenantModal,
  } = useSetModalState();

  const handleAddUserOk = useCallback(
    async (email: string) => {
      const code = await addTenantUser(email);
      if (code === 0) {
        hideAddingTenantModal();
      }
    },
    [addTenantUser, hideAddingTenantModal],
  );

  return {
    addingTenantModalVisible,
    hideAddingTenantModal,
    showAddingTenantModal,
    handleAddUserOk,
  };
};

export const useHandleDeleteUser = () => {
  const { deleteTenantUser, loading } = useDeleteTenantUser();
  const showDeleteConfirm = useShowDeleteConfirm();
  const { t } = useTranslation();

  const handleDeleteTenantUser = (userId: string) => () => {
    showDeleteConfirm({
      title: t('setting.sureDelete'),
      onOk: async () => {
        const code = await deleteTenantUser({ userId });
        if (code === 0) {
        }
        return;
      },
    });
  };

  return { handleDeleteTenantUser, loading };
};

export const useHandleAgreeTenant = () => {
  const { agreeTenant } = useAgreeTenant();
  const { deleteTenantUser } = useDeleteTenantUser();
  const { data: user } = useFetchUserInfo();

  const handleAgree = (tenantId: string, isAgree: boolean) => () => {
    if (isAgree) {
      agreeTenant(tenantId);
    } else {
      deleteTenantUser({ tenantId, userId: user.id });
    }
  };

  return { handleAgree };
};

export const useHandleQuitUser = () => {
  const { deleteTenantUser, loading } = useDeleteTenantUser();
  const showDeleteConfirm = useShowDeleteConfirm();
  const { t } = useTranslation();

  const handleQuitTenantUser = (userId: string, tenantId: string) => () => {
    showDeleteConfirm({
      title: t('setting.sureQuit'),
      onOk: async () => {
        deleteTenantUser({ userId, tenantId });
      },
    });
  };

  return { handleQuitTenantUser, loading };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-team/hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 89 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `useAddUser`: Exported entity
- `useHandleDeleteUser`: Exported entity
- `useHandleAgreeTenant`: Exported entity
- `useHandleQuitUser`: Exported entity

### Functions (8)

- `useAddUser()`: Function definition
- `handleAddUserOk()`: Function definition
- `useHandleDeleteUser()`: Function definition
- `handleDeleteTenantUser()`: Function definition
- `useHandleAgreeTenant()`: Function definition
- `handleAgree()`: Function definition
- `useHandleQuitUser()`: Function definition
- `handleQuitTenantUser()`: Function definition

### Imports (4)

- `import { useSetModalState, useShowDeleteConfirm } from '@/hooks/common-hooks';`
- `import {`
- `import { useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 89
- Blank lines: 13 (14.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~76


## Dependencies and Imports

- `@/hooks/common-hooks`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-team`.

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

- Other files in `web/src/pages/user-setting/setting-team/` directory
- Potential test file: `test_hooks.ts`

## Keywords

@/hooks/common-hooks, TypeScript, code, handleAddUserOk, handleAgree, handleDeleteTenantUser, handleQuitTenantUser, react, react-i18next, showDeleteConfirm, useAddUser, useHandleAgreeTenant, useHandleDeleteUser, useHandleQuitUser

---
*Generated by RAGFlow Repository Documentation Generator*
