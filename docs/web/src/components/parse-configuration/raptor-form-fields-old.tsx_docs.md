# Documentation: web/src/components/parse-configuration/raptor-form-fields-old.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/raptor-form-fields-old.tsx`
- **Size**: 4408 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/parse-configuration/raptor-form-fields-old.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/components/parse-configuration/raptor-form-fields-old.tsx` is located in the `web/src/components/parse-configuration` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parse-configuration.

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

- [graph-rag-form-fields.tsx](graph-rag-form-fields.tsx_docs.md)
- [graph-rag-items.tsx](graph-rag-items.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [raptor-form-fields.tsx](raptor-form-fields.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
