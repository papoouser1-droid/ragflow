# File Documentation: web/src/utils/request.ts

## File Metadata

- **Path**: `web/src/utils/request.ts`
- **Extension**: `.ts`
- **Lines**: 144
- **Characters**: 3,490
- **Size**: 3,490 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/utils/request.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 144 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `RetcodeMessage`: Exported entity
- `get`: Exported entity
- `post`: Exported entity
- `drop`: Exported entity
- `put`: Exported entity

### Functions (6)

- `errorHandler()`: Function definition
- `params()`: Function definition
- `get()`: Function definition
- `post()`: Function definition
- `drop()`: Function definition
- `put()`: Function definition

### Imports (7)

- `import { Authorization } from '@/constants/authorization';`
- `import { ResponseType } from '@/interfaces/database/base';`
- `import i18n from '@/locales/config';`
- `import authorizationUtil, {`
- `import { message, notification } from 'antd';`
- `import { RequestMethod, extend } from 'umi-request';`
- `import { convertTheKeysOfTheObjectToSnake } from './common-util';`

## Code Structure Analysis

- Total lines: 144
- Blank lines: 15 (10.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~129


## Dependencies and Imports

- `@/constants/authorization`
- `@/interfaces/database/base`
- `@/locales/config`
- `antd`
- `umi-request`
- `./common-util`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/utils`.

This appears to be a UI component or frontend module.

## Performance & Complexity

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

- Other files in `web/src/utils/` directory
- Potential test file: `test_request.ts`

## Keywords

./common-util, @/constants/authorization, @/interfaces/database/base, @/locales/config, Authorization, FAILED_TO_FETCH, Failed, RequestMethod, Response, ResponseType, ResultCode, RetcodeMessage, TypeScript, antd, data, drop, errorHandler, errorText, get, params, post, put, request, umi-request

---
*Generated by RAGFlow Repository Documentation Generator*
