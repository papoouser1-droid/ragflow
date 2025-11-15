# File Documentation: web/src/pages/agent/form/parser-form/image-form-fields.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/parser-form/image-form-fields.tsx`
- **Extension**: `.tsx`
- **Lines**: 61
- **Characters**: 1,960
- **Size**: 1,960 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { RAGFlowFormItem } from '@/components/ragflow-form';
import { Textarea } from '@/components/ui/textarea';
import { buildOptions } from '@/utils/form';
import { isEmpty } from 'lodash';
import { useEffect, useMemo } from 'react';
import { useFormContext, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { ImageParseMethod } from '../../constant/pipeline';
import { LanguageFormField, ParserMethodFormField } from './common-form-fields';
import { CommonProps } from './interface';
import { useSetInitialLanguage } from './use-set-initial-language';
import { buildFieldNameWithPrefix } from './utils';

export function ImageFormFields({ prefix }: CommonProps) {
  const { t } = useTranslation();
  const form = useFormContext();
  const options = buildOptions(
    ImageParseMethod,
    t,
    'flow.imageParseMethodOptions',
  );
  const parseMethodName = buildFieldNameWithPrefix('parse_method', prefix);

  const parseMethod = useWatch({
    name: parseMethodName,
  });

  const languageShown = useMemo(() => {
    return !isEmpty(parseMethod) && parseMethod !== ImageParseMethod.OCR;
  }, [parseMethod]);

  useEffect(() => {
    if (isEmpty(form.getValues(parseMethodName))) {
      form.setValue(parseMethodName, ImageParseMethod.OCR, {
        shouldValidate: true,
        shouldDirty: true,
      });
    }
  }, [form, parseMethodName]);

  useSetInitialLanguage({ prefix, languageShown });

  return (
    <>
      <ParserMethodFormField
        prefix={prefix}
        optionsWithoutLLM={options}
      ></ParserMethodFormField>
      {languageShown && <LanguageFormField prefix={prefix}></LanguageFormField>}
      {languageShown && (
        <RAGFlowFormItem
          name={buildFieldNameWithPrefix('system_prompt', prefix)}
          label={t('flow.systemPrompt')}
        >
          <Textarea placeholder={t('flow.systemPromptPlaceholder')} />
        </RAGFlowFormItem>
      )}
    </>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/parser-form/image-form-fields.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 61 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ImageFormFields`: Exported entity

### Functions (2)

- `ImageFormFields()`: Function definition
- `languageShown()`: Function definition

### Imports (12)

- `import { RAGFlowFormItem } from '@/components/ragflow-form';`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { buildOptions } from '@/utils/form';`
- `import { isEmpty } from 'lodash';`
- `import { useEffect, useMemo } from 'react';`
- `import { useFormContext, useWatch } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`
- `import { ImageParseMethod } from '../../constant/pipeline';`
- `import { LanguageFormField, ParserMethodFormField } from './common-form-fields';`
- `import { CommonProps } from './interface';`

## Code Structure Analysis

- Total lines: 61
- Blank lines: 7 (11.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~54


## Dependencies and Imports

- `@/components/ragflow-form`
- `@/components/ui/textarea`
- `@/utils/form`
- `lodash`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../constant/pipeline`
- `./common-form-fields`
- `./interface`
- `./use-set-initial-language`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/parser-form`.

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

- Other files in `web/src/pages/agent/form/parser-form/` directory
- Potential test file: `test_image-form-fields.tsx`

## Keywords

../../constant/pipeline, ./common-form-fields, ./interface, ./use-set-initial-language, ./utils, @/components/ragflow-form, @/components/ui/textarea, @/utils/form, CommonProps, ImageFormFields, ImageParseMethod, LanguageFormField, OCR, ParserMethodFormField, RAGFlowFormItem, Textarea, TypeScript, form, languageShown, lodash, options, parseMethod, parseMethodName, react, react-hook-form, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
