# File Documentation: web/src/components/parse-configuration/raptor-form-fields-old.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/raptor-form-fields-old.tsx`
- **Extension**: `.tsx`
- **Lines**: 147
- **Characters**: 4,408
- **Size**: 4,408 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { DocumentParserType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import random from 'lodash/random';
import { Plus } from 'lucide-react';
import { useCallback } from 'react';
import { useFormContext, useWatch } from 'react-hook-form';
import { SliderInputFormField } from '../slider-input-form-field';
import { Button } from '../ui/button';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '../ui/form';
import { Input } from '../ui/input';
import { Switch } from '../ui/switch';
import { Textarea } from '../ui/textarea';

export const excludedParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.One,
  DocumentParserType.Picture,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Qa,
  DocumentParserType.Tag,
];

export const showRaptorParseConfiguration = (
  parserId: DocumentParserType | undefined,
) => {
  return !excludedParseMethods.some((x) => x === parserId);
};

export const excludedTagParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const showTagItems = (parserId: DocumentParserType) => {
  return !excludedTagParseMethods.includes(parserId);
};

const UseRaptorField = 'parser_config.raptor.use_raptor';
const RandomSeedField = 'parser_config.raptor.random_seed';

// The three types "table", "resume" and "one" do not display this configuration.

const RaptorFormFields = () => {
  const form = useFormContext();
  const { t } = useTranslate('knowledgeConfiguration');
  const useRaptor = useWatch({ name: UseRaptorField });

  const handleGenerate = useCallback(() => {
    form.setValue(RandomSeedField, random(10000));
  }, [form]);

  return (
    <>
      <FormField
        control={form.control}
        name={UseRaptorField}
        render={({ field }) => (
          <FormItem defaultChecked={false}>
            <FormLabel tooltip={t('useRaptorTip')}>{t('useRaptor')}</FormLabel>
            <FormControl>
              <Switch
                checked={field.value}
                onCheckedChange={field.onChange}
              ></Switch>
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      {useRaptor && (
        <div className="space-y-3">
          <FormField
            control={form.control}
            name={'parser_config.raptor.prompt'}
            render={({ field }) => (
              <FormItem>
                <FormLabel tooltip={t('promptTip')}>{t('prompt')}</FormLabel>
                <FormControl>
                  <Textarea {...field} rows={8} />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
          <SliderInputFormField
            name={'parser_config.raptor.max_token'}
            label={t('maxToken')}
            tooltip={t('maxTokenTip')}
            defaultValue={256}
            max={2048}
            min={0}
          ></SliderInputFormField>
          <SliderInputFormField
            name={'parser_config.raptor.threshold'}
            label={t('threshold')}
            tooltip={t('thresholdTip')}
            defaultValue={0.1}
            step={0.01}
            max={1}
            min={0}
          ></SliderInputFormField>
          <SliderInputFormField
            name={'parser_config.raptor.max_cluster'}
            label={t('maxCluster')}
            tooltip={t('maxClusterTip')}
            defaultValue={64}
            max={1024}
            min={1}
          ></SliderInputFormField>
          <FormField
            control={form.control}
            name={'parser_config.raptor.random_seed'}
            render={({ field }) => (
              <FormItem>
                <FormLabel>{t('randomSeed')}</FormLabel>
                <FormControl defaultValue={0}>
                  <div className="flex gap-4">
                    <Input {...field} />
                    <Button
                      size={'sm'}
                      onClick={handleGenerate}
                      type={'button'}
                    >
                      <Plus />
                    </Button>
                  </div>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        </div>
      )}
    </>
  );
};

export default RaptorFormFields;

```

## High-Level Overview

// The three types "table", "resume" and "one" do not display this configuration.

## Detailed Walkthrough

### Exports (4)

- `excludedParseMethods`: Exported entity
- `showRaptorParseConfiguration`: Exported entity
- `excludedTagParseMethods`: Exported entity
- `showTagItems`: Exported entity

### Functions (4)

- `showRaptorParseConfiguration()`: Function definition
- `showTagItems()`: Function definition
- `RaptorFormFields()`: Function definition
- `handleGenerate()`: Function definition

### Imports (12)

- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import random from 'lodash/random';`
- `import { Plus } from 'lucide-react';`
- `import { useCallback } from 'react';`
- `import { useFormContext, useWatch } from 'react-hook-form';`
- `import { SliderInputFormField } from '../slider-input-form-field';`
- `import { Button } from '../ui/button';`
- `import {`
- `import { Input } from '../ui/input';`

## Code Structure Analysis

- Total lines: 147
- Blank lines: 11 (7.5%)
- Comment lines: ~1 (0.7%)
- Code lines: ~135


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `lodash/random`
- `lucide-react`
- `react`
- `react-hook-form`
- `../slider-input-form-field`
- `../ui/button`
- `../ui/input`
- `../ui/switch`
- `../ui/textarea`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/parse-configuration`.

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

- Other files in `web/src/components/parse-configuration/` directory
- Potential test file: `test_raptor-form-fields-old.tsx`

## Keywords

../slider-input-form-field, ../ui/button, ../ui/input, ../ui/switch, ../ui/textarea, @/constants/knowledge, @/hooks/common-hooks, Button, DocumentParserType, FormControl, FormField, FormItem, FormLabel, FormMessage, Input, KnowledgeGraph, One, Picture, Plus, RandomSeedField, RaptorFormFields, Resume, SliderInputFormField, Switch, Table, Tag, Textarea, The, TypeScript, UseRaptorField, excludedParseMethods, excludedTagParseMethods, form, handleGenerate, lodash/random, lucide-react, react, react-hook-form, showRaptorParseConfiguration, showTagItems, useRaptor

---
*Generated by RAGFlow Repository Documentation Generator*
