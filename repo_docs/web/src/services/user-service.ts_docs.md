# File Documentation: web/src/services/user-service.ts

## File Metadata

- **Path**: `web/src/services/user-service.ts`
- **Extension**: `.ts`
- **Lines**: 155
- **Characters**: 2,906
- **Size**: 2,906 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import api from '@/utils/api';
import registerServer from '@/utils/register-server';
import request, { post } from '@/utils/request';

const {
  login,
  logout,
  register,
  setting,
  user_info,
  tenant_info,
  factories_list,
  llm_list,
  my_llm,
  set_api_key,
  set_tenant_info,
  add_llm,
  delete_llm,
  enable_llm,
  deleteFactory,
  getSystemStatus,
  getSystemVersion,
  getSystemTokenList,
  removeSystemToken,
  createSystemToken,
  getSystemConfig,
  setLangfuseConfig,
} = api;

const methods = {
  login: {
    url: login,
    method: 'post',
  },
  logout: {
    url: logout,
    method: 'get',
  },
  register: {
    url: register,
    method: 'post',
  },
  setting: {
    url: setting,
    method: 'post',
  },
  user_info: {
    url: user_info,
    method: 'get',
  },
  get_tenant_info: {
    url: tenant_info,
    method: 'get',
  },
  set_tenant_info: {
    url: set_tenant_info,
    method: 'post',
  },
  factories_list: {
    url: factories_list,
    method: 'get',
  },
  llm_list: {
    url: llm_list,
    method: 'get',
  },
  my_llm: {
    url: my_llm,
    method: 'get',
  },
  set_api_key: {
    url: set_api_key,
    method: 'post',
  },
  add_llm: {
    url: add_llm,
    method: 'post',
  },
  delete_llm: {
    url: delete_llm,
    method: 'post',
  },
  enable_llm: {
    url: enable_llm,
    method: 'post',
  },
  getSystemStatus: {
    url: getSystemStatus,
    method: 'get',
  },
  getSystemVersion: {
    url: getSystemVersion,
    method: 'get',
  },
  deleteFactory: {
    url: deleteFactory,
    method: 'post',
  },
  listToken: {
    url: getSystemTokenList,
    method: 'get',
  },
  createToken: {
    url: createSystemToken,
    method: 'post',
  },
  removeToken: {
    url: removeSystemToken,
    method: 'delete',
  },
  getSystemConfig: {
    url: getSystemConfig,
    method: 'get',
  },
  setLangfuseConfig: {
    url: setLangfuseConfig,
    method: 'put',
  },
  getLangfuseConfig: {
    url: setLangfuseConfig,
    method: 'get',
  },
  deleteLangfuseConfig: {
    url: setLangfuseConfig,
    method: 'delete',
  },
} as const;

const userService = registerServer<keyof typeof methods>(methods, request);

export const getLoginChannels = () => request.get(api.login_channels);
export const loginWithChannel = (channel: string) =>
  (window.location.href = api.login_channel(channel));

export const listTenantUser = (tenantId: string) =>
  request.get(api.listTenantUser(tenantId));

export const addTenantUser = (tenantId: string, email: string) =>
  post(api.addTenantUser(tenantId), { email });

export const deleteTenantUser = ({
  tenantId,
  userId,
}: {
  tenantId: string;
  userId: string;
}) => request.delete(api.deleteTenantUser(tenantId, userId));

export const listTenant = () => request.get(api.listTenant);

export const agreeTenant = (tenantId: string) =>
  request.put(api.agreeTenant(tenantId));

export default userService;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/services/user-service.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 155 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (7)

- `getLoginChannels`: Exported entity
- `loginWithChannel`: Exported entity
- `listTenantUser`: Exported entity
- `addTenantUser`: Exported entity
- `deleteTenantUser`: Exported entity
- `listTenant`: Exported entity
- `agreeTenant`: Exported entity

### Functions (7)

- `getLoginChannels()`: Function definition
- `loginWithChannel()`: Function definition
- `listTenantUser()`: Function definition
- `addTenantUser()`: Function definition
- `deleteTenantUser()`: Function definition
- `listTenant()`: Function definition
- `agreeTenant()`: Function definition

### Imports (3)

- `import api from '@/utils/api';`
- `import registerServer from '@/utils/register-server';`
- `import request, { post } from '@/utils/request';`

## Code Structure Analysis

- Total lines: 155
- Blank lines: 11 (7.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~144


## Dependencies and Imports

- `@/utils/api`
- `@/utils/register-server`
- `@/utils/request`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/services`.

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

- Other files in `web/src/services/` directory
- Potential test file: `test_user-service.ts`

## Keywords

@/utils/api, @/utils/register-server, @/utils/request, TypeScript, addTenantUser, agreeTenant, deleteTenantUser, getLoginChannels, listTenant, listTenantUser, loginWithChannel, methods, userService

---
*Generated by RAGFlow Repository Documentation Generator*
