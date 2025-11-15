# Documentation: web/src/pages/dataset/knowledge-graph/force-graph.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/knowledge-graph/force-graph.tsx`
- **Size**: 3648 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/knowledge-graph/force-graph.tsx`.

## Original Source Code

```tsx
import { ElementDatum, Graph, IElementEvent } from '@antv/g6';
import isEmpty from 'lodash/isEmpty';
import { useCallback, useEffect, useMemo, useRef } from 'react';
import { buildNodesAndCombos } from './util';

import styles from './index.less';

const TooltipColorMap = {
  combo: 'red',
  node: 'black',
  edge: 'blue',
};

interface IProps {
  data: any;
  show: boolean;
}

const ForceGraph = ({ data, show }: IProps) => {
  const containerRef = useRef<HTMLDivElement>(null);
  const graphRef = useRef<Graph | null>(null);

  const nextData = useMemo(() => {
    if (!isEmpty(data)) {
      const graphData = data;
      const mi = buildNodesAndCombos(graphData.nodes);
      return { edges: graphData.edges, ...mi };
    }
    return { nodes: [], edges: [] };
  }, [data]);

  const render = useCallback(() => {
    const graph = new Graph({
      container: containerRef.current!,
      autoFit: 'view',
      autoResize: true,
      behaviors: [
        'drag-element',
        'drag-canvas',
        'zoom-canvas',
        'collapse-expand',
        {
          type: 'hover-activate',
          degree: 1, // 👈🏻 Activate relations.
        },
      ],
      plugins: [
        {
          type: 'tooltip',
          enterable: true,
          getContent: (e: IElementEvent, items: ElementDatum) => {
            if (Array.isArray(items)) {
              if (items.some((x) => x?.isCombo)) {
                return `<p style="font-weight:600;color:red">${items?.[0]?.data?.label}</p>`;
              }
              let result = ``;
              items.forEach((item) => {
                result += `<section style="color:${TooltipColorMap[e['targetType'] as keyof typeof TooltipColorMap]};"><h3>${item?.id}</h3>`;
                if (item?.entity_type) {
                  result += `<div style="padding-bottom: 6px;"><b>Entity type: </b>${item?.entity_type}</div>`;
                }
                if (item?.weight) {
                  result += `<div><b>Weight: </b>${item?.weight}</div>`;
                }
                if (item?.description) {
                  result += `<p>${item?.description}</p>`;
                }
              });
              return result + '</section>';
            }
            return undefined;
          },
        },
      ],
      layout: {
        type: 'combo-combined',
        preventOverlap: true,
        comboPadding: 1,
        spacing: 100,
      },
      node: {
        style: {
          size: 150,
          labelText: (d) => d.id,
          // labelPadding: 30,
          labelFontSize: 40,
          //   labelOffsetX: 20,
          labelOffsetY: 20,
          labelPlacement: 'center',
          labelWordWrap: true,
        },
        palette: {
          type: 'group',
          field: (d) => {
            return d?.entity_type as string;
          },
        },
      },
      edge: {
        style: (model) => {
          const weight: number = Number(model?.weight) || 2;
          const lineWeight = weight * 4;
          return {
            stroke: '#99ADD1',
            lineWidth: lineWeight > 10 ? 10 : lineWeight,
          };
        },
      },
    });

    if (graphRef.current) {
      graphRef.current.destroy();
    }

    graphRef.current = graph;

    graph.setData(nextData);

    graph.render();
  }, [nextData]);

  useEffect(() => {
    if (!isEmpty(data)) {
      render();
    }
  }, [data, render]);

  return (
    <div
      ref={containerRef}
      className={styles.forceContainer}
      style={{
        width: '100%',
        height: '100%',
        display: show ? 'block' : 'none',
      }}
    />
  );
};

export default ForceGraph;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/knowledge-graph/force-graph.tsx` is located in the `web/src/pages/dataset/knowledge-graph` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge-graph.

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

- [constant.ts](constant.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [use-delete-graph.ts](use-delete-graph.ts_docs.md)
- [util.ts](util.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
