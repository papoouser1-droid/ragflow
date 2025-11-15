# Documentation: web/src/hooks/use-user-setting-request.tsx

## File Metadata

- **Path**: `web/src/hooks/use-user-setting-request.tsx`
- **Size**: 12197 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/use-user-setting-request.tsx`.

## Original Source Code

```tsx
import message from '@/components/ui/message';
import { LanguageTranslationMap } from '@/constants/common';
import { ResponseGetType } from '@/interfaces/database/base';
import { IToken } from '@/interfaces/database/chat';
import { ITenantInfo } from '@/interfaces/database/knowledge';
import { ILangfuseConfig } from '@/interfaces/database/system';
import {
  ISystemStatus,
  ITenant,
  ITenantUser,
  IUserInfo,
} from '@/interfaces/database/user-setting';
import { ISetLangfuseConfigRequestBody } from '@/interfaces/request/system';
import userService, {
  addTenantUser,
  agreeTenant,
  deleteTenantUser,
  listTenant,
  listTenantUser,
} from '@/services/user-service';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { Modal } from 'antd';
import DOMPurify from 'dompurify';
import { isEmpty } from 'lodash';
import { useCallback, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { history } from 'umi';

export const enum UserSettingApiAction {
  UserInfo = 'userInfo',
  TenantInfo = 'tenantInfo',
  SaveSetting = 'saveSetting',
  FetchManualSystemTokenList = 'fetchManualSystemTokenList',
  FetchSystemTokenList = 'fetchSystemTokenList',
  RemoveSystemToken = 'removeSystemToken',
  CreateSystemToken = 'createSystemToken',
  ListTenantUser = 'listTenantUser',
  AddTenantUser = 'addTenantUser',
  DeleteTenantUser = 'deleteTenantUser',
  ListTenant = 'listTenant',
  AgreeTenant = 'agreeTenant',
  SetLangfuseConfig = 'setLangfuseConfig',
  DeleteLangfuseConfig = 'deleteLangfuseConfig',
  FetchLangfuseConfig = 'fetchLangfuseConfig',
}

export const useFetchUserInfo = (): ResponseGetType<IUserInfo> => {
  const { i18n } = useTranslation();

  const { data, isFetching: loading } = useQuery({
    queryKey: [UserSettingApiAction.UserInfo],
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.user_info();
      if (data.code === 0) {
        i18n.changeLanguage(
          LanguageTranslationMap[
            data.data.language as keyof typeof LanguageTranslationMap
          ],
        );
      }
      return data?.data ?? {};
    },
  });

  return { data, loading };
};

export const useFetchTenantInfo = (
  showEmptyModelWarn = false,
): ResponseGetType<ITenantInfo> => {
  const { t } = useTranslation();
  const { data, isFetching: loading } = useQuery({
    queryKey: [UserSettingApiAction.TenantInfo],
    initialData: {},
    gcTime: 0,
    queryFn: async () => {
      const { data: res } = await userService.get_tenant_info();
      if (res.code === 0) {
        // llm_id is chat_id
        // asr_id is speech2txt
        const { data } = res;
        if (
          showEmptyModelWarn &&
          (isEmpty(data.embd_id) || isEmpty(data.llm_id))
        ) {
          Modal.warning({
            title: t('common.warn'),
            content: (
              <div
                dangerouslySetInnerHTML={{
                  __html: DOMPurify.sanitize(t('setting.modelProvidersWarn')),
                }}
              ></div>
            ),
            onOk() {
              history.push('/user-setting/model');
            },
          });
        }
        data.chat_id = data.llm_id;
        data.speech2text_id = data.asr_id;

        return data;
      }

      return res;
    },
  });

  return { data, loading };
};

export const useSelectParserList = (): Array<{
  value: string;
  label: string;
}> => {
  const { data: tenantInfo } = useFetchTenantInfo(true);

  const parserList = useMemo(() => {
    const parserArray: Array<string> = tenantInfo?.parser_ids?.split(',') ?? [];
    return parserArray.map((x) => {
      const arr = x.split(':');
      return { value: arr[0], label: arr[1] };
    });
  }, [tenantInfo]);

  return parserList;
};

export const useSaveSetting = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.SaveSetting],
    mutationFn: async (
      userInfo: { new_password: string } | Partial<IUserInfo>,
    ) => {
      const { data } = await userService.setting(userInfo);
      if (data.code === 0) {
        message.success(t('message.modified'));
        queryClient.invalidateQueries({ queryKey: ['userInfo'] });
      }
      return data?.code;
    },
  });

  return { data, loading, saveSetting: mutateAsync };
};

export const useFetchSystemVersion = () => {
  const [version, setVersion] = useState('');
  const [loading, setLoading] = useState(false);

  const fetchSystemVersion = useCallback(async () => {
    try {
      setLoading(true);
      const { data } = await userService.getSystemVersion();
      if (data.code === 0) {
        setVersion(data.data);
        setLoading(false);
      }
    } catch (error) {
      setLoading(false);
    }
  }, []);

  return { fetchSystemVersion, version, loading };
};

export const useFetchSystemStatus = () => {
  const [systemStatus, setSystemStatus] = useState<ISystemStatus>(
    {} as ISystemStatus,
  );
  const [loading, setLoading] = useState(false);

  const fetchSystemStatus = useCallback(async () => {
    setLoading(true);
    const { data } = await userService.getSystemStatus();
    if (data.code === 0) {
      setSystemStatus(data.data);
      setLoading(false);
    }
  }, []);

  return {
    systemStatus,
    fetchSystemStatus,
    loading,
  };
};

export const useFetchManualSystemTokenList = () => {
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.FetchManualSystemTokenList],
    mutationFn: async () => {
      const { data } = await userService.listToken();

      return data?.data ?? [];
    },
  });

  return { data, loading, fetchSystemTokenList: mutateAsync };
};

export const useFetchSystemTokenList = () => {
  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<IToken[]>({
    queryKey: [UserSettingApiAction.FetchSystemTokenList],
    initialData: [],
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.listToken();

      return data?.data ?? [];
    },
  });

  return { data, loading, refetch };
};

export const useRemoveSystemToken = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.RemoveSystemToken],
    mutationFn: async (token: string) => {
      const { data } = await userService.removeToken({}, token);
      if (data.code === 0) {
        message.success(t('message.deleted'));
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.FetchSystemTokenList],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, removeToken: mutateAsync };
};

export const useCreateSystemToken = () => {
  const queryClient = useQueryClient();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.CreateSystemToken],
    mutationFn: async (params: Record<string, any>) => {
      const { data } = await userService.createToken(params);
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.FetchSystemTokenList],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, createToken: mutateAsync };
};

export const useListTenantUser = () => {
  const { data: tenantInfo } = useFetchTenantInfo();
  const tenantId = tenantInfo.tenant_id;
  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<ITenantUser[]>({
    queryKey: [UserSettingApiAction.ListTenantUser, tenantId],
    initialData: [],
    gcTime: 0,
    enabled: !!tenantId,
    queryFn: async () => {
      const { data } = await listTenantUser(tenantId);

      return data?.data ?? [];
    },
  });

  return { data, loading, refetch };
};

export const useAddTenantUser = () => {
  const { data: tenantInfo } = useFetchTenantInfo();
  const queryClient = useQueryClient();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.AddTenantUser],
    mutationFn: async (email: string) => {
      const { data } = await addTenantUser(tenantInfo.tenant_id, email);
      if (data.code === 0) {
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.ListTenantUser],
        });
      }
      return data?.code;
    },
  });

  return { data, loading, addTenantUser: mutateAsync };
};

export const useDeleteTenantUser = () => {
  const { data: tenantInfo } = useFetchTenantInfo();
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.DeleteTenantUser],
    mutationFn: async ({
      userId,
      tenantId,
    }: {
      userId: string;
      tenantId?: string;
    }) => {
      const { data } = await deleteTenantUser({
        tenantId: tenantId ?? tenantInfo.tenant_id,
        userId,
      });
      if (data.code === 0) {
        message.success(t('message.deleted'));
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.ListTenantUser],
        });
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.ListTenant],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, deleteTenantUser: mutateAsync };
};

export const useListTenant = () => {
  const { data: tenantInfo } = useFetchTenantInfo();
  const tenantId = tenantInfo.tenant_id;
  const {
    data,
    isFetching: loading,
    refetch,
  } = useQuery<ITenant[]>({
    queryKey: [UserSettingApiAction.ListTenant, tenantId],
    initialData: [],
    gcTime: 0,
    enabled: !!tenantId,
    queryFn: async () => {
      const { data } = await listTenant();

      return data?.data ?? [];
    },
  });

  return { data, loading, refetch };
};

export const useAgreeTenant = () => {
  const queryClient = useQueryClient();
  const { t } = useTranslation();

  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.AgreeTenant],
    mutationFn: async (tenantId: string) => {
      const { data } = await agreeTenant(tenantId);
      if (data.code === 0) {
        message.success(t('message.operated'));
        queryClient.invalidateQueries({
          queryKey: [UserSettingApiAction.ListTenant],
        });
      }
      return data?.data ?? [];
    },
  });

  return { data, loading, agreeTenant: mutateAsync };
};

export const useSetLangfuseConfig = () => {
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.SetLangfuseConfig],
    mutationFn: async (params: ISetLangfuseConfigRequestBody) => {
      const { data } = await userService.setLangfuseConfig(params);
      if (data.code === 0) {
        message.success(t('message.operated'));
      }
      return data?.code;
    },
  });

  return { data, loading, setLangfuseConfig: mutateAsync };
};

export const useDeleteLangfuseConfig = () => {
  const { t } = useTranslation();
  const {
    data,
    isPending: loading,
    mutateAsync,
  } = useMutation({
    mutationKey: [UserSettingApiAction.DeleteLangfuseConfig],
    mutationFn: async () => {
      const { data } = await userService.deleteLangfuseConfig();
      if (data.code === 0) {
        message.success(t('message.deleted'));
      }
      return data?.code;
    },
  });

  return { data, loading, deleteLangfuseConfig: mutateAsync };
};

export const useFetchLangfuseConfig = () => {
  const { data, isFetching: loading } = useQuery<ILangfuseConfig>({
    queryKey: [UserSettingApiAction.FetchLangfuseConfig],
    gcTime: 0,
    queryFn: async () => {
      const { data } = await userService.getLangfuseConfig();

      return data?.data;
    },
  });

  return { data, loading };
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/use-user-setting-request.tsx` is located in the `web/src/hooks` directory.

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
