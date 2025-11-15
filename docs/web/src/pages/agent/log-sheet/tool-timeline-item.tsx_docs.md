# Documentation: web/src/pages/agent/log-sheet/tool-timeline-item.tsx

## File Metadata

- **Path**: `web/src/pages/agent/log-sheet/tool-timeline-item.tsx`
- **Size**: 8123 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/log-sheet/tool-timeline-item.tsx`.

## Original Source Code

```tsx
import {
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
import { cn } from '@/lib/utils';
import { isEmpty } from 'lodash';
import { Operator } from '../constant';
import OperatorIcon, { SVGIconMap } from '../operator-icon';
import {
  JsonViewer,
  toLowerCaseStringAndDeleteChar,
  typeMap,
} from './workflow-timeline';
type IToolIcon =
  | Operator.ArXiv
  | Operator.GitHub
  | Operator.Bing
  | Operator.DuckDuckGo
  | Operator.Google
  | Operator.GoogleScholar
  | Operator.PubMed
  | Operator.TavilyExtract
  | Operator.TavilySearch
  | Operator.Wikipedia
  | Operator.YahooFinance
  | Operator.WenCai
  | Operator.Crawler;

const capitalizeWords = (str: string, separator: string = '_'): string[] => {
  if (!str) return [''];

  const resultStrArr = str.split(separator).map((word) => {
    return word.charAt(0).toUpperCase() + word.slice(1).toLowerCase();
  });
  return resultStrArr;
};
const changeToolName = (toolName: any) => {
  const name = 'Agent ' + capitalizeWords(toolName).join(' ');
  return name;
};
const ToolTimelineItem = ({
  tools,
  sendLoading = false,
  isShare = false,
}: {
  tools: Record<string, any>[];
  sendLoading: boolean;
  isShare?: boolean;
}) => {
  if (!tools || tools.length === 0 || !Array.isArray(tools)) return null;
  const blackList = ['add_memory', 'gen_citations'];
  const filteredTools = tools.filter(
    (tool) => !blackList.includes(tool.tool_name),
  );

  const parentName = (str: string, separator: string = '-->') => {
    if (!str) return '';
    const strs = str.split(separator);
    if (strs.length > 1) {
      return strs[strs.length - 1];
    } else {
      return str;
    }
  };
  return (
    <>
      {filteredTools?.map((tool, idx) => {
        const toolName = capitalizeWords(tool.tool_name, '_').join('');

        return (
          <TimelineItem
            key={'tool_' + idx}
            step={idx}
            className="group-data-[orientation=vertical]/timeline:ms-10 group-data-[orientation=vertical]/timeline:not-last:pb-8"
          >
            <TimelineHeader>
              <TimelineSeparator
                className="group-data-[orientation=vertical]/timeline:-left-7 group-data-[orientation=vertical]/timeline:h-[calc(100%-1.5rem-0.25rem)] group-data-[orientation=vertical]/timeline:translate-y-6.5 top-6"
                style={{
                  background:
                    idx < filteredTools.length - 1
                      ? 'repeating-linear-gradient( to bottom, rgba(76, 164, 231, 1), rgba(76, 164, 231, 1) 5px, transparent 5px, transparent 10px'
                      : 'rgba(76, 164, 231, 1)',
                  width: '1px',
                }}
              />

              <TimelineIndicator
                className={cn(
                  'group-data-completed/timeline-item:bg-primary group-data-completed/timeline-item:text-primary-foreground flex size-6 p-1 items-center justify-center group-data-[orientation=vertical]/timeline:-left-7',
                  {
                    'border border-blue-500': !(
                      idx >= filteredTools.length - 1 &&
                      tool.result === '...' &&
                      sendLoading
                    ),
                  },
                )}
              >
                <div className='relative after:content-[""] after:absolute after:inset-0 after:z-10 after:bg-transparent after:transition-all after:duration-300'>
                  <div className="absolute inset-0 z-10 flex items-center justify-center ">
                    <div
                      className={cn('rounded-full w-6 h-6', {
                        ' border-muted-foreground border-2 border-t-transparent animate-spin ':
                          idx >= filteredTools.length - 1 &&
                          tool.result === '...' &&
                          sendLoading,
                      })}
                    ></div>
                  </div>
                  <div className="size-6 flex items-center justify-center">
                    <OperatorIcon
                      className="size-4"
                      name={
                        (SVGIconMap[toolName as IToolIcon]
                          ? toolName
                          : 'Agent') as Operator
                      }
                    ></OperatorIcon>
                  </div>
                </div>
              </TimelineIndicator>
            </TimelineHeader>
            <TimelineContent className="text-foreground  rounded-lg border  mb-5">
              <section key={idx}>
                <Accordion
                  type="single"
                  collapsible
                  className="bg-bg-card px-3"
                >
                  <AccordionItem value={idx.toString()}>
                    <AccordionTrigger
                      hideDownIcon={isShare && isEmpty(tool.arguments)}
                    >
                      <div className="flex gap-2 items-center">
                        {!isShare && (
                          <span>
                            {parentName(tool.path) + ' '}
                            {capitalizeWords(tool.tool_name, '_').join(' ')}
                          </span>
                        )}
                        {isShare && (
                          <span>
                            {typeMap[
                              toLowerCaseStringAndDeleteChar(
                                tool.tool_name,
                              ) as keyof typeof typeMap
                            ] ?? changeToolName(tool.tool_name)}
                          </span>
                        )}
                        <span className="text-text-secondary text-xs">
                          {/* 0:00*/}
                          {tool.elapsed_time?.toString().slice(0, 6) || ''}
                          {tool.elapsed_time ? 's' : ''}
                        </span>
                        <span
                          className={cn(
                            'border-background  -end-1 -top-1 size-2 rounded-full bg-state--success',
                          )}
                        >
                          <span className="sr-only">Online</span>
                        </span>
                      </div>
                    </AccordionTrigger>
                    {!isShare && (
                      <AccordionContent>
                        <div className="space-y-2">
                          <JsonViewer
                            data={tool.result}
                            title="content"
                          ></JsonViewer>
                        </div>
                      </AccordionContent>
                    )}
                    {isShare && !isEmpty(tool.arguments) && (
                      <AccordionContent>
                        <div className="space-y-2 bg-muted p-2">
                          {tool &&
                            tool.arguments &&
                            Object.entries(tool.arguments).length &&
                            Object.entries(tool.arguments).map(([key, val]) => {
                              return (
                                <div key={key}>
                                  <div className="text-sm font-medium leading-none">
                                    {key}
                                  </div>
                                  <div className="text-sm text-muted-foreground mt-1">
                                    {val as string}
                                  </div>
                                </div>
                              );
                            })}
                        </div>
                      </AccordionContent>
                    )}
                  </AccordionItem>
                </Accordion>
              </section>
            </TimelineContent>
          </TimelineItem>
        );
      })}
    </>
  );
};

export default ToolTimelineItem;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/log-sheet/tool-timeline-item.tsx` is located in the `web/src/pages/agent/log-sheet` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to log-sheet.

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

- [data.ts](data.ts_docs.md)
- [index.tsx](index.tsx_docs.md)
- [workflow-timeline.tsx](workflow-timeline.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
