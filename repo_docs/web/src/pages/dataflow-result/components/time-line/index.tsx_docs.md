# File Documentation: web/src/pages/dataflow-result/components/time-line/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/time-line/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 96
- **Characters**: 2,441
- **Size**: 2,441 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { CustomTimeline, TimelineNode } from '@/components/originui/timeline';
import {
  Blocks,
  File,
  FilePlay,
  FileStack,
  Heading,
  ListPlus,
} from 'lucide-react';
import { useMemo } from 'react';
import { TimelineNodeType } from '../../constant';
import { IPipelineFileLogDetail } from '../../interface';

export type ITimelineNodeObj = {
  title: string;
  icon: JSX.Element;
  clickable?: boolean;
  type: TimelineNodeType;
};

export const TimelineNodeObj = {
  [TimelineNodeType.begin]: {
    title: 'File',
    icon: <File size={13} />,
    clickable: false,
  },
  [TimelineNodeType.parser]: {
    title: 'Parser',
    icon: <FilePlay size={13} />,
  },
  [TimelineNodeType.contextGenerator]: {
    title: 'Context Generator',
    icon: <FileStack size={13} />,
  },
  [TimelineNodeType.titleSplitter]: {
    title: 'Title Splitter',
    icon: <Heading size={13} />,
  },
  [TimelineNodeType.characterSplitter]: {
    title: 'Character Splitter',
    icon: <Blocks size={13} />,
  },
  [TimelineNodeType.tokenizer]: {
    title: 'Tokenizer',
    icon: <ListPlus size={13} />,
    clickable: false,
  },
};
export interface TimelineDataFlowProps {
  activeId: number | string;
  activeFunc: (id: number | string, step: TimelineNode) => void;
  data: IPipelineFileLogDetail;
  timelineNodes: TimelineNode[];
}
const TimelineDataFlow = ({
  activeFunc,
  activeId,
  data,
  timelineNodes,
}: TimelineDataFlowProps) => {
  // const [timelineNodeArr,setTimelineNodeArr] = useState<ITimelineNodeObj & {id: number | string}>()

  const activeStep = useMemo(() => {
    const index = timelineNodes.findIndex((node) => node.id === activeId);
    return index > -1 ? index + 1 : 0;
  }, [activeId, timelineNodes]);
  const handleStepChange = (step: number, id: string | number) => {
    activeFunc?.(
      id,
      timelineNodes.find((node) => node.id === activeStep) as TimelineNode,
    );
  };

  return (
    <div className="">
      <div>
        <CustomTimeline
          nodes={timelineNodes as TimelineNode[]}
          activeStep={activeStep}
          onStepChange={handleStepChange}
          orientation="horizontal"
          lineStyle="solid"
          nodeSize={24}
          activeStyle={{
            nodeSize: 30,
            iconColor: 'rgb(var(--accent-primary))',
            textColor: 'rgb(var(--accent-primary))',
          }}
        />
      </div>
    </div>
  );
};

export default TimelineDataFlow;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataflow-result/components/time-line/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 96 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `TimelineNodeObj`: Exported entity

### Functions (4)

- `TimelineDataFlow()`: Function definition
- `activeStep()`: Function definition
- `index()`: Function definition
- `handleStepChange()`: Function definition

### Imports (5)

- `import { CustomTimeline, TimelineNode } from '@/components/originui/timeline';`
- `import {`
- `import { useMemo } from 'react';`
- `import { TimelineNodeType } from '../../constant';`
- `import { IPipelineFileLogDetail } from '../../interface';`

## Code Structure Analysis

- Total lines: 96
- Blank lines: 6 (6.2%)
- Comment lines: ~1 (1.0%)
- Code lines: ~89


## Dependencies and Imports

- `@/components/originui/timeline`
- `react`
- `../../constant`
- `../../interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataflow-result/components/time-line`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataflow-result/components/time-line/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ../../interface, @/components/originui/timeline, Blocks, Character, Context, CustomTimeline, Element, File, FilePlay, FileStack, Generator, Heading, IPipelineFileLogDetail, ITimelineNodeObj, JSX, ListPlus, Parser, Splitter, TimelineDataFlow, TimelineDataFlowProps, TimelineNode, TimelineNodeObj, TimelineNodeType, Title, Tokenizer, TypeScript, activeStep, handleStepChange, index, react

---
*Generated by RAGFlow Repository Documentation Generator*
