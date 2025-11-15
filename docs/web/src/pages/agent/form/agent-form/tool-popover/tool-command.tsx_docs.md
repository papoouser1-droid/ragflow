# Documentation: web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx`
- **Size**: 4213 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx`.

## Original Source Code

```tsx
import { Checkbox } from '@/components/ui/checkbox';
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
} from '@/components/ui/command';
import { useListMcpServer } from '@/hooks/use-mcp-request';
import { Operator } from '@/pages/agent/constant';
import OperatorIcon from '@/pages/agent/operator-icon';
import { t } from 'i18next';
import { lowerFirst } from 'lodash';
import { PropsWithChildren, useCallback, useEffect, useState } from 'react';
import { useTranslation } from 'react-i18next';

const Menus = [
  {
    label: t('flow.search'),
    list: [
      Operator.TavilySearch,
      Operator.TavilyExtract,
      Operator.Google,
      // Operator.Bing,
      Operator.DuckDuckGo,
      Operator.Wikipedia,
      Operator.SearXNG,
      Operator.YahooFinance,
      Operator.PubMed,
      Operator.GoogleScholar,
      Operator.ArXiv,
      Operator.WenCai,
    ],
  },
  {
    label: t('flow.communication'),
    list: [Operator.Email],
  },
  // {
  //   label: 'Productivity',
  //   list: [],
  // },
  {
    label: t('flow.developer'),
    list: [Operator.GitHub, Operator.ExeSQL, Operator.Code, Operator.Retrieval],
  },
];

type ToolCommandProps = {
  value?: string[];
  onChange?(values: string[]): void;
};

type ToolCommandItemProps = {
  toggleOption(id: string): void;
  id: string;
  isSelected: boolean;
} & ToolCommandProps;

function ToolCommandItem({
  toggleOption,
  id,
  isSelected,
  children,
}: ToolCommandItemProps & PropsWithChildren) {
  return (
    <CommandItem className="cursor-pointer" onSelect={() => toggleOption(id)}>
      <Checkbox checked={isSelected} />
      {children}
    </CommandItem>
  );
}

function useHandleSelectChange({ onChange, value }: ToolCommandProps) {
  const [currentValue, setCurrentValue] = useState<string[]>([]);

  const toggleOption = useCallback(
    (option: string) => {
      const newSelectedValues = currentValue.includes(option)
        ? currentValue.filter((value) => value !== option)
        : [...currentValue, option];
      setCurrentValue(newSelectedValues);
      onChange?.(newSelectedValues);
    },
    [currentValue, onChange],
  );

  useEffect(() => {
    if (Array.isArray(value)) {
      setCurrentValue(value);
    }
  }, [value]);

  return {
    toggleOption,
    currentValue,
  };
}

export function ToolCommand({ value, onChange }: ToolCommandProps) {
  const { t } = useTranslation();
  const { toggleOption, currentValue } = useHandleSelectChange({
    onChange,
    value,
  });

  return (
    <Command>
      <CommandInput placeholder={t('flow.typeCommandOrsearch')} />
      <CommandList>
        <CommandEmpty>No results found.</CommandEmpty>
        {Menus.map((x) => (
          <CommandGroup heading={x.label} key={x.label}>
            {x.list.map((y) => {
              const isSelected = currentValue.includes(y);
              return (
                <ToolCommandItem
                  key={y}
                  id={y}
                  toggleOption={toggleOption}
                  isSelected={isSelected}
                >
                  <>
                    <OperatorIcon name={y as Operator}></OperatorIcon>
                    <span>{t(`flow.${lowerFirst(y)}`)}</span>
                  </>
                </ToolCommandItem>
              );
            })}
          </CommandGroup>
        ))}
      </CommandList>
    </Command>
  );
}

export function MCPCommand({ onChange, value }: ToolCommandProps) {
  const { data } = useListMcpServer();
  const { toggleOption, currentValue } = useHandleSelectChange({
    onChange,
    value,
  });

  return (
    <Command>
      <CommandInput placeholder="Type a command or search..." />
      <CommandList>
        <CommandEmpty>No results found.</CommandEmpty>
        {data.mcp_servers.map((item) => {
          const isSelected = currentValue.includes(item.id);

          return (
            <ToolCommandItem
              key={item.id}
              id={item.id}
              isSelected={isSelected}
              toggleOption={toggleOption}
            >
              {item.name}
            </ToolCommandItem>
          );
        })}
      </CommandList>
    </Command>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx` is located in the `web/src/pages/agent/form/agent-form/tool-popover` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to tool-popover.

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

- [index.tsx](index.tsx_docs.md)
- [use-update-mcp.ts](use-update-mcp.ts_docs.md)
- [use-update-tools.ts](use-update-tools.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
