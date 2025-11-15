# File Documentation: web/src/pages/agent/form/code-form/use-watch-change.ts

## File Metadata

- **Path**: `web/src/pages/agent/form/code-form/use-watch-change.ts`
- **Extension**: `.ts`
- **Lines**: 96
- **Characters**: 2,610
- **Size**: 2,610 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import { CodeTemplateStrMap, ProgrammingLanguage } from '@/constants/agent';
import { ICodeForm } from '@/interfaces/database/agent';
import { isEmpty } from 'lodash';
import { useCallback, useEffect } from 'react';
import { UseFormReturn, useWatch } from 'react-hook-form';
import useGraphStore from '../../store';
import { FormSchemaType } from './schema';

function convertToObject(list: FormSchemaType['arguments'] = []) {
  return list.reduce<Record<string, string>>((pre, cur) => {
    pre[cur.name] = cur.type;

    return pre;
  }, {});
}

type ArrayOutputs = Extract<FormSchemaType['outputs'], Array<any>>;

type ObjectOutputs = Exclude<FormSchemaType['outputs'], Array<any>>;

function convertOutputsToObject({ lang, outputs }: FormSchemaType) {
  if (lang === ProgrammingLanguage.Python) {
    return (outputs as ArrayOutputs).reduce<ICodeForm['outputs']>(
      (pre, cur) => {
        pre[cur.name] = {
          value: '',
          type: cur.type,
        };

        return pre;
      },
      {},
    );
  }
  const outputsObject = outputs as ObjectOutputs;
  if (isEmpty(outputsObject)) {
    return {};
  }
  return {
    [outputsObject.name]: {
      value: '',
      type: outputsObject.type,
    },
  };
}

export function useWatchFormChange(
  id?: string,
  form?: UseFormReturn<FormSchemaType>,
) {
  let values = useWatch({ control: form?.control });
  const updateNodeForm = useGraphStore((state) => state.updateNodeForm);

  useEffect(() => {
    // Manually triggered form updates are synchronized to the canvas
    if (id) {
      values = form?.getValues() || {};
      let nextValues: any = {
        ...values,
        arguments: convertToObject(
          values?.arguments as FormSchemaType['arguments'],
        ),
        outputs: convertOutputsToObject(values as FormSchemaType),
      };

      updateNodeForm(id, nextValues);
    }
  }, [form?.formState.isDirty, id, updateNodeForm, values]);
}

export function useHandleLanguageChange(
  id?: string,
  form?: UseFormReturn<FormSchemaType>,
) {
  const updateNodeForm = useGraphStore((state) => state.updateNodeForm);

  const handleLanguageChange = useCallback(
    (lang: string) => {
      if (id) {
        const script = CodeTemplateStrMap[lang as ProgrammingLanguage];
        form?.setValue('script', script);
        form?.setValue(
          'outputs',
          (lang === ProgrammingLanguage.Python
            ? []
            : {}) as FormSchemaType['outputs'],
        );
        updateNodeForm(id, script, ['script']);
      }
    },
    [form, id, updateNodeForm],
  );

  return handleLanguageChange;
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/code-form/use-watch-change.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 96 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (2)

- `useWatchFormChange`: Exported entity
- `useHandleLanguageChange`: Exported entity

### Functions (7)

- `convertToObject()`: Function definition
- `convertOutputsToObject()`: Function definition
- `useWatchFormChange()`: Function definition
- `updateNodeForm()`: Function definition
- `useHandleLanguageChange()`: Function definition
- `updateNodeForm()`: Function definition
- `handleLanguageChange()`: Function definition

### Imports (7)

- `import { CodeTemplateStrMap, ProgrammingLanguage } from '@/constants/agent';`
- `import { ICodeForm } from '@/interfaces/database/agent';`
- `import { isEmpty } from 'lodash';`
- `import { useCallback, useEffect } from 'react';`
- `import { UseFormReturn, useWatch } from 'react-hook-form';`
- `import useGraphStore from '../../store';`
- `import { FormSchemaType } from './schema';`

## Code Structure Analysis

- Total lines: 96
- Blank lines: 13 (13.5%)
- Comment lines: ~1 (1.0%)
- Code lines: ~82


## Dependencies and Imports

- `@/constants/agent`
- `@/interfaces/database/agent`
- `lodash`
- `react`
- `react-hook-form`
- `../../store`
- `./schema`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/code-form`.

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

- Other files in `web/src/pages/agent/form/code-form/` directory
- Potential test file: `test_use-watch-change.ts`

## Keywords

../../store, ./schema, @/constants/agent, @/interfaces/database/agent, Array, ArrayOutputs, CodeTemplateStrMap, Exclude, Extract, FormSchemaType, ICodeForm, Manually, ObjectOutputs, ProgrammingLanguage, Python, Record, TypeScript, UseFormReturn, convertOutputsToObject, convertToObject, handleLanguageChange, lodash, nextValues, outputsObject, react, react-hook-form, script, updateNodeForm, useHandleLanguageChange, useWatchFormChange, values

---
*Generated by RAGFlow Repository Documentation Generator*
