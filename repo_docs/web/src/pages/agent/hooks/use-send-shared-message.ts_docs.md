# File Documentation: web/src/pages/agent/hooks/use-send-shared-message.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-send-shared-message.ts`
- **Extension**: `.ts`
- **Lines**: 109
- **Characters**: 2,901
- **Size**: 2,901 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { SharedFrom } from '@/constants/chat';
import { useSetModalState } from '@/hooks/common-hooks';
import { useFetchExternalAgentInputs } from '@/hooks/use-agent-request';
import { IEventList } from '@/hooks/use-send-message';
import {
  buildRequestBody,
  useSendAgentMessage,
} from '@/pages/agent/chat/use-send-agent-message';
import { isEmpty } from 'lodash';
import trim from 'lodash/trim';
import { useCallback, useEffect, useRef, useState } from 'react';
import { useSearchParams } from 'umi';
import { AgentDialogueMode } from '../constant';

export const useSendButtonDisabled = (value: string) => {
  return trim(value) === '';
};

export const useGetSharedChatSearchParams = () => {
  const [searchParams] = useSearchParams();
  const data_prefix = 'data_';
  const data = Object.fromEntries(
    searchParams
      .entries()
      .filter(([key]) => key.startsWith(data_prefix))
      .map(([key, value]) => [key.replace(data_prefix, ''), value]),
  );
  return {
    from: searchParams.get('from') as SharedFrom,
    sharedId: searchParams.get('shared_id'),
    locale: searchParams.get('locale'),
    data: data,
    visibleAvatar: searchParams.get('visible_avatar')
      ? searchParams.get('visible_avatar') !== '1'
      : true,
  };
};

export const useSendNextSharedMessage = (
  addEventList: (data: IEventList, messageId: string) => void,
) => {
  const { from, sharedId: conversationId } = useGetSharedChatSearchParams();
  const url = `/api/v1/${from === SharedFrom.Agent ? 'agentbots' : 'chatbots'}/${conversationId}/completions`;
  const { data: inputsData } = useFetchExternalAgentInputs();

  const [params, setParams] = useState<any[]>([]);
  const sendedTaskMessage = useRef<boolean>(false);

  const isTaskMode = inputsData.mode === AgentDialogueMode.Task;

  const {
    visible: parameterDialogVisible,
    hideModal: hideParameterDialog,
    showModal: showParameterDialog,
  } = useSetModalState();

  const ret = useSendAgentMessage({
    url,
    addEventList,
    beginParams: params,
    isShared: true,
    isTaskMode,
  });

  const ok = useCallback(
    (params: any[]) => {
      if (isTaskMode) {
        const msgBody = buildRequestBody('');

        ret.sendMessage({
          message: msgBody,
          beginInputs: params,
        });
      } else {
        setParams(params);
      }

      hideParameterDialog();
    },
    [hideParameterDialog, isTaskMode, ret],
  );

  const runTask = useCallback(() => {
    if (
      isTaskMode &&
      isEmpty(inputsData?.inputs) &&
      !sendedTaskMessage.current
    ) {
      ok([]);
      sendedTaskMessage.current = true;
    }
  }, [inputsData?.inputs, isTaskMode, ok]);

  useEffect(() => {
    runTask();
  }, [runTask]);

  return {
    ...ret,
    hasError: false,
    parameterDialogVisible,
    inputsData,
    isTaskMode,
    hideParameterDialog,
    showParameterDialog,
    ok,
  };
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/hooks/use-send-shared-message.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 109 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `useSendButtonDisabled`: Exported entity
- `useGetSharedChatSearchParams`: Exported entity
- `useSendNextSharedMessage`: Exported entity

### Functions (6)

- `useSendButtonDisabled()`: Function definition
- `useGetSharedChatSearchParams()`: Function definition
- `data()`: Function definition
- `useSendNextSharedMessage()`: Function definition
- `ok()`: Function definition
- `runTask()`: Function definition

### Imports (10)

- `import { SharedFrom } from '@/constants/chat';`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useFetchExternalAgentInputs } from '@/hooks/use-agent-request';`
- `import { IEventList } from '@/hooks/use-send-message';`
- `import {`
- `import { isEmpty } from 'lodash';`
- `import trim from 'lodash/trim';`
- `import { useCallback, useEffect, useRef, useState } from 'react';`
- `import { useSearchParams } from 'umi';`
- `import { AgentDialogueMode } from '../constant';`

## Code Structure Analysis

- Total lines: 109
- Blank lines: 14 (12.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~95


## Dependencies and Imports

- `@/constants/chat`
- `@/hooks/common-hooks`
- `@/hooks/use-agent-request`
- `@/hooks/use-send-message`
- `lodash`
- `lodash/trim`
- `react`
- `umi`
- `../constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

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

- Other files in `web/src/pages/agent/hooks/` directory
- Potential test file: `test_use-send-shared-message.ts`

## Keywords

../constant, @/constants/chat, @/hooks/common-hooks, @/hooks/use-agent-request, @/hooks/use-send-message, Agent, AgentDialogueMode, IEventList, Object, SharedFrom, Task, TypeScript, data, data_prefix, isTaskMode, lodash, lodash/trim, msgBody, ok, react, ret, runTask, sendedTaskMessage, umi, url, useGetSharedChatSearchParams, useSendButtonDisabled, useSendNextSharedMessage

---
*Generated by RAGFlow Repository Documentation Generator*
