# File Documentation: web/src/pages/admin/task-executor-detail.tsx

## File Metadata

- **Path**: `web/src/pages/admin/task-executor-detail.tsx`
- **Extension**: `.tsx`
- **Lines**: 163
- **Characters**: 4,586
- **Size**: 4,586 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import dayjs from 'dayjs';
import JsonView from 'react18-json-view';
import {
  Bar,
  BarChart,
  CartesianGrid,
  Legend,
  Rectangle,
  ResponsiveContainer,
  Tooltip,
  XAxis,
} from 'recharts';

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';

import { ScrollArea } from '@/components/ui/scroll-area';
import { formatDate, formatTime } from '@/utils/date';

interface TaskExecutorDetailProps {
  content?: AdminService.TaskExecutorInfo;
}

function CustomTooltip({ active, payload, ...restProps }: any) {
  if (active && payload && payload.length) {
    const item = payload.at(0)?.payload ?? {};

    return (
      <Card className="border bg-bg-base overflow-hidden shadow-xl">
        <CardHeader className="px-3 py-2 text-text-primary bg-bg-title">
          <CardTitle className="text-lg">
            {formatDate(restProps.label)}
          </CardTitle>
        </CardHeader>

        <CardContent className="p-0">
          <ScrollArea className="px-3">
            <div className="max-h-56">
              <JsonView
                src={item}
                displaySize={30}
                className="py-2 break-words text-text-secondary"
              />
            </div>
          </ScrollArea>
        </CardContent>
      </Card>
    );
  }

  return null;
}

function CustomAxisTick({ x, y, payload }: any) {
  return (
    <g transform={`translate(${x},${y})`}>
      <text
        x={0}
        y={0}
        dy={8}
        transform="rotate(60)"
        textAnchor="start"
        fill="rgb(var(--text-secondary))"
      >
        {formatTime(payload.value)}
      </text>
    </g>
  );
}

function TaskExecutorDetail({ content }: TaskExecutorDetailProps) {
  return (
    <section className="space-y-8">
      {Object.entries(content ?? {}).map(([name, data]) => {
        const items = data.map((x) => ({
          ...x,
          done: Math.floor(Math.random() * 100),
          failed: Math.floor(Math.random() * 100),
          now: dayjs(x.now).valueOf(),
        }));

        const lastItem = items.at(-1);

        return (
          <Card key={name} className="border-0">
            <CardHeader className="text-text-primary">
              <CardTitle>{name}</CardTitle>

              <div className="text-sm text-text-secondary space-x-4">
                <span>Lag: {lastItem?.lag}</span>
                <span>Pending: {lastItem?.pending}</span>
              </div>
            </CardHeader>

            <CardContent>
              <ResponsiveContainer className="min-h-40 w-full">
                <BarChart
                  data={items}
                  margin={{ bottom: 32 }}
                  className="text-sm text-text-secondary"
                >
                  <XAxis
                    dataKey="now"
                    type="number"
                    scale="time"
                    domain={['dataMin', 'dataMax']}
                    tick={CustomAxisTick}
                    interval="equidistantPreserveStart"
                    angle={60}
                    minTickGap={16}
                    allowDataOverflow
                    padding={{ left: 24, right: 24 }}
                  />

                  {/* <YAxis
                      type="number"
                      tick={{ fill: 'rgb(var(--text-secondary))' }}
                    /> */}

                  <CartesianGrid strokeDasharray="3 3" />

                  <Tooltip
                    trigger="click"
                    content={CustomTooltip}
                    wrapperStyle={{
                      pointerEvents: 'auto',
                    }}
                  />

                  <Legend wrapperStyle={{ bottom: 0 }} />

                  <Bar
                    dataKey="done"
                    fill="rgb(var(--state-success))"
                    activeBar={
                      <Rectangle
                        fill="rgb(var(--state-success))"
                        stroke="var(--bg-base)"
                      />
                    }
                  />

                  <Bar
                    dataKey="failed"
                    fill="rgb(var(--state-error))"
                    activeBar={
                      <Rectangle
                        fill="rgb(var(--state-error))"
                        stroke="var(--bg-base)"
                      />
                    }
                  />
                </BarChart>
              </ResponsiveContainer>
            </CardContent>
          </Card>
        );
      })}
    </section>
  );
}

export default TaskExecutorDetail;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/admin/task-executor-detail.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 163 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `CustomTooltip()`: Function definition
- `CustomAxisTick()`: Function definition
- `TaskExecutorDetail()`: Function definition
- `items()`: Function definition

### Imports (6)

- `import dayjs from 'dayjs';`
- `import JsonView from 'react18-json-view';`
- `import {`
- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import { ScrollArea } from '@/components/ui/scroll-area';`
- `import { formatDate, formatTime } from '@/utils/date';`

## Code Structure Analysis

- Total lines: 163
- Blank lines: 21 (12.9%)
- Comment lines: ~0 (0.0%)
- Code lines: ~142


## Dependencies and Imports

- `dayjs`
- `react18-json-view`
- `@/components/ui/card`
- `@/components/ui/scroll-area`
- `@/utils/date`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/admin`.

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

- Other files in `web/src/pages/admin/` directory
- Potential test file: `test_task-executor-detail.tsx`

## Keywords

@/components/ui/card, @/components/ui/scroll-area, @/utils/date, AdminService, Bar, BarChart, Card, CardContent, CardHeader, CardTitle, CartesianGrid, CustomAxisTick, CustomTooltip, JsonView, Lag, Legend, Math, Object, Pending, Rectangle, ResponsiveContainer, ScrollArea, TaskExecutorDetail, TaskExecutorDetailProps, TaskExecutorInfo, Tooltip, TypeScript, XAxis, YAxis, dayjs, item, items, lastItem, react18-json-view

---
*Generated by RAGFlow Repository Documentation Generator*
