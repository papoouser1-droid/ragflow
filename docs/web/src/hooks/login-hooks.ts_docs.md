# Documentation: web/src/hooks/login-hooks.ts

## File Metadata

- **Path**: `web/src/hooks/login-hooks.ts`
- **Size**: 3906 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/login-hooks.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/login-hooks.ts` is located in the `web/src/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [auth-hooks.ts](auth-hooks.ts_docs.md)
- [chat-hooks.ts](chat-hooks.ts_docs.md)
- [chunk-hooks.ts](chunk-hooks.ts_docs.md)
- [common-hooks.tsx](common-hooks.tsx_docs.md)
- [document-hooks.ts](document-hooks.ts_docs.md)
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
