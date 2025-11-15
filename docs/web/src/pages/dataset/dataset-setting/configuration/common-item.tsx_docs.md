# Documentation: web/src/pages/dataset/dataset-setting/configuration/common-item.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/configuration/common-item.tsx`
- **Size**: 8357 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-setting/configuration/common-item.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-setting/configuration/common-item.tsx` is located in the `web/src/pages/dataset/dataset-setting/configuration` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to configuration.

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

- [audio.tsx](audio.tsx_docs.md)
- [book.tsx](book.tsx_docs.md)
- [email.tsx](email.tsx_docs.md)
- [knowledge-graph.tsx](knowledge-graph.tsx_docs.md)
- [laws.tsx](laws.tsx_docs.md)
- [manual.tsx](manual.tsx_docs.md)
- [naive.tsx](naive.tsx_docs.md)
- [one.tsx](one.tsx_docs.md)
- [paper.tsx](paper.tsx_docs.md)
- [picture.tsx](picture.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
