# Documentation: web/src/pages/agent/form/components/structured-output-secondary-menu.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/structured-output-secondary-menu.tsx`
- **Size**: 4518 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/components/structured-output-secondary-menu.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/components/structured-output-secondary-menu.tsx` is located in the `web/src/pages/agent/form/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [api-key-field.tsx](api-key-field.tsx_docs.md)
- [description-field.tsx](description-field.tsx_docs.md)
- [dynamic-fom-header.tsx](dynamic-fom-header.tsx_docs.md)
- [dynamic-input-variable.tsx](dynamic-input-variable.tsx_docs.md)
- [form-wrapper.tsx](form-wrapper.tsx_docs.md)
- [index.less](index.less_docs.md)
- [next-dynamic-input-variable.tsx](next-dynamic-input-variable.tsx_docs.md)
- [output.tsx](output.tsx_docs.md)
- [query-variable-list.tsx](query-variable-list.tsx_docs.md)
- [query-variable.tsx](query-variable.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
