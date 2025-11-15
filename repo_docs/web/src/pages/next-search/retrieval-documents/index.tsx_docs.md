# File Documentation: web/src/pages/next-search/retrieval-documents/index.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/retrieval-documents/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 235
- **Characters**: 8,062
- **Size**: 8,062 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
  CommandSeparator,
} from '@/components/ui/command';
import { MultiSelectOptionType } from '@/components/ui/multi-select';
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import { Separator } from '@/components/ui/separator';
import {
  useAllTestingResult,
  useSelectTestingResult,
} from '@/hooks/knowledge-hooks';
import { cn } from '@/lib/utils';
import { CheckIcon, ChevronDown, Files, XIcon } from 'lucide-react';
import { useMemo, useState } from 'react';

interface IProps {
  onTesting(documentIds: string[]): void;
  setSelectedDocumentIds(documentIds: string[]): void;
  selectedDocumentIds: string[];
}

const RetrievalDocuments = ({
  onTesting,
  selectedDocumentIds,
  setSelectedDocumentIds,
}: IProps) => {
  const { documents: documentsAll } = useAllTestingResult();
  const { documents } = useSelectTestingResult();
  const [isPopoverOpen, setIsPopoverOpen] = useState(false);
  const { documents: useDocuments } = {
    documents:
      documentsAll?.length > documents?.length ? documentsAll : documents,
  };
  const [selectedValues, setSelectedValues] =
    useState<string[]>(selectedDocumentIds);

  const multiOptions = useMemo(() => {
    return useDocuments?.map((item) => {
      return {
        label: item.doc_name,
        value: item.doc_id,
        disabled: item.doc_name === 'Disabled User',
        // suffix: (
        //   <div className="flex justify-between gap-3 ">
        //     <div>{item.count}</div>
        //     <div>
        //       <Eye />
        //     </div>
        //   </div>
        // ),
      };
    });
  }, [useDocuments]);

  const handleTogglePopover = () => {
    setIsPopoverOpen((prev) => !prev);
  };

  const onValueChange = (value: string[]) => {
    console.log(value);
    onTesting(value);
    setSelectedDocumentIds(value);
    // handleDatasetSelectChange(value, field.onChange);
  };
  const handleClear = () => {
    setSelectedValues([]);
    onValueChange([]);
  };

  const handleInputKeyDown = (event: React.KeyboardEvent<HTMLInputElement>) => {
    if (event.key === 'Enter') {
      setIsPopoverOpen(true);
    } else if (event.key === 'Backspace' && !event.currentTarget.value) {
      const newSelectedValues = [...selectedValues];
      newSelectedValues.pop();
      setSelectedValues(newSelectedValues);
      onValueChange(newSelectedValues);
    }
  };
  const toggleOption = (option: string) => {
    const newSelectedValues = selectedValues.includes(option)
      ? selectedValues.filter((value) => value !== option)
      : [...selectedValues, option];
    setSelectedValues(newSelectedValues);
    onValueChange(newSelectedValues);
  };
  return (
    <Popover open={isPopoverOpen} onOpenChange={setIsPopoverOpen}>
      <PopoverTrigger asChild>
        <Button
          onClick={handleTogglePopover}
          className={cn(
            'flex w-full p-1 rounded-md text-base text-text-primary border min-h-10 h-auto items-center justify-between bg-inherit hover:bg-inherit [&_svg]:pointer-events-auto',
          )}
        >
          <div className="flex justify-between items-center w-full">
            <div className="flex flex-wrap items-center gap-2">
              <Files />
              <span>
                {selectedDocumentIds?.length ?? 0}/{useDocuments?.length ?? 0}
              </span>
              Files
            </div>
            <div className="flex items-center justify-between">
              <XIcon
                className="h-4 mx-2 cursor-pointer text-muted-foreground"
                onClick={(event) => {
                  event.stopPropagation();
                  handleClear();
                }}
              />
              <Separator
                orientation="vertical"
                className="flex min-h-6 h-full"
              />
              <ChevronDown className="h-4 mx-2 cursor-pointer text-muted-foreground" />
            </div>
          </div>
        </Button>
      </PopoverTrigger>
      <PopoverContent
        className="w-auto p-0"
        align="start"
        onEscapeKeyDown={() => setIsPopoverOpen(false)}
      >
        <Command>
          <CommandInput
            placeholder="Search..."
            onKeyDown={handleInputKeyDown}
          />
          <CommandList>
            <CommandEmpty>No results found.</CommandEmpty>
            <CommandGroup>
              {!multiOptions.some((x) => 'options' in x) &&
                (multiOptions as unknown as MultiSelectOptionType[]).map(
                  (option) => {
                    const isSelected = selectedValues.includes(option.value);
                    return (
                      <CommandItem
                        key={option.value}
                        onSelect={() => {
                          if (option.disabled) return false;
                          toggleOption(option.value);
                        }}
                        className={cn('cursor-pointer', {
                          'cursor-not-allowed text-text-disabled':
                            option.disabled,
                        })}
                      >
                        <div
                          className={cn(
                            'mr-2 flex h-4 w-4 items-center justify-center rounded-sm border border-primary',
                            isSelected
                              ? 'bg-primary '
                              : 'opacity-50 [&_svg]:invisible',

                            { 'text-primary-foreground': !option.disabled },
                            { 'text-text-disabled': option.disabled },
                          )}
                        >
                          <CheckIcon className="h-4 w-4" />
                        </div>
                        {option.icon && (
                          <option.icon
                            className={cn('mr-2 h-4 w-4 ', {
                              'text-text-disabled': option.disabled,
                              'text-muted-foreground': !option.disabled,
                            })}
                          />
                        )}
                        <span
                          className={cn({
                            'text-text-disabled': option.disabled,
                          })}
                        >
                          {option.label}
                        </span>
                        {option.suffix && (
                          <span
                            className={cn({
                              'text-text-disabled': option.disabled,
                            })}
                          >
                            {option.suffix}
                          </span>
                        )}
                      </CommandItem>
                    );
                  },
                )}
            </CommandGroup>
            <CommandSeparator />
            <CommandGroup>
              <div className="flex items-center justify-between">
                {selectedValues.length > 0 && (
                  <>
                    <CommandItem
                      onSelect={handleClear}
                      className="flex-1 justify-center cursor-pointer"
                    >
                      Clear
                    </CommandItem>
                    <Separator
                      orientation="vertical"
                      className="flex min-h-6 h-full"
                    />
                  </>
                )}
                <CommandItem
                  onSelect={() => setIsPopoverOpen(false)}
                  className="flex-1 justify-center cursor-pointer max-w-full"
                >
                  Close
                </CommandItem>
              </div>
            </CommandGroup>
          </CommandList>
        </Command>
      </PopoverContent>
    </Popover>
  );
};

export default RetrievalDocuments;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-search/retrieval-documents/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 235 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (8)

- `RetrievalDocuments()`: Function definition
- `multiOptions()`: Function definition
- `handleTogglePopover()`: Function definition
- `onValueChange()`: Function definition
- `handleClear()`: Function definition
- `handleInputKeyDown()`: Function definition
- `toggleOption()`: Function definition
- `newSelectedValues()`: Function definition

### Imports (9)

- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { MultiSelectOptionType } from '@/components/ui/multi-select';`
- `import {`
- `import { Separator } from '@/components/ui/separator';`
- `import {`
- `import { cn } from '@/lib/utils';`
- `import { CheckIcon, ChevronDown, Files, XIcon } from 'lucide-react';`
- `import { useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 235
- Blank lines: 9 (3.8%)
- Comment lines: ~9 (3.8%)
- Code lines: ~217


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/multi-select`
- `@/components/ui/separator`
- `@/lib/utils`
- `lucide-react`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-search/retrieval-documents`.

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

- Other files in `web/src/pages/next-search/retrieval-documents/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/components/ui/button, @/components/ui/multi-select, @/components/ui/separator, @/lib/utils, Backspace, Button, CheckIcon, ChevronDown, Clear, Close, Command, CommandEmpty, CommandGroup, CommandInput, CommandItem, CommandList, CommandSeparator, Disabled, Enter, Eye, Files, HTMLInputElement, IProps, KeyboardEvent, MultiSelectOptionType, Popover, PopoverContent, PopoverTrigger, React, RetrievalDocuments, Search, Separator, TypeScript, User, XIcon, handleClear, handleInputKeyDown, handleTogglePopover, isSelected, lucide-react, multiOptions, newSelectedValues, onValueChange, react, toggleOption

---
*Generated by RAGFlow Repository Documentation Generator*
