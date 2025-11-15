# File Documentation: web/src/pages/dataflow-result/components/chunk-creating-modal/tag-feature-item.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/chunk-creating-modal/tag-feature-item.tsx`
- **Extension**: `.tsx`
- **Lines**: 137
- **Characters**: 4,767
- **Size**: 4,767 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { SelectWithSearch } from '@/components/originui/select-with-search';
import { Button } from '@/components/ui/button';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { NumberInput } from '@/components/ui/input';
import { useFetchTagListByKnowledgeIds } from '@/hooks/knowledge-hooks';
import { useFetchKnowledgeBaseConfiguration } from '@/hooks/use-knowledge-request';
import { CircleMinus, Plus } from 'lucide-react';
import { useCallback, useEffect, useMemo } from 'react';
import { useFieldArray, useFormContext } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { FormListItem } from '../../utils';

const FieldKey = 'tag_feas';

export const TagFeatureItem = () => {
  const { t } = useTranslation();
  const { setKnowledgeIds, list } = useFetchTagListByKnowledgeIds();
  const { data: knowledgeConfiguration } = useFetchKnowledgeBaseConfiguration();
  const form = useFormContext();
  const tagKnowledgeIds = useMemo(() => {
    return knowledgeConfiguration?.parser_config?.tag_kb_ids ?? [];
  }, [knowledgeConfiguration?.parser_config?.tag_kb_ids]);

  const options = useMemo(() => {
    return list.map((x) => ({
      value: x[0],
      label: x[0],
    }));
  }, [list]);

  const filterOptions = useCallback(
    (index: number) => {
      const tags: FormListItem[] = form.getValues(FieldKey) ?? [];

      // Exclude it's own current data
      const list = tags
        .filter((x, idx) => x && index !== idx)
        .map((x) => x.tag);
      // Exclude the selected data from other options from one's own options.
      const resultList = options.filter(
        (x) => !list.some((y) => x.value === y),
      );
      return resultList;
    },
    [form, options],
  );

  useEffect(() => {
    setKnowledgeIds(tagKnowledgeIds);
  }, [setKnowledgeIds, tagKnowledgeIds]);

  const { fields, append, remove } = useFieldArray({
    control: form.control,
    name: FieldKey,
  });
  return (
    <FormField
      control={form.control}
      name={FieldKey as any}
      render={() => (
        <FormItem>
          <FormLabel>{t('knowledgeConfiguration.tags')}</FormLabel>
          <div>
            {fields.map((item, name) => {
              return (
                <div key={item.id} className="flex gap-3 items-center mb-4">
                  <div className="flex flex-1 gap-8">
                    <FormField
                      control={form.control}
                      name={`${FieldKey}.${name}.tag` as any}
                      render={({ field }) => (
                        <FormItem className="w-2/3">
                          <FormControl className="w-full">
                            <div>
                              <SelectWithSearch
                                options={filterOptions(name)}
                                placeholder={t(
                                  'knowledgeConfiguration.tagName',
                                )}
                                value={field.value}
                                onChange={field.onChange}
                              />
                            </div>
                          </FormControl>
                          <FormMessage />
                        </FormItem>
                      )}
                    />
                    <FormField
                      control={form.control}
                      name={`${FieldKey}.${name}.frequency`}
                      render={({ field }) => (
                        <FormItem>
                          <FormControl>
                            <NumberInput
                              value={field.value}
                              onChange={field.onChange}
                              placeholder={t(
                                'knowledgeConfiguration.frequency',
                              )}
                              max={10}
                              min={0}
                            />
                          </FormControl>
                          <FormMessage />
                        </FormItem>
                      )}
                    />
                  </div>
                  <CircleMinus
                    onClick={() => remove(name)}
                    className="text-red-500"
                  />
                </div>
              );
            })}
            <Button
              variant="dashed"
              className="w-full flex items-center justify-center gap-2"
              onClick={() => append({ tag: '', frequency: 0 })}
            >
              <Plus size={16} />
              {t('knowledgeConfiguration.addTag')}
            </Button>
          </div>
        </FormItem>
      )}
    />
  );
};

```

## High-Level Overview

      // Exclude it's own current data
      // Exclude the selected data from other options from one's own options.

## Detailed Walkthrough

### Exports (1)

- `TagFeatureItem`: Exported entity

### Functions (6)

- `TagFeatureItem()`: Function definition
- `tagKnowledgeIds()`: Function definition
- `options()`: Function definition
- `filterOptions()`: Function definition
- `list()`: Function definition
- `resultList()`: Function definition

### Imports (11)

- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import { Button } from '@/components/ui/button';`
- `import {`
- `import { NumberInput } from '@/components/ui/input';`
- `import { useFetchTagListByKnowledgeIds } from '@/hooks/knowledge-hooks';`
- `import { useFetchKnowledgeBaseConfiguration } from '@/hooks/use-knowledge-request';`
- `import { CircleMinus, Plus } from 'lucide-react';`
- `import { useCallback, useEffect, useMemo } from 'react';`
- `import { useFieldArray, useFormContext } from 'react-hook-form';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 137
- Blank lines: 8 (5.8%)
- Comment lines: ~2 (1.5%)
- Code lines: ~127


## Dependencies and Imports

- `@/components/originui/select-with-search`
- `@/components/ui/button`
- `@/components/ui/input`
- `@/hooks/knowledge-hooks`
- `@/hooks/use-knowledge-request`
- `lucide-react`
- `react`
- `react-hook-form`
- `react-i18next`
- `../../utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataflow-result/components/chunk-creating-modal`.

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

- Other files in `web/src/pages/dataflow-result/components/chunk-creating-modal/` directory
- Potential test file: `test_tag-feature-item.tsx`

## Keywords

../../utils, @/components/originui/select-with-search, @/components/ui/button, @/components/ui/input, @/hooks/knowledge-hooks, @/hooks/use-knowledge-request, Button, CircleMinus, Exclude, FieldKey, FormControl, FormField, FormItem, FormLabel, FormListItem, FormMessage, NumberInput, Plus, SelectWithSearch, TagFeatureItem, TypeScript, filterOptions, form, list, lucide-react, options, react, react-hook-form, react-i18next, resultList, tagKnowledgeIds, tags

---
*Generated by RAGFlow Repository Documentation Generator*
