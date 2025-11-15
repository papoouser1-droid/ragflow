# Documentation: web/src/pages/admin/task-executor-detail.tsx

## File Metadata

- **Path**: `web/src/pages/admin/task-executor-detail.tsx`
- **Size**: 4586 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/admin/task-executor-detail.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/admin/task-executor-detail.tsx` is located in the `web/src/pages/admin` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to admin.

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

- [login.tsx](login.tsx_docs.md)
- [monitoring.tsx](monitoring.tsx_docs.md)
- [roles.tsx](roles.tsx_docs.md)
- [service-detail.tsx](service-detail.tsx_docs.md)
- [service-status.tsx](service-status.tsx_docs.md)
- [user-detail.tsx](user-detail.tsx_docs.md)
- [users.tsx](users.tsx_docs.md)
- [utils.tsx](utils.tsx_docs.md)
- [whitelist.tsx](whitelist.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
