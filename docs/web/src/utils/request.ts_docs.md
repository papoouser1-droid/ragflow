# Documentation: web/src/utils/request.ts

## File Metadata

- **Path**: `web/src/utils/request.ts`
- **Size**: 3490 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/utils/request.ts`.

## Original Source Code

```ts
import { Authorization } from '@/constants/authorization';
import { ResponseType } from '@/interfaces/database/base';
import i18n from '@/locales/config';
import authorizationUtil, {
  getAuthorization,
  redirectToLogin,
} from '@/utils/authorization-util';
import { message, notification } from 'antd';
import { RequestMethod, extend } from 'umi-request';
import { convertTheKeysOfTheObjectToSnake } from './common-util';

const FAILED_TO_FETCH = 'Failed to fetch';

export const RetcodeMessage = {
  200: i18n.t('message.200'),
  201: i18n.t('message.201'),
  202: i18n.t('message.202'),
  204: i18n.t('message.204'),
  400: i18n.t('message.400'),
  401: i18n.t('message.401'),
  403: i18n.t('message.403'),
  404: i18n.t('message.404'),
  406: i18n.t('message.406'),
  410: i18n.t('message.410'),
  413: i18n.t('message.413'),
  422: i18n.t('message.422'),
  500: i18n.t('message.500'),
  502: i18n.t('message.502'),
  503: i18n.t('message.503'),
  504: i18n.t('message.504'),
};
export type ResultCode =
  | 200
  | 201
  | 202
  | 204
  | 400
  | 401
  | 403
  | 404
  | 406
  | 410
  | 413
  | 422
  | 500
  | 502
  | 503
  | 504;

const errorHandler = (error: {
  response: Response;
  message: string;
}): Response => {
  const { response } = error;
  if (error.message === FAILED_TO_FETCH) {
    notification.error({
      description: i18n.t('message.networkAnomalyDescription'),
      message: i18n.t('message.networkAnomaly'),
    });
  } else {
    if (response && response.status) {
      const errorText =
        RetcodeMessage[response.status as ResultCode] || response.statusText;
      const { status, url } = response;
      notification.error({
        message: `${i18n.t('message.requestError')} ${status}: ${url}`,
        description: errorText,
      });
    }
  }
  return response ?? { data: { code: 1999 } };
};

const request: RequestMethod = extend({
  errorHandler,
  timeout: 300000,
  getResponse: true,
});

request.interceptors.request.use((url: string, options: any) => {
  const data = convertTheKeysOfTheObjectToSnake(options.data);
  const params = convertTheKeysOfTheObjectToSnake(options.params);

  return {
    url,
    options: {
      ...options,
      data,
      params,
      headers: {
        ...(options.skipToken
          ? undefined
          : { [Authorization]: getAuthorization() }),
        ...options.headers,
      },
      interceptors: true,
    },
  };
});

request.interceptors.response.use(async (response: Response, options) => {
  if (response?.status === 413 || response?.status === 504) {
    message.error(RetcodeMessage[response?.status as ResultCode]);
  }

  if (options.responseType === 'blob') {
    return response;
  }

  const data: ResponseType = await response?.clone()?.json();
  if (data?.code === 100) {
    message.error(data?.message);
  } else if (data?.code === 401) {
    notification.error({
      message: data?.message,
      description: data?.message,
      duration: 3,
    });
    authorizationUtil.removeAll();
    redirectToLogin();
  } else if (data?.code !== 0) {
    notification.error({
      message: `${i18n.t('message.hint')} : ${data?.code}`,
      description: data?.message,
      duration: 3,
    });
  }
  return response;
});

export default request;

export const get = (url: string) => {
  return request.get(url);
};

export const post = (url: string, body: any) => {
  return request.post(url, { data: body });
};

export const drop = () => {};

export const put = () => {};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/utils/request.ts` is located in the `web/src/utils` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to utils.

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

- [api.ts](api.ts_docs.md)
- [authorization-util.ts](authorization-util.ts_docs.md)
- [canvas-util.tsx](canvas-util.tsx_docs.md)
- [chat.ts](chat.ts_docs.md)
- [common-util.ts](common-util.ts_docs.md)
- [component-util.ts](component-util.ts_docs.md)
- [dataset-util.ts](dataset-util.ts_docs.md)
- [date.ts](date.ts_docs.md)
- [document-util.ts](document-util.ts_docs.md)
- [dom-util.ts](dom-util.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
