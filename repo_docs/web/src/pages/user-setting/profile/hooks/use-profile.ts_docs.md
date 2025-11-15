# File Documentation: web/src/pages/user-setting/profile/hooks/use-profile.ts

## File Metadata

- **Path**: `web/src/pages/user-setting/profile/hooks/use-profile.ts`
- **Extension**: `.ts`
- **Lines**: 152
- **Characters**: 3,763
- **Size**: 3,763 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
// src/hooks/useProfile.ts
import {
  useFetchUserInfo,
  useSaveSetting,
} from '@/hooks/use-user-setting-request';
import { rsaPsw } from '@/utils';
import { useCallback, useEffect, useState } from 'react';

interface ProfileData {
  userName: string;
  timeZone: string;
  currPasswd?: string;
  newPasswd?: string;
  avatar: string;
  email: string;
  confirmPasswd?: string;
}

export const EditType = {
  editName: 'editName',
  editTimeZone: 'editTimeZone',
  editPassword: 'editPassword',
} as const;

export type IEditType = keyof typeof EditType;

export const modalTitle = {
  [EditType.editName]: 'Edit Name',
  [EditType.editTimeZone]: 'Edit Time Zone',
  [EditType.editPassword]: 'Edit Password',
} as const;

export const useProfile = () => {
  const { data: userInfo } = useFetchUserInfo();
  const [profile, setProfile] = useState<ProfileData>({
    userName: '',
    avatar: '',
    timeZone: '',
    email: '',
    currPasswd: '',
  });

  const [editType, setEditType] = useState<IEditType>(EditType.editName);
  const [isEditing, setIsEditing] = useState(false);
  const [editForm, setEditForm] = useState<Partial<ProfileData>>({});
  const {
    saveSetting,
    loading: submitLoading,
    data: saveSettingData,
  } = useSaveSetting();

  useEffect(() => {
    // form.setValue('currPasswd', ''); // current password
    const profile = {
      userName: userInfo.nickname,
      timeZone: userInfo.timezone,
      avatar: userInfo.avatar || '',
      email: userInfo.email,
      currPasswd: userInfo.password,
    };
    setProfile(profile);
  }, [userInfo, setProfile]);

  useEffect(() => {
    if (saveSettingData === 0) {
      setIsEditing(false);
      setEditForm({});
    }
  }, [saveSettingData]);
  const onSubmit = (newProfile: ProfileData) => {
    const payload: Partial<{
      nickname: string;
      password: string;
      new_password: string;
      avatar: string;
      timezone: string;
    }> = {
      nickname: newProfile.userName,
      avatar: newProfile.avatar,
      timezone: newProfile.timeZone,
    };

    if (
      'currPasswd' in newProfile &&
      'newPasswd' in newProfile &&
      newProfile.currPasswd &&
      newProfile.newPasswd
    ) {
      payload.password = rsaPsw(newProfile.currPasswd!) as string;
      payload.new_password = rsaPsw(newProfile.newPasswd!) as string;
    }
    console.log('payload', payload);
    if (editType === EditType.editName && payload.nickname) {
      saveSetting({ nickname: payload.nickname });
      setProfile(newProfile);
    }
    if (editType === EditType.editTimeZone && payload.timezone) {
      saveSetting({ timezone: payload.timezone });
      setProfile(newProfile);
    }
    if (editType === EditType.editPassword && payload.password) {
      saveSetting({
        password: payload.password,
        new_password: payload.new_password,
      });
      setProfile(newProfile);
    }
    // saveSetting(payload);
  };

  const handleEditClick = useCallback(
    (type: IEditType) => {
      setEditForm(profile);
      setEditType(type);
      setIsEditing(true);
    },
    [profile],
  );

  const handleCancel = useCallback(() => {
    setIsEditing(false);
    setEditForm({});
  }, []);

  const handleSave = (data: ProfileData) => {
    console.log('handleSave', data);
    const newProfile = { ...profile, ...data };

    onSubmit(newProfile);
    // setIsEditing(false);
    // setEditForm({});
  };

  const handleAvatarUpload = (avatar: string) => {
    setProfile((prev) => ({ ...prev, avatar }));
    saveSetting({ avatar });
  };

  return {
    profile,
    setProfile,
    submitLoading: submitLoading,
    isEditing,
    editType,
    editForm,
    handleEditClick,
    handleCancel,
    handleSave,
    handleAvatarUpload,
  };
};

```

## High-Level Overview

// src/hooks/useProfile.ts

## Detailed Walkthrough

### Exports (3)

- `EditType`: Exported entity
- `modalTitle`: Exported entity
- `useProfile`: Exported entity

### Functions (7)

- `useProfile()`: Function definition
- `profile()`: Function definition
- `onSubmit()`: Function definition
- `handleEditClick()`: Function definition
- `handleCancel()`: Function definition
- `handleSave()`: Function definition
- `handleAvatarUpload()`: Function definition

### Imports (3)

- `import {`
- `import { rsaPsw } from '@/utils';`
- `import { useCallback, useEffect, useState } from 'react';`

## Code Structure Analysis

- Total lines: 152
- Blank lines: 16 (10.5%)
- Comment lines: ~5 (3.3%)
- Code lines: ~131


## Dependencies and Imports

- `@/utils`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/profile/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/profile/hooks/` directory
- Potential test file: `test_use-profile.ts`

## Keywords

@/utils, Edit, EditType, IEditType, Name, Partial, Password, ProfileData, Time, TypeScript, Zone, handleAvatarUpload, handleCancel, handleEditClick, handleSave, modalTitle, newProfile, onSubmit, payload, profile, react, useProfile

---
*Generated by RAGFlow Repository Documentation Generator*
