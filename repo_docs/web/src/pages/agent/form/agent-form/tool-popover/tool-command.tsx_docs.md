# File Documentation: web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx`
- **Extension**: `.tsx`
- **Lines**: 168
- **Characters**: 4,213
- **Size**: 4,213 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

      // Operator.Bing,
  // {
  //   label: 'Productivity',
  //   list: [],
  // },

## Detailed Walkthrough

### Exports (2)

- `ToolCommand`: Exported entity
- `MCPCommand`: Exported entity

### Functions (6)

- `ToolCommandItem()`: Function definition
- `useHandleSelectChange()`: Function definition
- `toggleOption()`: Function definition
- `newSelectedValues()`: Function definition
- `ToolCommand()`: Function definition
- `MCPCommand()`: Function definition

### Imports (9)

- `import { Checkbox } from '@/components/ui/checkbox';`
- `import {`
- `import { useListMcpServer } from '@/hooks/use-mcp-request';`
- `import { Operator } from '@/pages/agent/constant';`
- `import OperatorIcon from '@/pages/agent/operator-icon';`
- `import { t } from 'i18next';`
- `import { lowerFirst } from 'lodash';`
- `import { PropsWithChildren, useCallback, useEffect, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 168
- Blank lines: 14 (8.3%)
- Comment lines: ~5 (3.0%)
- Code lines: ~149


## Dependencies and Imports

- `@/components/ui/checkbox`
- `@/hooks/use-mcp-request`
- `@/pages/agent/constant`
- `@/pages/agent/operator-icon`
- `i18next`
- `lodash`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/agent-form/tool-popover`.

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

- Other files in `web/src/pages/agent/form/agent-form/tool-popover/` directory
- Potential test file: `test_tool-command.tsx`

## Keywords

@/components/ui/checkbox, @/hooks/use-mcp-request, @/pages/agent/constant, @/pages/agent/operator-icon, ArXiv, Array, Bing, Checkbox, Code, Command, CommandEmpty, CommandGroup, CommandInput, CommandItem, CommandList, DuckDuckGo, Email, ExeSQL, GitHub, Google, GoogleScholar, MCPCommand, Menus, Operator, OperatorIcon, Productivity, PropsWithChildren, PubMed, Retrieval, SearXNG, TavilyExtract, TavilySearch, ToolCommand, ToolCommandItem, ToolCommandItemProps, ToolCommandProps, Type, TypeScript, WenCai, Wikipedia, YahooFinance, i18next, isSelected, lodash, newSelectedValues, react, react-i18next, toggleOption, useHandleSelectChange

---
*Generated by RAGFlow Repository Documentation Generator*
