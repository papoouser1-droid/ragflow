# File Documentation: web/src/services/agent-service.ts

## File Metadata

- **Path**: `web/src/services/agent-service.ts`
- **Extension**: `.ts`
- **Lines**: 147
- **Characters**: 2,720
- **Size**: 2,720 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  IAgentLogsRequest,
  IPipeLineListRequest,
} from '@/interfaces/database/agent';
import api from '@/utils/api';
import { registerNextServer } from '@/utils/register-server';
import request from '@/utils/request';

const {
  getCanvasSSE,
  setCanvas,
  listCanvas,
  resetCanvas,
  removeCanvas,
  runCanvas,
  listTemplates,
  testDbConnect,
  getInputElements,
  debug,
  settingCanvas,
  uploadCanvasFile,
  trace,
  inputForm,
  fetchVersionList,
  fetchVersion,
  fetchCanvas,
  fetchAgentAvatar,
  fetchAgentLogs,
  fetchExternalAgentInputs,
  prompt,
  cancelDataflow,
  cancelCanvas,
} = api;

const methods = {
  fetchCanvas: {
    url: fetchCanvas,
    method: 'get',
  },
  getCanvasSSE: {
    url: getCanvasSSE,
    method: 'get',
  },
  setCanvas: {
    url: setCanvas,
    method: 'post',
  },
  fetchVersionList: {
    url: fetchVersionList,
    method: 'get',
  },
  fetchVersion: {
    url: fetchVersion,
    method: 'get',
  },
  listCanvas: {
    url: listCanvas,
    method: 'get',
  },
  resetCanvas: {
    url: resetCanvas,
    method: 'post',
  },
  removeCanvas: {
    url: removeCanvas,
    method: 'post',
  },
  runCanvas: {
    url: runCanvas,
    method: 'post',
  },
  listTemplates: {
    url: listTemplates,
    method: 'get',
  },
  testDbConnect: {
    url: testDbConnect,
    method: 'post',
  },
  getInputElements: {
    url: getInputElements,
    method: 'get',
  },
  debugSingle: {
    url: debug,
    method: 'post',
  },
  settingCanvas: {
    url: settingCanvas,
    method: 'post',
  },
  uploadCanvasFile: {
    url: uploadCanvasFile,
    method: 'post',
  },
  trace: {
    url: trace,
    method: 'get',
  },
  inputForm: {
    url: inputForm,
    method: 'get',
  },
  fetchAgentAvatar: {
    url: fetchAgentAvatar,
    method: 'get',
  },
  fetchAgentLogs: {
    url: fetchAgentLogs,
    method: 'get',
  },
  fetchExternalAgentInputs: {
    url: fetchExternalAgentInputs,
    method: 'get',
  },
  fetchPrompt: {
    url: prompt,
    method: 'get',
  },
  cancelDataflow: {
    url: cancelDataflow,
    method: 'put',
  },
  cancelCanvas: {
    url: cancelCanvas,
    method: 'put',
  },
} as const;

const agentService = registerNextServer<keyof typeof methods>(methods);

export const fetchTrace = (data: { canvas_id: string; message_id: string }) => {
  return request.get(methods.trace.url, { params: data });
};
export const fetchAgentLogsByCanvasId = (
  canvasId: string,
  params: IAgentLogsRequest,
) => {
  return request.get(methods.fetchAgentLogs.url(canvasId), { params: params });
};

export const fetchPipeLineList = (params: IPipeLineListRequest) => {
  return request.get(api.listCanvas, { params: params });
};

export default agentService;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/services/agent-service.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 147 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `fetchTrace`: Exported entity
- `fetchAgentLogsByCanvasId`: Exported entity
- `fetchPipeLineList`: Exported entity

### Functions (3)

- `fetchTrace()`: Function definition
- `fetchAgentLogsByCanvasId()`: Function definition
- `fetchPipeLineList()`: Function definition

### Imports (4)

- `import {`
- `import api from '@/utils/api';`
- `import { registerNextServer } from '@/utils/register-server';`
- `import request from '@/utils/request';`

## Code Structure Analysis

- Total lines: 147
- Blank lines: 7 (4.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~140


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
- Potential test file: `test_agent-service.ts`

## Keywords

@/utils/api, @/utils/register-server, @/utils/request, IAgentLogsRequest, IPipeLineListRequest, TypeScript, agentService, fetchAgentLogsByCanvasId, fetchPipeLineList, fetchTrace, methods

---
*Generated by RAGFlow Repository Documentation Generator*
