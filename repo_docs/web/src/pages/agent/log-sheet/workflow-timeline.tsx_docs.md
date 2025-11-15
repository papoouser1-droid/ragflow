# File Documentation: web/src/pages/agent/log-sheet/workflow-timeline.tsx

## File Metadata

- **Path**: `web/src/pages/agent/log-sheet/workflow-timeline.tsx`
- **Extension**: `.tsx`
- **Lines**: 356
- **Characters**: 12,811
- **Size**: 12,811 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import HightLightMarkdown from '@/components/highlight-markdown';
import {
  Timeline,
  TimelineContent,
  TimelineHeader,
  TimelineIndicator,
  TimelineItem,
  TimelineSeparator,
} from '@/components/originui/timeline';
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/components/ui/accordion';
import { useFetchMessageTrace } from '@/hooks/use-agent-request';
import {
  INodeData,
  INodeEvent,
  MessageEventType,
} from '@/hooks/use-send-message';
import { ITraceData } from '@/interfaces/database/agent';
import { cn } from '@/lib/utils';
import { t } from 'i18next';
import { get, isEmpty, isEqual, uniqWith } from 'lodash';
import { useCallback, useEffect, useMemo } from 'react';
import JsonView from 'react18-json-view';
import { Operator } from '../constant';
import { useCacheChatLog } from '../hooks/use-cache-chat-log';
import OperatorIcon from '../operator-icon';
import ToolTimelineItem from './tool-timeline-item';
type LogFlowTimelineProps = Pick<
  ReturnType<typeof useCacheChatLog>,
  'currentEventListWithoutMessage' | 'currentMessageId'
> & {
  canvasId?: string;
  sendLoading: boolean;
  isShare?: boolean;
};
export function JsonViewer({
  data,
  title,
}: {
  data: Record<string, any>;
  title: string;
}) {
  return (
    <section className="space-y-2">
      <div>{title}</div>
      <JsonView
        src={data}
        displaySize
        collapseStringsAfterLength={100000000000}
        className="w-full h-[200px] break-words overflow-auto scrollbar-auto p-2 bg-muted"
      />
    </section>
  );
}
export const typeMap = {
  begin: t('flow.logTimeline.begin'),
  agent: t('flow.logTimeline.agent'),
  retrieval: t('flow.logTimeline.retrieval'),
  message: t('flow.logTimeline.message'),
  awaitResponse: t('flow.logTimeline.awaitResponse'),
  switch: t('flow.logTimeline.switch'),
  iteration: t('flow.logTimeline.iteration'),
  categorize: t('flow.logTimeline.categorize'),
  code: t('flow.logTimeline.code'),
  textProcessing: t('flow.logTimeline.textProcessing'),
  tavilySearch: t('flow.logTimeline.tavilySearch'),
  tavilyExtract: t('flow.logTimeline.tavilyExtract'),
  exeSQL: t('flow.logTimeline.exeSQL'),
  google: t('flow.logTimeline.google'),
  duckDuckGo: t('flow.logTimeline.google'),
  wikipedia: t('flow.logTimeline.wikipedia'),
  googleScholar: t('flow.logTimeline.googleScholar'),
  arXiv: t('flow.logTimeline.googleScholar'),
  pubMed: t('flow.logTimeline.googleScholar'),
  gitHub: t('flow.logTimeline.gitHub'),
  email: t('flow.logTimeline.email'),
  httpRequest: t('flow.logTimeline.httpRequest'),
  wenCai: t('flow.logTimeline.wenCai'),
  yahooFinance: t('flow.logTimeline.yahooFinance'),
  userFillUp: t('flow.logTimeline.userFillUp'),
};
export const toLowerCaseStringAndDeleteChar = (
  str: string,
  char: string = '_',
) => str.toLowerCase().replace(/ /g, '').replaceAll(char, '');

// Convert all keys in typeMap to lowercase and output the new typeMap
export const typeMapLowerCase = Object.fromEntries(
  Object.entries(typeMap).map(([key, value]) => [
    toLowerCaseStringAndDeleteChar(key),
    value,
  ]),
);

function getInputsOrOutputs(
  nodeEventList: INodeData[],
  field: 'inputs' | 'outputs',
) {
  const inputsOrOutputs = nodeEventList.map((x) => get(x, field, {}));

  if (inputsOrOutputs.length < 2) {
    return inputsOrOutputs[0] || {};
  }

  return uniqWith(inputsOrOutputs, isEqual); // TODO: Violence should not be used to
}
export const WorkFlowTimeline = ({
  currentEventListWithoutMessage,
  currentMessageId,
  canvasId,
  sendLoading,
  isShare,
}: LogFlowTimelineProps) => {
  // const getNode = useGraphStore((state) => state.getNode);

  const {
    data: traceData,
    setMessageId,
    setISStopFetchTrace,
  } = useFetchMessageTrace(canvasId);

  useEffect(() => {
    setMessageId(currentMessageId);
  }, [currentMessageId, setMessageId]);
  const getNodeName = (nodeId: string) => {
    if ('begin' === nodeId) return t('flow.begin');
    return nodeId;
  };

  useEffect(() => {
    setISStopFetchTrace(!sendLoading);
  }, [sendLoading, setISStopFetchTrace]);

  const startedNodeList = useMemo(() => {
    const finish = currentEventListWithoutMessage?.some(
      (item) => item.event === MessageEventType.WorkflowFinished,
    );
    setISStopFetchTrace(finish || !sendLoading);
    const duplicateList = currentEventListWithoutMessage?.filter(
      (x) => x.event === MessageEventType.NodeStarted,
    ) as INodeEvent[];

    // Remove duplicate nodes
    return duplicateList?.reduce<Array<INodeEvent>>((pre, cur) => {
      if (pre.every((x) => x.data.component_id !== cur.data.component_id)) {
        pre.push(cur);
      }
      return pre;
    }, []);
  }, [currentEventListWithoutMessage, sendLoading, setISStopFetchTrace]);

  const getElapsedTime = (nodeId: string) => {
    if (nodeId === 'begin') {
      return '';
    }
    const data = currentEventListWithoutMessage?.find((x) => {
      return (
        x.data.component_id === nodeId &&
        x.event === MessageEventType.NodeFinished
      );
    });
    if (!data || data?.data.elapsed_time < 0.000001) {
      return '';
    }
    return data?.data.elapsed_time || '';
  };

  const hasTrace = useCallback(
    (componentId: string) => {
      if (Array.isArray(traceData)) {
        return traceData?.some((x) => x.component_id === componentId);
      }
      return false;
    },
    [traceData],
  );

  const filterTrace = useCallback(
    (componentId: string) => {
      const trace = traceData
        ?.filter((x) => x.component_id === componentId)
        .reduce<ITraceData['trace']>((pre, cur) => {
          pre.push(...cur.trace);

          return pre;
        }, []);
      return Array.isArray(trace) ? trace : [{}];
    },
    [traceData],
  );

  const filterFinishedNodeList = useCallback(
    (componentId: string) => {
      const nodeEventList = currentEventListWithoutMessage
        .filter(
          (x) =>
            x.event === MessageEventType.NodeFinished &&
            (x.data as INodeData)?.component_id === componentId,
        )
        .map((x) => x.data);

      return nodeEventList;
    },
    [currentEventListWithoutMessage],
  );

  return (
    <Timeline>
      {startedNodeList?.map((x, idx) => {
        const nodeDataList = filterFinishedNodeList(x.data.component_id);
        const finishNodeIds = nodeDataList.map(
          (x: INodeData) => x.component_id,
        );
        const inputs = getInputsOrOutputs(nodeDataList, 'inputs');
        const outputs = getInputsOrOutputs(nodeDataList, 'outputs');
        const nodeLabel = x.data.component_type;
        return (
          <>
            <TimelineItem
              key={idx}
              step={idx}
              className="group-data-[orientation=vertical]/timeline:ms-10 group-data-[orientation=vertical]/timeline:not-last:pb-8"
            >
              <TimelineHeader>
                <TimelineSeparator
                  className="group-data-[orientation=vertical]/timeline:-left-7 group-data-[orientation=vertical]/timeline:h-[calc(100%-1.5rem-0.25rem)] group-data-[orientation=vertical]/timeline:translate-y-6.5 top-6 bg-accent-primary"
                  style={{
                    background:
                      x.data.component_type === 'Agent'
                        ? 'repeating-linear-gradient( to bottom, rgba(76, 164, 231, 1), rgba(76, 164, 231, 1) 5px, transparent 5px, transparent 10px'
                        : '',
                  }}
                />

                <TimelineIndicator
                  className={cn(
                    ' group-data-completed/timeline-item:bg-primary group-data-completed/timeline-item:text-primary-foreground flex size-6 p-1  items-center justify-center group-data-[orientation=vertical]/timeline:-left-7',
                    {
                      'border border-blue-500': finishNodeIds.includes(
                        x.data.component_id,
                      ),
                    },
                  )}
                >
                  <div className='relative after:content-[""] after:absolute after:inset-0 after:z-10 after:bg-transparent after:transition-all after:duration-300'>
                    <div className="absolute inset-0 z-10 flex items-center justify-center ">
                      <div
                        className={cn('rounded-full w-6 h-6', {
                          ' border-muted-foreground border-2 border-t-transparent animate-spin ':
                            !finishNodeIds.includes(x.data.component_id) &&
                            sendLoading,
                        })}
                      ></div>
                    </div>
                    <div className="size-6 flex items-center justify-center">
                      <OperatorIcon
                        className="size-4"
                        name={nodeLabel as Operator}
                      ></OperatorIcon>
                    </div>
                  </div>
                </TimelineIndicator>
              </TimelineHeader>
              <TimelineContent className="text-foreground  rounded-lg border  mb-5">
                <section key={'content_' + idx}>
                  <Accordion
                    type="single"
                    collapsible
                    className="bg-bg-card px-3"
                  >
                    <AccordionItem value={idx.toString()}>
                      <AccordionTrigger
                        hideDownIcon={isShare && !x.data?.thoughts}
                      >
                        <div className="flex gap-2 items-center">
                          <span>
                            {!isShare && getNodeName(x.data?.component_name)}
                            {isShare &&
                              (typeMapLowerCase[
                                toLowerCaseStringAndDeleteChar(
                                  nodeLabel,
                                ) as keyof typeof typeMap
                              ] ??
                                nodeLabel)}
                          </span>
                          <span className="text-text-secondary text-xs">
                            {getElapsedTime(x.data.component_id)
                              .toString()
                              .slice(0, 6)}
                            {getElapsedTime(x.data.component_id) ? 's' : ''}
                          </span>
                          <span
                            className={cn(
                              'border-background  -end-1 -top-1 size-2 rounded-full',
                              { 'bg-state--success': isEmpty(x.data.error) },
                              { 'bg-state--error': !isEmpty(x.data.error) },
                            )}
                          >
                            <span className="sr-only">Online</span>
                          </span>
                        </div>
                      </AccordionTrigger>
                      {!isShare && (
                        <AccordionContent>
                          <div className="space-y-2">
                            {!isShare && (
                              <>
                                <JsonViewer
                                  data={inputs}
                                  title="Input"
                                ></JsonViewer>

                                <JsonViewer
                                  data={outputs}
                                  title={'Output'}
                                ></JsonViewer>
                              </>
                            )}
                          </div>
                        </AccordionContent>
                      )}
                      {isShare && x.data?.thoughts && (
                        <AccordionContent>
                          <div className="space-y-2">
                            <div className="w-full h-[200px] break-words overflow-auto scrollbar-auto p-2 bg-muted">
                              <HightLightMarkdown>
                                {x.data.thoughts || ''}
                              </HightLightMarkdown>
                            </div>
                          </div>
                        </AccordionContent>
                      )}
                    </AccordionItem>
                  </Accordion>
                </section>
              </TimelineContent>
            </TimelineItem>
            {hasTrace(x.data.component_id) && (
              <ToolTimelineItem
                key={'tool_' + idx}
                tools={filterTrace(x.data.component_id)}
                sendLoading={sendLoading}
                isShare={isShare}
              ></ToolTimelineItem>
            )}
          </>
        );
      })}
    </Timeline>
  );
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/log-sheet/workflow-timeline.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 356 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (5)

- `JsonViewer`: Exported entity
- `typeMap`: Exported entity
- `toLowerCaseStringAndDeleteChar`: Exported entity
- `typeMapLowerCase`: Exported entity
- `WorkFlowTimeline`: Exported entity

### Functions (19)

- `JsonViewer()`: Function definition
- `toLowerCaseStringAndDeleteChar()`: Function definition
- `typeMapLowerCase()`: Function definition
- `getInputsOrOutputs()`: Function definition
- `inputsOrOutputs()`: Function definition
- `WorkFlowTimeline()`: Function definition
- `getNode()`: Function definition
- `getNodeName()`: Function definition
- `startedNodeList()`: Function definition
- `finish()`: Function definition
- `duplicateList()`: Function definition
- `getElapsedTime()`: Function definition
- `data()`: Function definition
- `hasTrace()`: Function definition
- `filterTrace()`: Function definition
- `trace()`: Function definition
- `filterFinishedNodeList()`: Function definition
- `nodeEventList()`: Function definition
- `finishNodeIds()`: Function definition

### Imports (15)

- `import HightLightMarkdown from '@/components/highlight-markdown';`
- `import {`
- `import {`
- `import { useFetchMessageTrace } from '@/hooks/use-agent-request';`
- `import {`
- `import { ITraceData } from '@/interfaces/database/agent';`
- `import { cn } from '@/lib/utils';`
- `import { t } from 'i18next';`
- `import { get, isEmpty, isEqual, uniqWith } from 'lodash';`
- `import { useCallback, useEffect, useMemo } from 'react';`

## Code Structure Analysis

- Total lines: 356
- Blank lines: 19 (5.3%)
- Comment lines: ~3 (0.8%)
- Code lines: ~334


## Dependencies and Imports

- `@/components/highlight-markdown`
- `@/hooks/use-agent-request`
- `@/interfaces/database/agent`
- `@/lib/utils`
- `i18next`
- `lodash`
- `react`
- `react18-json-view`
- `../constant`
- `../hooks/use-cache-chat-log`
- `../operator-icon`
- `./tool-timeline-item`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/log-sheet`.

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

- Other files in `web/src/pages/agent/log-sheet/` directory
- Potential test file: `test_workflow-timeline.tsx`

## Keywords

../constant, ../hooks/use-cache-chat-log, ../operator-icon, ./tool-timeline-item, @/components/highlight-markdown, @/hooks/use-agent-request, @/interfaces/database/agent, @/lib/utils, Accordion, AccordionContent, AccordionItem, AccordionTrigger, Agent, Array, Convert, HightLightMarkdown, INodeData, INodeEvent, ITraceData, Input, JsonView, JsonViewer, LogFlowTimelineProps, MessageEventType, NodeFinished, NodeStarted, Object, Online, Operator, OperatorIcon, Output, Pick, Record, Remove, ReturnType, TODO, Timeline, TimelineContent, TimelineHeader, TimelineIndicator, TimelineItem, TimelineSeparator, ToolTimelineItem, TypeScript, Violence, WorkFlowTimeline, WorkflowFinished, data, duplicateList, filterFinishedNodeList...

---
*Generated by RAGFlow Repository Documentation Generator*
