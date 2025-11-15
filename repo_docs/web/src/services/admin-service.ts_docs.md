# File Documentation: web/src/services/admin-service.ts

## File Metadata

- **Path**: `web/src/services/admin-service.ts`
- **Extension**: `.ts`
- **Lines**: 264
- **Characters**: 8,185
- **Size**: 8,185 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { message, notification } from 'antd';
import axios from 'axios';
import { history } from 'umi';

import { Authorization } from '@/constants/authorization';
import i18n from '@/locales/config';
import { Routes } from '@/routes';
import api from '@/utils/api';
import authorizationUtil, {
  getAuthorization,
} from '@/utils/authorization-util';
import { convertTheKeysOfTheObjectToSnake } from '@/utils/common-util';
import { ResultCode, RetcodeMessage } from '@/utils/request';

const request = axios.create({
  timeout: 300000,
});

request.interceptors.request.use((config) => {
  const data = convertTheKeysOfTheObjectToSnake(config.data);
  const params = convertTheKeysOfTheObjectToSnake(config.params) as any;

  const newConfig = { ...config, data, params };

  // @ts-ignore
  if (!newConfig.skipToken) {
    newConfig.headers.set(Authorization, getAuthorization());
  }

  return newConfig;
});

request.interceptors.response.use(
  (response) => {
    if (response.config.responseType === 'blob') {
      return response;
    }

    const { data } = response ?? {};

    if (data?.code === 100) {
      message.error(data?.message);
    } else if (data?.code === 401) {
      notification.error({
        message: data?.message,
        description: data?.message,
        duration: 3,
      });

      authorizationUtil.removeAll();
      history.push(Routes.Admin);
    } else if (data?.code && data.code !== 0) {
      notification.error({
        message: `${i18n.t('message.hint')}: ${data?.code}`,
        description: data?.message,
        duration: 3,
      });
    }

    return response;
  },
  (error) => {
    const { response, message } = error;
    const { data } = response ?? {};

    if (error.message === 'Failed to fetch') {
      notification.error({
        description: i18n.t('message.networkAnomalyDescription'),
        message: i18n.t('message.networkAnomaly'),
      });
    } else if (data?.code === 100) {
      message.error(data?.message);
    } else if (response.status === 401 || data?.code === 401) {
      notification.error({
        message: data?.message || response.statusText,
        description:
          data?.message || RetcodeMessage[response?.status as ResultCode],
        duration: 3,
      });

      authorizationUtil.removeAll();
      history.push(Routes.Admin);
    } else if (data?.code && data.code !== 0) {
      notification.error({
        message: `${i18n.t('message.hint')}: ${data?.code}`,
        description: data?.message,
        duration: 3,
      });
    } else if (response.status) {
      notification.error({
        message: `${i18n.t('message.requestError')} ${response.status}: ${response.config.url}`,
        description:
          RetcodeMessage[response.status as ResultCode] || response.statusText,
      });
    } else if (response.status === 413 || response?.status === 504) {
      message.error(RetcodeMessage[response?.status as ResultCode]);
    }

    throw error;
  },
);

const {
  adminLogin,
  adminLogout,
  adminListUsers,
  adminCreateUser,
  adminGetUserDetails,
  adminUpdateUserStatus,
  adminUpdateUserPassword,
  adminDeleteUser,
  adminListUserDatasets,
  adminListUserAgents,

  adminListServices,
  adminShowServiceDetails,

  adminListRoles,
  adminListRolesWithPermission,
  adminCreateRole,
  adminDeleteRole,
  adminUpdateRoleDescription,
  adminGetRolePermissions,
  adminAssignRolePermissions,
  adminRevokeRolePermissions,

  adminGetUserPermissions,
  adminUpdateUserRole,

  adminListResources,

  adminListWhitelist,
  adminCreateWhitelistEntry,
  adminUpdateWhitelistEntry,
  adminDeleteWhitelistEntry,
  adminImportWhitelist,

  adminGetSystemVersion,
} = api;

type ResponseData<D = NonNullable<unknown>> = {
  code: number;
  message: string;
  data: D;
};

export const login = (params: { email: string; password: string }) =>
  request.post<ResponseData<AdminService.LoginData>>(adminLogin, params);
export const logout = () => request.get<ResponseData<boolean>>(adminLogout);
export const listUsers = () =>
  request.get<ResponseData<AdminService.ListUsersItem[]>>(adminListUsers, {});

export const createUser = (email: string, password: string) =>
  request.post<ResponseData<boolean>>(adminCreateUser, {
    username: email,
    password,
  });
export const getUserDetails = (email: string) =>
  request.get<ResponseData<[AdminService.UserDetail]>>(
    adminGetUserDetails(email),
  );
export const listUserDatasets = (email: string) =>
  request.get<ResponseData<AdminService.ListUserDatasetItem[]>>(
    adminListUserDatasets(email),
  );
export const listUserAgents = (email: string) =>
  request.get<ResponseData<AdminService.ListUserAgentItem[]>>(
    adminListUserAgents(email),
  );
export const updateUserStatus = (email: string, status: 'on' | 'off') =>
  request.put(adminUpdateUserStatus(email), { activate_status: status });
export const updateUserPassword = (email: string, password: string) =>
  request.put(adminUpdateUserPassword(email), { new_password: password });
export const deleteUser = (email: string) =>
  request.delete(adminDeleteUser(email));

export const listServices = () =>
  request.get<ResponseData<AdminService.ListServicesItem[]>>(adminListServices);
export const showServiceDetails = (serviceId: number) =>
  request.get<ResponseData<AdminService.ServiceDetail>>(
    adminShowServiceDetails(String(serviceId)),
  );

export const createRole = (params: {
  roleName: string;
  description?: string;
}) =>
  request.post<ResponseData<AdminService.RoleDetail>>(adminCreateRole, params);
export const updateRoleDescription = (role: string, description: string) =>
  request.put<ResponseData<AdminService.RoleDetail>>(
    adminUpdateRoleDescription(role),
    { description },
  );
export const deleteRole = (role: string) =>
  request.delete<ResponseData<ResponseData<never>>>(adminDeleteRole(role));
export const listRoles = () =>
  request.get<
    ResponseData<{ roles: AdminService.ListRoleItem[]; total: number }>
  >(adminListRoles);
export const listRolesWithPermission = () =>
  request.get<
    ResponseData<{
      roles: AdminService.ListRoleItemWithPermission[];
      total: number;
    }>
  >(adminListRolesWithPermission);
export const getRolePermissions = (role: string) =>
  request.get<ResponseData<AdminService.RoleDetailWithPermission>>(
    adminGetRolePermissions(role),
  );
export const assignRolePermissions = (
  role: string,
  permissions: Partial<AdminService.AssignRolePermissionsInput>,
) =>
  request.post<ResponseData<never>>(adminAssignRolePermissions(role), {
    new_permissions: permissions,
  });
export const revokeRolePermissions = (
  role: string,
  permissions: Partial<AdminService.RevokeRolePermissionInput>,
) =>
  request.delete<ResponseData<never>>(adminRevokeRolePermissions(role), {
    data: { revoke_permissions: permissions },
  });

export const updateUserRole = (username: string, role: string) =>
  request.put<ResponseData<never>>(adminUpdateUserRole(username), {
    role_name: role,
  });
export const getUserPermissions = (username: string) =>
  request.get<ResponseData<AdminService.UserDetailWithPermission>>(
    adminGetUserPermissions(username),
  );
export const listResources = () =>
  request.get<ResponseData<AdminService.ResourceType>>(adminListResources);

export const listWhitelist = () =>
  request.get<
    ResponseData<{
      total: number;
      white_list: AdminService.ListWhitelistItem[];
    }>
  >(adminListWhitelist);

export const createWhitelistEntry = (email: string) =>
  request.post<ResponseData<never>>(adminCreateWhitelistEntry, { email });

export const updateWhitelistEntry = (id: number, email: string) =>
  request.put<ResponseData<never>>(adminUpdateWhitelistEntry(id), { email });

export const deleteWhitelistEntry = (email: string) =>
  request.delete<ResponseData<never>>(adminDeleteWhitelistEntry(email));

export const importWhitelistFromExcel = (file: File) => {
  const fd = new FormData();

  fd.append('file', file);

  return request.post<ResponseData<never>>(adminImportWhitelist, fd);
};

export const getSystemVersion = () =>
  request.get<ResponseData<{ version: string }>>(adminGetSystemVersion);

```

## High-Level Overview

  // @ts-ignore

## Detailed Walkthrough

### Exports (29)

- `login`: Exported entity
- `logout`: Exported entity
- `listUsers`: Exported entity
- `createUser`: Exported entity
- `getUserDetails`: Exported entity
- `listUserDatasets`: Exported entity
- `listUserAgents`: Exported entity
- `updateUserStatus`: Exported entity
- `updateUserPassword`: Exported entity
- `deleteUser`: Exported entity
- `listServices`: Exported entity
- `showServiceDetails`: Exported entity
- `createRole`: Exported entity
- `updateRoleDescription`: Exported entity
- `deleteRole`: Exported entity
- `listRoles`: Exported entity
- `listRolesWithPermission`: Exported entity
- `getRolePermissions`: Exported entity
- `assignRolePermissions`: Exported entity
- `revokeRolePermissions`: Exported entity

### Functions (31)

- `request()`: Function definition
- `newConfig()`: Function definition
- `login()`: Function definition
- `logout()`: Function definition
- `listUsers()`: Function definition
- `createUser()`: Function definition
- `getUserDetails()`: Function definition
- `listUserDatasets()`: Function definition
- `listUserAgents()`: Function definition
- `updateUserStatus()`: Function definition
- `updateUserPassword()`: Function definition
- `deleteUser()`: Function definition
- `listServices()`: Function definition
- `showServiceDetails()`: Function definition
- `createRole()`: Function definition
- `updateRoleDescription()`: Function definition
- `deleteRole()`: Function definition
- `listRoles()`: Function definition
- `listRolesWithPermission()`: Function definition
- `getRolePermissions()`: Function definition

### Imports (10)

- `import { message, notification } from 'antd';`
- `import axios from 'axios';`
- `import { history } from 'umi';`
- `import { Authorization } from '@/constants/authorization';`
- `import i18n from '@/locales/config';`
- `import { Routes } from '@/routes';`
- `import api from '@/utils/api';`
- `import authorizationUtil, {`
- `import { convertTheKeysOfTheObjectToSnake } from '@/utils/common-util';`
- `import { ResultCode, RetcodeMessage } from '@/utils/request';`

## Code Structure Analysis

- Total lines: 264
- Blank lines: 36 (13.6%)
- Comment lines: ~1 (0.4%)
- Code lines: ~227


## Dependencies and Imports

- `antd`
- `axios`
- `umi`
- `@/constants/authorization`
- `@/locales/config`
- `@/routes`
- `@/utils/api`
- `@/utils/common-util`
- `@/utils/request`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/services`.

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

- Other files in `web/src/services/` directory
- Potential test file: `test_admin-service.ts`

## Keywords

@/constants/authorization, @/locales/config, @/routes, @/utils/api, @/utils/common-util, @/utils/request, Admin, AdminService, AssignRolePermissionsInput, Authorization, Failed, File, FormData, ListRoleItem, ListRoleItemWithPermission, ListServicesItem, ListUserAgentItem, ListUserDatasetItem, ListUsersItem, ListWhitelistItem, LoginData, NonNullable, Partial, ResourceType, ResponseData, ResultCode, RetcodeMessage, RevokeRolePermissionInput, RoleDetail, RoleDetailWithPermission, Routes, ServiceDetail, String, TypeScript, UserDetail, UserDetailWithPermission, antd, assignRolePermissions, axios, createRole, createUser, createWhitelistEntry, data, deleteRole, deleteUser, deleteWhitelistEntry, fd, getRolePermissions, getSystemVersion, getUserDetails...

---
*Generated by RAGFlow Repository Documentation Generator*
