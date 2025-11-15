# File Documentation: web/src/pages/agent/hooks/use-save-graph.ts

## File Metadata

- **Path**: `web/src/pages/agent/hooks/use-save-graph.ts`
- **Extension**: `.ts`
- **Lines**: 94
- **Characters**: 2,656
- **Size**: 2,656 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import {
  useFetchAgent,
  useResetAgent,
  useSetAgent,
} from '@/hooks/use-agent-request';
import { GlobalVariableType } from '@/interfaces/database/agent';
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { formatDate } from '@/utils/date';
import { useDebounceEffect } from 'ahooks';
import { useCallback, useEffect, useState } from 'react';
import { useParams } from 'umi';
import useGraphStore from '../store';
import { useBuildDslData } from './use-build-dsl';

export const useSaveGraph = (showMessage: boolean = true) => {
  const { data } = useFetchAgent();
  const { setAgent, loading } = useSetAgent(showMessage);
  const { id } = useParams();
  const { buildDslData } = useBuildDslData();

  const saveGraph = useCallback(
    async (
      currentNodes?: RAGFlowNodeType[],
      otherParam?: { globalVariables: Record<string, GlobalVariableType> },
    ) => {
      return setAgent({
        id,
        title: data.title,
        dsl: buildDslData(currentNodes, otherParam),
      });
    },
    [setAgent, data, id, buildDslData],
  );

  return { saveGraph, loading };
};

export const useSaveGraphBeforeOpeningDebugDrawer = (show: () => void) => {
  const { saveGraph, loading } = useSaveGraph();
  const { resetAgent } = useResetAgent();

  const handleRun = useCallback(
    async (nextNodes?: RAGFlowNodeType[]) => {
      const saveRet = await saveGraph(nextNodes);
      if (saveRet?.code === 0) {
        // Call the reset api before opening the run drawer each time
        const resetRet = await resetAgent();
        // After resetting, all previous messages will be cleared.
        if (resetRet?.code === 0) {
          show();
        }
      }
    },
    [saveGraph, resetAgent, show],
  );

  return { handleRun, loading };
};

export const useWatchAgentChange = (chatDrawerVisible: boolean) => {
  const [time, setTime] = useState<string>();
  const nodes = useGraphStore((state) => state.nodes);
  const edges = useGraphStore((state) => state.edges);
  const { saveGraph } = useSaveGraph(false);
  const { data: flowDetail } = useFetchAgent();

  const setSaveTime = useCallback((updateTime: number) => {
    setTime(formatDate(updateTime));
  }, []);

  useEffect(() => {
    setSaveTime(flowDetail?.update_time);
  }, [flowDetail, setSaveTime]);

  const saveAgent = useCallback(async () => {
    if (!chatDrawerVisible) {
      const ret = await saveGraph();
      setSaveTime(ret.data.update_time);
    }
  }, [chatDrawerVisible, saveGraph, setSaveTime]);

  useDebounceEffect(
    () => {
      saveAgent();
    },
    [nodes, edges],
    {
      wait: 1000 * 20,
    },
  );

  return time;
};

```

## High-Level Overview

        // Call the reset api before opening the run drawer each time
        // After resetting, all previous messages will be cleared.

## Detailed Walkthrough

### Exports (3)

- `useSaveGraph`: Exported entity
- `useSaveGraphBeforeOpeningDebugDrawer`: Exported entity
- `useWatchAgentChange`: Exported entity

### Functions (10)

- `useSaveGraph()`: Function definition
- `saveGraph()`: Function definition
- `useSaveGraphBeforeOpeningDebugDrawer()`: Function definition
- `handleRun()`: Function definition
- `useWatchAgentChange()`: Function definition
- `nodes()`: Function definition
- `edges()`: Function definition
- `setSaveTime()`: Function definition
- `saveAgent()`: Function definition
- `ret()`: Function definition

### Imports (9)

- `import {`
- `import { GlobalVariableType } from '@/interfaces/database/agent';`
- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { formatDate } from '@/utils/date';`
- `import { useDebounceEffect } from 'ahooks';`
- `import { useCallback, useEffect, useState } from 'react';`
- `import { useParams } from 'umi';`
- `import useGraphStore from '../store';`
- `import { useBuildDslData } from './use-build-dsl';`

## Code Structure Analysis

- Total lines: 94
- Blank lines: 13 (13.8%)
- Comment lines: ~2 (2.1%)
- Code lines: ~79


## Dependencies and Imports

- `@/interfaces/database/agent`
- `@/interfaces/database/flow`
- `@/utils/date`
- `ahooks`
- `react`
- `umi`
- `../store`
- `./use-build-dsl`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

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
- Potential test file: `test_use-save-graph.ts`

## Keywords

../store, ./use-build-dsl, @/interfaces/database/agent, @/interfaces/database/flow, @/utils/date, After, Call, GlobalVariableType, RAGFlowNodeType, Record, TypeScript, ahooks, edges, handleRun, nodes, react, resetRet, ret, saveAgent, saveGraph, saveRet, setSaveTime, umi, useSaveGraph, useSaveGraphBeforeOpeningDebugDrawer, useWatchAgentChange

---
*Generated by RAGFlow Repository Documentation Generator*
