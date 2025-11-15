# Documentation: web/src/pages/user-setting/setting-team/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-team/index.tsx`
- **Size**: 3432 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/setting-team/index.tsx`.

## Original Source Code

```tsx
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import {
  useFetchUserInfo,
  useListTenantUser,
} from '@/hooks/user-setting-hooks';
import { useTranslation } from 'react-i18next';

import Spotlight from '@/components/spotlight';
import { SearchInput } from '@/components/ui/input';
import { UserPlus } from 'lucide-react';
import { useState } from 'react';
import {
  ProfileSettingWrapperCard,
  UserSettingHeader,
} from '../components/user-setting-header';
import AddingUserModal from './add-user-modal';
import { useAddUser } from './hooks';
import TenantTable from './tenant-table';
import UserTable from './user-table';

const UserSettingTeam = () => {
  const { data: userInfo } = useFetchUserInfo();
  const { t } = useTranslation();
  const [searchTerm, setSearchTerm] = useState('');
  const [searchUser, setSearchUser] = useState('');
  useListTenantUser();
  const {
    addingTenantModalVisible,
    hideAddingTenantModal,
    showAddingTenantModal,
    handleAddUserOk,
  } = useAddUser();

  return (
    // <div className="w-full flex flex-col gap-4 relative">
    //   <Spotlight />
    //   <UserSettingHeader
    //     name={userInfo?.nickname + ' ' + t('setting.workspace')}
    //   />
    <ProfileSettingWrapperCard
      header={
        <UserSettingHeader
          name={userInfo?.nickname + ' ' + t('setting.workspace')}
        />
      }
    >
      <Spotlight />
      <Card className="bg-transparent border-none">
        <CardHeader className="flex flex-row items-center justify-between space-y-0 p-4 pb-4">
          {/* <User className="mr-2 h-5 w-5 text-[#1677ff]" /> */}
          <CardTitle className="text-base">
            {t('setting.teamMembers')}
          </CardTitle>
          <section className="flex gap-4 items-center">
            <SearchInput
              className="bg-bg-input border-border-default w-32"
              placeholder={t('common.search')}
              value={searchUser}
              onChange={(e) => setSearchUser(e.target.value)}
            />
            <Button onClick={showAddingTenantModal}>
              <UserPlus className=" h-4 w-4" />
              {t('setting.invite')}
            </Button>
          </section>
        </CardHeader>
        <CardContent className="p-4">
          <UserTable searchUser={searchUser}></UserTable>
        </CardContent>
      </Card>

      <Card className="bg-transparent border-none mt-8">
        <CardHeader className="flex flex-row items-center justify-between space-y-0 p-4 pb-4">
          {/* <Users className="mr-2 h-5 w-5 text-[#1677ff]" /> */}
          <CardTitle className="text-base w-fit">
            {t('setting.joinedTeams')}
          </CardTitle>
          <SearchInput
            className="bg-bg-input border-border-default w-32"
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            placeholder={t('common.search')}
          />
        </CardHeader>
        <CardContent className="p-4">
          <TenantTable searchTerm={searchTerm}></TenantTable>
        </CardContent>
      </Card>

      {addingTenantModalVisible && (
        <AddingUserModal
          visible
          hideModal={hideAddingTenantModal}
          onOk={handleAddUserOk}
        ></AddingUserModal>
      )}
    </ProfileSettingWrapperCard>
  );
};

export default UserSettingTeam;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/setting-team/index.tsx` is located in the `web/src/pages/user-setting/setting-team` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to setting-team.

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

- [add-user-modal.tsx](add-user-modal.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [tenant-table.tsx](tenant-table.tsx_docs.md)
- [user-table.tsx](user-table.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
