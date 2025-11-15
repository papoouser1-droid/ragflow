# File Documentation: web/src/pages/agent/form/parser-form/common-form-fields.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/parser-form/common-form-fields.tsx`
- **Extension**: `.tsx`
- **Lines**: 107
- **Characters**: 2,816
- **Size**: 2,816 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { crossLanguageOptions } from '@/components/cross-language-form-field';
import { LayoutRecognizeFormField } from '@/components/layout-recognize-form-field';
import {
  LLMFormField,
  LLMFormFieldProps,
} from '@/components/llm-setting-items/llm-form-field';
import {
  SelectWithSearch,
  SelectWithSearchFlagOptionType,
} from '@/components/originui/select-with-search';
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { upperCase, upperFirst } from 'lodash';
import { useTranslation } from 'react-i18next';
import {
  FileType,
  OutputFormatMap,
  SpreadsheetOutputFormat,
} from '../../constant/pipeline';
import { CommonProps } from './interface';
import { buildFieldNameWithPrefix } from './utils';

const UppercaseFields = [
  SpreadsheetOutputFormat.Html,
  SpreadsheetOutputFormat.Json,
];

function buildOutputOptionsFormatMap() {
  return Object.entries(OutputFormatMap).reduce<
    Record<string, SelectWithSearchFlagOptionType[]>
  >((pre, [key, value]) => {
    pre[key] = Object.values(value).map((v) => ({
      label: UppercaseFields.some((x) => x === v)
        ? upperCase(v)
        : upperFirst(v),
      value: v,
    }));
    return pre;
  }, {});
}

export type OutputFormatFormFieldProps = CommonProps & {
  fileType: FileType;
};

export function OutputFormatFormField({
  prefix,
  fileType,
}: OutputFormatFormFieldProps) {
  const { t } = useTranslation();
  return (
    <RAGFlowFormItem
      name={buildFieldNameWithPrefix(`output_format`, prefix)}
      label={t('flow.outputFormat')}
    >
      <SelectWithSearch
        options={buildOutputOptionsFormatMap()[fileType]}
      ></SelectWithSearch>
    </RAGFlowFormItem>
  );
}

export function ParserMethodFormField({
  prefix,
  optionsWithoutLLM,
}: CommonProps & { optionsWithoutLLM?: { value: string; label: string }[] }) {
  const { t } = useTranslation();
  return (
    <LayoutRecognizeFormField
      name={buildFieldNameWithPrefix(`parse_method`, prefix)}
      horizontal={false}
      optionsWithoutLLM={optionsWithoutLLM}
      label={t('flow.parserMethod')}
    ></LayoutRecognizeFormField>
  );
}

export function LargeModelFormField({
  prefix,
  options,
}: CommonProps & Pick<LLMFormFieldProps, 'options'>) {
  return (
    <LLMFormField
      name={buildFieldNameWithPrefix('llm_id', prefix)}
      options={options}
    ></LLMFormField>
  );
}

export function LanguageFormField({ prefix }: CommonProps) {
  const { t } = useTranslation();

  return (
    <RAGFlowFormItem
      name={buildFieldNameWithPrefix(`lang`, prefix)}
      label={t('flow.lang')}
    >
      {(field) => (
        <SelectWithSearch
          options={crossLanguageOptions}
          value={field.value}
          onChange={field.onChange}
        ></SelectWithSearch>
      )}
    </RAGFlowFormItem>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/parser-form/common-form-fields.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 107 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `OutputFormatFormField`: Exported entity
- `ParserMethodFormField`: Exported entity
- `LargeModelFormField`: Exported entity
- `LanguageFormField`: Exported entity

### Functions (5)

- `UppercaseFields()`: Function definition
- `OutputFormatFormField()`: Function definition
- `ParserMethodFormField()`: Function definition
- `LargeModelFormField()`: Function definition
- `LanguageFormField()`: Function definition

### Imports (10)

- `import { crossLanguageOptions } from '@/components/cross-language-form-field';`
- `import { LayoutRecognizeFormField } from '@/components/layout-recognize-form-field';`
- `import {`
- `import {`
- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { upperCase, upperFirst } from 'lodash';`
- `import { useTranslation } from 'react-i18next';`
- `import {`
- `import { CommonProps } from './interface';`
- `import { buildFieldNameWithPrefix } from './utils';`

## Code Structure Analysis

- Total lines: 107
- Blank lines: 9 (8.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~98


## Dependencies and Imports

- `@/components/cross-language-form-field`
- `@/components/layout-recognize-form-field`
- `@/components/ragflow-form`
- `lodash`
- `react-i18next`
- `./interface`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/parser-form`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/parser-form/` directory
- Potential test file: `test_common-form-fields.tsx`

## Keywords

./interface, ./utils, @/components/cross-language-form-field, @/components/layout-recognize-form-field, @/components/ragflow-form, CommonProps, FileType, Html, Json, LLMFormField, LLMFormFieldProps, LanguageFormField, LargeModelFormField, LayoutRecognizeFormField, Object, OutputFormatFormField, OutputFormatFormFieldProps, OutputFormatMap, ParserMethodFormField, Pick, RAGFlowFormItem, Record, SelectWithSearch, SelectWithSearchFlagOptionType, SpreadsheetOutputFormat, TypeScript, UppercaseFields, buildOutputOptionsFormatMap, lodash, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
