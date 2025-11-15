# File Documentation: web/src/pages/dataset/dataset-setting/configuration/common-item.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/configuration/common-item.tsx`
- **Extension**: `.tsx`
- **Lines**: 269
- **Characters**: 8,357
- **Size**: 8,357 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Radio } from '@/components/ui/radio';
import { Spin } from '@/components/ui/spin';
import { Switch } from '@/components/ui/switch';
import { useTranslate } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import { useMemo, useState } from 'react';
import { useFormContext } from 'react-hook-form';
import {
  useHandleKbEmbedding,
  useHasParsedDocument,
  useSelectChunkMethodList,
  useSelectEmbeddingModelOptions,
} from '../hooks';
interface IProps {
  line?: 1 | 2;
  isEdit?: boolean;
}
export function ChunkMethodItem(props: IProps) {
  const { line } = props;
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();
  // const handleChunkMethodSelectChange = useHandleChunkMethodSelectChange(form);
  const parserList = useSelectChunkMethodList();

  return (
    <FormField
      control={form.control}
      name={'parser_id'}
      render={({ field }) => (
        <FormItem className=" items-center space-y-1">
          <div className={line === 1 ? 'flex items-center' : ''}>
            <FormLabel
              required
              tooltip={t('chunkMethodTip')}
              className={cn('text-sm', {
                'w-1/4 whitespace-pre-wrap': line === 1,
              })}
            >
              {t('builtIn')}
            </FormLabel>
            <div className={line === 1 ? 'w-3/4 ' : 'w-full'}>
              <FormControl>
                <SelectWithSearch
                  {...field}
                  options={parserList}
                  placeholder={t('chunkMethodPlaceholder')}
                />
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className={line === 1 ? 'w-1/4' : ''}></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );
}
export function EmbeddingModelItem({ line = 1, isEdit }: IProps) {
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();
  const embeddingModelOptions = useSelectEmbeddingModelOptions();
  const { handleChange } = useHandleKbEmbedding();
  const disabled = useHasParsedDocument(isEdit);
  const oldValue = useMemo(() => {
    const embdStr = form.getValues('embd_id');
    return embdStr || '';
  }, [form]);
  const [loading, setLoading] = useState(false);
  return (
    <>
      <FormField
        control={form.control}
        name={'embd_id'}
        render={({ field }) => (
          <FormItem className={cn(' items-center space-y-0 ')}>
            <div
              className={cn('flex', {
                ' items-center': line === 1,
                'flex-col gap-1': line === 2,
              })}
            >
              <FormLabel
                required
                tooltip={t('embeddingModelTip')}
                className={cn('text-sm  whitespace-wrap ', {
                  'w-1/4': line === 1,
                })}
              >
                {t('embeddingModel')}
              </FormLabel>
              <div
                className={cn('text-muted-foreground', { 'w-3/4': line === 1 })}
              >
                <FormControl>
                  <Spin
                    spinning={loading}
                    className={cn(' rounded-lg after:bg-bg-base', {
                      'opacity-20': loading,
                    })}
                  >
                    <SelectWithSearch
                      onChange={async (value) => {
                        field.onChange(value);
                        if (isEdit && disabled) {
                          setLoading(true);
                          const res = await handleChange({
                            embed_id: value,
                            callback: field.onChange,
                          });
                          if (res.code !== 0) {
                            field.onChange(oldValue);
                          }
                          setLoading(false);
                        }
                      }}
                      value={field.value}
                      options={embeddingModelOptions}
                      placeholder={t('embeddingModelPlaceholder')}
                      triggerClassName="!bg-bg-base"
                    />
                  </Spin>
                </FormControl>
              </div>
            </div>
            <div className="flex pt-1">
              <div className={line === 1 ? 'w-1/4' : ''}></div>
              <FormMessage />
            </div>
          </FormItem>
        )}
      />
    </>
  );
}

export function ParseTypeItem({ line = 2 }: { line?: number }) {
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={'parseType'}
      render={({ field }) => (
        <FormItem className=" items-center space-y-0 ">
          <div
            className={cn('flex', {
              ' items-center': line === 1,
              'flex-col gap-1': line === 2,
            })}
          >
            <FormLabel
              // tooltip={t('parseTypeTip')}
              className={cn('text-sm  whitespace-wrap ', {
                'w-1/4': line === 1,
              })}
            >
              {t('parseType')}
            </FormLabel>
            <div
              className={cn('text-muted-foreground', { 'w-3/4': line === 1 })}
            >
              <FormControl>
                <Radio.Group {...field}>
                  <div
                    className={cn(
                      'flex gap-2 justify-between text-muted-foreground',
                      line === 1 ? 'w-1/2' : 'w-3/4',
                    )}
                  >
                    <Radio value={1}>{t('builtIn')}</Radio>
                    <Radio value={2}>{t('manualSetup')}</Radio>
                  </div>
                </Radio.Group>
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className={line === 1 ? 'w-1/4' : ''}></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );
}

export function EnableAutoGenerateItem() {
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={'enableAutoGenerate'}
      render={({ field }) => (
        <FormItem className=" items-center space-y-0 ">
          <div className="flex items-center">
            <FormLabel
              tooltip={t('enableAutoGenerateTip')}
              className="text-sm  whitespace-wrap w-1/4"
            >
              {t('enableAutoGenerate')}
            </FormLabel>
            <div className="text-muted-foreground w-3/4">
              <FormControl>
                <Switch
                  checked={field.value}
                  onCheckedChange={field.onChange}
                />
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className="w-1/4"></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );
}

export function EnableTocToggle() {
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();

  return (
    <FormField
      control={form.control}
      name={'parser_config.toc_extraction'}
      render={({ field }) => (
        <FormItem className=" items-center space-y-0 ">
          <div className="flex items-center">
            <FormLabel
              tooltip={t('tocExtractionTip')}
              className="text-sm  whitespace-wrap w-1/4"
            >
              {t('tocExtraction')}
            </FormLabel>
            <div className="text-muted-foreground w-3/4">
              <FormControl>
                <Switch
                  checked={field.value}
                  onCheckedChange={field.onChange}
                />
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className="w-1/4"></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );
}

```

## High-Level Overview

  // const handleChunkMethodSelectChange = useHandleChunkMethodSelectChange(form);

## Detailed Walkthrough

### Exports (5)

- `ChunkMethodItem`: Exported entity
- `EmbeddingModelItem`: Exported entity
- `ParseTypeItem`: Exported entity
- `EnableAutoGenerateItem`: Exported entity
- `EnableTocToggle`: Exported entity

### Functions (6)

- `ChunkMethodItem()`: Function definition
- `EmbeddingModelItem()`: Function definition
- `oldValue()`: Function definition
- `ParseTypeItem()`: Function definition
- `EnableAutoGenerateItem()`: Function definition
- `EnableTocToggle()`: Function definition

### Imports (10)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import {`
- `import { Radio } from '@/components/ui/radio';`
- `import { Spin } from '@/components/ui/spin';`
- `import { Switch } from '@/components/ui/switch';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { cn } from '@/lib/utils';`
- `import { useMemo, useState } from 'react';`
- `import { useFormContext } from 'react-hook-form';`
- `import {`

## Code Structure Analysis

- Total lines: 269
- Blank lines: 8 (3.0%)
- Comment lines: ~2 (0.7%)
- Code lines: ~259


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ui/radio`
- `@/components/ui/spin`
- `@/components/ui/switch`
- `@/hooks/common-hooks`
- `@/lib/utils`
- `react`
- `react-hook-form`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting/configuration`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataset/dataset-setting/configuration/` directory
- Potential test file: `test_common-item.tsx`

## Keywords

@/components/originui/select-with-search, @/components/ui/radio, @/components/ui/spin, @/components/ui/switch, @/hooks/common-hooks, @/lib/utils, ChunkMethodItem, EmbeddingModelItem, EnableAutoGenerateItem, EnableTocToggle, FormControl, FormField, FormItem, FormLabel, FormMessage, Group, IProps, ParseTypeItem, Radio, SelectWithSearch, Spin, Switch, TypeScript, disabled, embdStr, embeddingModelOptions, form, handleChunkMethodSelectChange, oldValue, parserList, react, react-hook-form, res

---
*Generated by RAGFlow Repository Documentation Generator*
