# File Documentation: web/src/components/layout-recognize-form-field.tsx

## File Metadata

- **Path**: `web/src/components/layout-recognize-form-field.tsx`
- **Extension**: `.tsx`
- **Lines**: 115
- **Characters**: 3,227
- **Size**: 3,227 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { LlmModelType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';
import { cn } from '@/lib/utils';
import { camelCase } from 'lodash';
import { ReactNode, useMemo } from 'react';
import { useFormContext } from 'react-hook-form';
import { SelectWithSearch } from './originui/select-with-search';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from './ui/form';

export const enum ParseDocumentType {
  DeepDOC = 'DeepDOC',
  PlainText = 'Plain Text',
  MinerU = 'MinerU',
  Docling = 'Docling',
  TCADPParser = 'TCADP Parser',
}

export function LayoutRecognizeFormField({
  name = 'parser_config.layout_recognize',
  horizontal = true,
  optionsWithoutLLM,
  label,
}: {
  name?: string;
  horizontal?: boolean;
  optionsWithoutLLM?: { value: string; label: string }[];
  label?: ReactNode;
}) {
  const form = useFormContext();

  const { t } = useTranslate('knowledgeDetails');
  const allOptions = useSelectLlmOptionsByModelType();

  const options = useMemo(() => {
    const list = optionsWithoutLLM
      ? optionsWithoutLLM
      : [
          ParseDocumentType.DeepDOC,
          ParseDocumentType.PlainText,
          ParseDocumentType.MinerU,
          ParseDocumentType.Docling,
          ParseDocumentType.TCADPParser,
        ].map((x) => ({
          label: x === ParseDocumentType.PlainText ? t(camelCase(x)) : x,
          value: x,
        }));

    const image2TextList = allOptions[LlmModelType.Image2text].map((x) => {
      return {
        ...x,
        options: x.options.map((y) => {
          return {
            ...y,
            label: (
              <div className="flex justify-between items-center gap-2">
                {y.label}
                <span className="text-red-500 text-sm">Experimental</span>
              </div>
            ),
          };
        }),
      };
    });

    return [...list, ...image2TextList];
  }, [allOptions, optionsWithoutLLM, t]);

  return (
    <FormField
      control={form.control}
      name={name}
      render={({ field }) => {
        return (
          <FormItem className={'items-center space-y-0 '}>
            <div
              className={cn('flex', {
                'flex-col ': !horizontal,
                'items-center': horizontal,
              })}
            >
              <FormLabel
                tooltip={t('layoutRecognizeTip')}
                className={cn('text-sm text-text-secondary whitespace-wrap', {
                  ['w-1/4']: horizontal,
                })}
              >
                {label || t('layoutRecognize')}
              </FormLabel>
              <div className={horizontal ? 'w-3/4' : 'w-full'}>
                <FormControl>
                  <SelectWithSearch
                    {...field}
                    options={options}
                  ></SelectWithSearch>
                </FormControl>
              </div>
            </div>
            <div className="flex pt-1">
              <div className={horizontal ? 'w-1/4' : 'w-full'}></div>
              <FormMessage />
            </div>
          </FormItem>
        );
      }}
    />
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/layout-recognize-form-field.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 115 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `enum`: Exported entity
- `LayoutRecognizeFormField`: Exported entity

### Functions (4)

- `LayoutRecognizeFormField()`: Function definition
- `options()`: Function definition
- `list()`: Function definition
- `image2TextList()`: Function definition

### Imports (9)

- `import { LlmModelType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';`
- `import { cn } from '@/lib/utils';`
- `import { camelCase } from 'lodash';`
- `import { ReactNode, useMemo } from 'react';`
- `import { useFormContext } from 'react-hook-form';`
- `import { SelectWithSearch } from './originui/select-with-search';`
- `import {`

## Code Structure Analysis

- Total lines: 115
- Blank lines: 8 (7.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~107


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/hooks/llm-hooks`
- `@/lib/utils`
- `lodash`
- `react`
- `react-hook-form`
- `./originui/select-with-search`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components`.

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

- Other files in `web/src/components/` directory
- Potential test file: `test_layout-recognize-form-field.tsx`

## Keywords

./originui/select-with-search, @/constants/knowledge, @/hooks/common-hooks, @/hooks/llm-hooks, @/lib/utils, DeepDOC, Docling, Experimental, FormControl, FormField, FormItem, FormLabel, FormMessage, Image2text, LayoutRecognizeFormField, LlmModelType, MinerU, ParseDocumentType, Parser, Plain, PlainText, ReactNode, SelectWithSearch, TCADP, TCADPParser, Text, TypeScript, allOptions, enum, form, image2TextList, list, lodash, options, react, react-hook-form

---
*Generated by RAGFlow Repository Documentation Generator*
