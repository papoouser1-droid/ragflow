# File Documentation: web/src/hooks/login-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/login-hooks.ts`
- **Extension**: `.ts`
- **Lines**: 153
- **Characters**: 3,906
- **Size**: 3,906 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import message from '@/components/ui/message';
import { Authorization } from '@/constants/authorization';
import userService, {
  getLoginChannels,
  loginWithChannel,
} from '@/services/user-service';
import authorizationUtil, { redirectToLogin } from '@/utils/authorization-util';
import { useMutation, useQuery } from '@tanstack/react-query';
import { Form } from 'antd';
import { FormInstance } from 'antd/lib';
import { useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';

export interface ILoginRequestBody {
  email: string;
  password: string;
}

export interface IRegisterRequestBody extends ILoginRequestBody {
  nickname: string;
}

export interface ILoginChannel {
  channel: string;
  display_name: string;
  icon: string;
}

export const useLoginChannels = () => {
  const { data, isLoading } = useQuery({
    queryKey: ['loginChannels'],
    queryFn: async () => {
      const { data: res = {} } = await getLoginChannels();
      return res.data || [];
    },
  });

  return { channels: data as ILoginChannel[], loading: isLoading };
};

export const useLoginWithChannel = () => {
  const { isPending: loading, mutateAsync } = useMutation({
    mutationKey: ['loginWithChannel'],
    mutationFn: async (channel: string) => {
      loginWithChannel(channel);
      return Promise.resolve();
    },
  });

  return { loading, login: mutateAsync };
};

export const useLogin = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['login'],
    mutationFn: async (params: { email: string; password: string }) => {
      const { data: res = {}, response } = await userService.login(params);
      if (res.code === 0) {
        const { data } = res;
        const authorization = response.headers.get(Authorization);
        const token = data.access_token;
        const userInfo = {
          avatar: data.avatar,
          name: data.nickname,
          email: data.email,
        };
        authorizationUtil.setItems({
          Authorization: authorization,
          userInfo: JSON.stringify(userInfo),
          Token: token,
        });
      }
      return res.code;
    },
  });

  return { data, loading, login: mutateAsync };
};

export const useRegister = () => {
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['register'],
    mutationFn: async (params: {
      email: string;
      password: string;
      nickname: string;
    }) => {
      const { data = {} } = await userService.register(params);
      if (data.code === 0) {
        message.success(t('message.registered'));
      } else if (
        data.message &&
        data.message.includes('registration is disabled')
      ) {
        message.error(
          t('message.registerDisabled') || 'User registration is disabled',
        );
      }
      return data.code;
    },
  });

  return { data, loading, register: mutateAsync };
};

export const useLogout = () => {
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: ['logout'],
    mutationFn: async () => {
      const { data = {} } = await userService.logout();
      if (data.code === 0) {
        message.success(t('message.logout'));
        authorizationUtil.removeAll();
        redirectToLogin();
      }
      return data.code;
    },
  });

  return { data, loading, logout: mutateAsync };
};

export const useHandleSubmittable = (form: FormInstance) => {
  const [submittable, setSubmittable] = useState<boolean>(false);

  // Watch all values
  const values = Form.useWatch([], form);

  useEffect(() => {
    form
      .validateFields({ validateOnly: true })
      .then(() => setSubmittable(true))
      .catch(() => setSubmittable(false));
  }, [form, values]);

  return { submittable };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/login-hooks.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 153 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (6)

- `useLoginChannels`: Exported entity
- `useLoginWithChannel`: Exported entity
- `useLogin`: Exported entity
- `useRegister`: Exported entity
- `useLogout`: Exported entity
- `useHandleSubmittable`: Exported entity

### Functions (7)

- `useLoginChannels()`: Function definition
- `useLoginWithChannel()`: Function definition
- `useLogin()`: Function definition
- `useRegister()`: Function definition
- `useLogout()`: Function definition
- `useHandleSubmittable()`: Function definition
- `values()`: Function definition

### Imports (9)

- `import message from '@/components/ui/message';`
- `import { Authorization } from '@/constants/authorization';`
- `import userService, {`
- `import authorizationUtil, { redirectToLogin } from '@/utils/authorization-util';`
- `import { useMutation, useQuery } from '@tanstack/react-query';`
- `import { Form } from 'antd';`
- `import { FormInstance } from 'antd/lib';`
- `import { useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 153
- Blank lines: 19 (12.4%)
- Comment lines: ~1 (0.7%)
- Code lines: ~133


## Dependencies and Imports

- `@/components/ui/message`
- `@/constants/authorization`
- `@/utils/authorization-util`
- `@tanstack/react-query`
- `antd`
- `antd/lib`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization
- Uses asynchronous patterns for better performance

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

- Other files in `web/src/hooks/` directory
- Potential test file: `test_login-hooks.ts`

## Keywords

@/components/ui/message, @/constants/authorization, @/utils/authorization-util, @tanstack/react-query, Authorization, Form, FormInstance, ILoginChannel, ILoginRequestBody, IRegisterRequestBody, JSON, Promise, Token, TypeScript, User, Watch, antd, antd/lib, authorization, react, react-i18next, tanstack, token, useHandleSubmittable, useLogin, useLoginChannels, useLoginWithChannel, useLogout, useRegister, userInfo, values

---
*Generated by RAGFlow Repository Documentation Generator*
