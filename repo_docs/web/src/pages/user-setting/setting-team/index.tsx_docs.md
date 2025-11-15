# File Documentation: web/src/pages/user-setting/setting-team/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-team/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 103
- **Characters**: 3,432
- **Size**: 3,432 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

    // <div className="w-full flex flex-col gap-4 relative">
    //   <Spotlight />
    //   <UserSettingHeader
    //     name={userInfo?.nickname + ' ' + t('setting.workspace')}
    //   />

## Detailed Walkthrough


### Functions (1)

- `UserSettingTeam()`: Function definition

### Imports (13)

- `import { Button } from '@/components/ui/button';`
- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import {`
- `import { useTranslation } from 'react-i18next';`
- `import Spotlight from '@/components/spotlight';`
- `import { SearchInput } from '@/components/ui/input';`
- `import { UserPlus } from 'lucide-react';`
- `import { useState } from 'react';`
- `import {`
- `import AddingUserModal from './add-user-modal';`

## Code Structure Analysis

- Total lines: 103
- Blank lines: 7 (6.8%)
- Comment lines: ~5 (4.9%)
- Code lines: ~91


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/card`
- `react-i18next`
- `@/components/spotlight`
- `@/components/ui/input`
- `lucide-react`
- `react`
- `./add-user-modal`
- `./hooks`
- `./tenant-table`
- `./user-table`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-team`.

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

- Other files in `web/src/pages/user-setting/setting-team/` directory
- Potential test file: `test_index.tsx`

## Keywords

./add-user-modal, ./hooks, ./tenant-table, ./user-table, @/components/spotlight, @/components/ui/button, @/components/ui/card, @/components/ui/input, AddingUserModal, Button, Card, CardContent, CardHeader, CardTitle, ProfileSettingWrapperCard, SearchInput, Spotlight, TenantTable, TypeScript, User, UserPlus, UserSettingHeader, UserSettingTeam, UserTable, Users, lucide-react, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
