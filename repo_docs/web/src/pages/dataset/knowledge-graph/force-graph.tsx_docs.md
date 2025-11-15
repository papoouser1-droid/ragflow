# File Documentation: web/src/pages/dataset/knowledge-graph/force-graph.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/knowledge-graph/force-graph.tsx`
- **Extension**: `.tsx`
- **Lines**: 142
- **Characters**: 3,642
- **Size**: 3,648 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/knowledge-graph/force-graph.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 142 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `ForceGraph()`: Function definition
- `nextData()`: Function definition
- `render()`: Function definition
- `graph()`: Function definition

### Imports (5)

- `import { ElementDatum, Graph, IElementEvent } from '@antv/g6';`
- `import isEmpty from 'lodash/isEmpty';`
- `import { useCallback, useEffect, useMemo, useRef } from 'react';`
- `import { buildNodesAndCombos } from './util';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 142
- Blank lines: 14 (9.9%)
- Comment lines: ~2 (1.4%)
- Code lines: ~126


## Dependencies and Imports

- `@antv/g6`
- `lodash/isEmpty`
- `react`
- `./util`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/knowledge-graph`.

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

- Other files in `web/src/pages/dataset/knowledge-graph/` directory
- Potential test file: `test_force-graph.tsx`

## Keywords

./index.less, ./util, @antv/g6, Activate, Array, ElementDatum, Entity, ForceGraph, Graph, HTMLDivElement, IElementEvent, IProps, Number, TooltipColorMap, TypeScript, Weight, antv, containerRef, graph, graphData, graphRef, lineWeight, lodash/isEmpty, mi, nextData, react, render, result, weight

---
*Generated by RAGFlow Repository Documentation Generator*
