# File Documentation: web/src/pages/agent/form/components/structured-output-secondary-menu.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/structured-output-secondary-menu.tsx`
- **Extension**: `.tsx`
- **Lines**: 140
- **Characters**: 4,518
- **Size**: 4,518 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from '@/components/ui/hover-card';
import { cn } from '@/lib/utils';
import { get, isEmpty, isPlainObject } from 'lodash';
import { ChevronRight } from 'lucide-react';
import { PropsWithChildren, ReactNode, useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { JsonSchemaDataType } from '../../constant';
import { useGetStructuredOutputByValue } from '../../hooks/use-build-structured-output';
import {
  hasJsonSchemaChild,
  hasSpecificTypeChild,
} from '../../utils/filter-agent-structured-output';

type DataItem = { label: ReactNode; value: string; parentLabel?: ReactNode };

type StructuredOutputSecondaryMenuProps = {
  data: DataItem;
  click(option: { label: ReactNode; value: string }): void;
  types?: JsonSchemaDataType[];
} & PropsWithChildren;

export function StructuredOutputSecondaryMenu({
  data,
  click,
  types = [],
}: StructuredOutputSecondaryMenuProps) {
  const { t } = useTranslation();
  const filterStructuredOutput = useGetStructuredOutputByValue();
  const structuredOutput = filterStructuredOutput(data.value);

  const handleSubMenuClick = useCallback(
    (option: { label: ReactNode; value: string }, dataType?: string) => () => {
      // The query variable of the iteration operator can only select array type data.
      if (
        (!isEmpty(types) && types?.some((x) => x === dataType)) ||
        isEmpty(types)
      ) {
        click(option);
      }
    },
    [click, types],
  );

  const handleMenuClick = useCallback(() => {
    if (isEmpty(types) || types?.some((x) => x === JsonSchemaDataType.Object)) {
      click(data);
    }
  }, [click, data, types]);

  const renderAgentStructuredOutput = useCallback(
    (values: any, option: { label: ReactNode; value: string }) => {
      const properties =
        get(values, 'properties') || get(values, 'items.properties');

      if (isPlainObject(values) && properties) {
        return (
          <ul className="border-l">
            {Object.entries(properties).map(([key, value]) => {
              const nextOption = {
                label: option.label + `.${key}`,
                value: option.value + `.${key}`,
              };

              const dataType = get(value, 'type');

              if (
                isEmpty(types) ||
                (!isEmpty(types) &&
                  (types?.some((x) => x === dataType) ||
                    hasSpecificTypeChild(value ?? {}, types)))
              ) {
                return (
                  <li key={key} className="pl-1">
                    <div
                      onClick={handleSubMenuClick(nextOption, dataType)}
                      className="hover:bg-bg-card p-1 text-text-primary rounded-sm flex justify-between"
                    >
                      {key}
                      <span className="text-text-secondary">{dataType}</span>
                    </div>
                    {[JsonSchemaDataType.Object, JsonSchemaDataType.Array].some(
                      (x) => x === dataType,
                    ) && renderAgentStructuredOutput(value, nextOption)}
                  </li>
                );
              }

              return null;
            })}
          </ul>
        );
      }

      return <div></div>;
    },
    [handleSubMenuClick, types],
  );

  if (
    !hasJsonSchemaChild(structuredOutput) ||
    (!isEmpty(types) && !hasSpecificTypeChild(structuredOutput, types))
  ) {
    return null;
  }

  return (
    <HoverCard key={data.value} openDelay={100} closeDelay={100}>
      <HoverCardTrigger asChild>
        <li
          onClick={handleMenuClick}
          className="hover:bg-bg-card py-1 px-2 text-text-primary rounded-sm text-sm flex justify-between items-center gap-2"
        >
          <div className="flex justify-between flex-1">
            {data.label} <span className="text-text-secondary">object</span>
          </div>
          <ChevronRight className="size-3.5 text-text-secondary" />
        </li>
      </HoverCardTrigger>
      <HoverCardContent
        side="left"
        align="start"
        className={cn(
          'min-w-[140px]  border border-border rounded-md shadow-lg p-0',
        )}
      >
        <section className="p-2">
          <div className="p-1">
            {t('flow.structuredOutput.structuredOutput')}
          </div>
          {renderAgentStructuredOutput(structuredOutput, data)}
        </section>
      </HoverCardContent>
    </HoverCard>
  );
}

```

## High-Level Overview

      // The query variable of the iteration operator can only select array type data.

## Detailed Walkthrough

### Exports (1)

- `StructuredOutputSecondaryMenu`: Exported entity

### Functions (5)

- `StructuredOutputSecondaryMenu()`: Function definition
- `handleSubMenuClick()`: Function definition
- `handleMenuClick()`: Function definition
- `renderAgentStructuredOutput()`: Function definition
- `dataType()`: Function definition

### Imports (9)

- `import {`
- `import { cn } from '@/lib/utils';`
- `import { get, isEmpty, isPlainObject } from 'lodash';`
- `import { ChevronRight } from 'lucide-react';`
- `import { PropsWithChildren, ReactNode, useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { JsonSchemaDataType } from '../../constant';`
- `import { useGetStructuredOutputByValue } from '../../hooks/use-build-structured-output';`
- `import {`

## Code Structure Analysis

- Total lines: 140
- Blank lines: 14 (10.0%)
- Comment lines: ~1 (0.7%)
- Code lines: ~125


## Dependencies and Imports

- `@/lib/utils`
- `lodash`
- `lucide-react`
- `react`
- `react-i18next`
- `../../constant`
- `../../hooks/use-build-structured-output`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/components/` directory
- Potential test file: `test_structured-output-secondary-menu.tsx`

## Keywords

../../constant, ../../hooks/use-build-structured-output, @/lib/utils, Array, ChevronRight, DataItem, HoverCard, HoverCardContent, HoverCardTrigger, JsonSchemaDataType, Object, PropsWithChildren, ReactNode, StructuredOutputSecondaryMenu, StructuredOutputSecondaryMenuProps, The, TypeScript, data, dataType, filterStructuredOutput, handleMenuClick, handleSubMenuClick, lodash, lucide-react, nextOption, properties, react, react-i18next, renderAgentStructuredOutput, structuredOutput

---
*Generated by RAGFlow Repository Documentation Generator*
