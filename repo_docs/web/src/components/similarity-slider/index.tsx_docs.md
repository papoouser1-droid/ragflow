# File Documentation: web/src/components/similarity-slider/index.tsx

## File Metadata

- **Path**: `web/src/components/similarity-slider/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 176
- **Characters**: 5,438
- **Size**: 5,438 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { FormLayout } from '@/constants/form';
import { useTranslate } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import { Form, Slider } from 'antd';
import { useFormContext } from 'react-hook-form';
import { z } from 'zod';
import { SliderInputFormField } from '../slider-input-form-field';
import { SingleFormSlider } from '../ui/dual-range-slider';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '../ui/form';
import { NumberInput } from '../ui/input';

type FieldType = {
  similarity_threshold?: number;
  // vector_similarity_weight?: number;
};

interface IProps {
  isTooltipShown?: boolean;
  vectorSimilarityWeightName?: string;
}

const SimilaritySlider = ({
  isTooltipShown = false,
  vectorSimilarityWeightName = 'vector_similarity_weight',
}: IProps) => {
  const { t } = useTranslate('knowledgeDetails');

  return (
    <>
      <Form.Item<FieldType>
        label={t('similarityThreshold')}
        name={'similarity_threshold'}
        tooltip={isTooltipShown && t('similarityThresholdTip')}
        initialValue={0.2}
      >
        <Slider max={1} step={0.01} />
      </Form.Item>
      <Form.Item
        label={t('vectorSimilarityWeight')}
        name={vectorSimilarityWeightName}
        initialValue={1 - 0.3}
        tooltip={isTooltipShown && t('vectorSimilarityWeightTip')}
      >
        <Slider max={1} step={0.01} />
      </Form.Item>
    </>
  );
};

export default SimilaritySlider;

interface SimilaritySliderFormFieldProps {
  similarityName?: string;
  vectorSimilarityWeightName?: string;
  isTooltipShown?: boolean;
}

export const initialSimilarityThresholdValue = {
  similarity_threshold: 0.2,
};
export const initialKeywordsSimilarityWeightValue = {
  keywords_similarity_weight: 0.7,
};

export const similarityThresholdSchema = { similarity_threshold: z.number() };

export const keywordsSimilarityWeightSchema = {
  keywords_similarity_weight: z.number(),
};

export const vectorSimilarityWeightSchema = {
  vector_similarity_weight: z.number(),
};

export const initialVectorSimilarityWeightValue = {
  vector_similarity_weight: 0.3,
};

export function SimilaritySliderFormField({
  similarityName = 'similarity_threshold',
  vectorSimilarityWeightName = 'vector_similarity_weight',
  isTooltipShown,
}: SimilaritySliderFormFieldProps) {
  const { t } = useTranslate('knowledgeDetails');
  const form = useFormContext();
  const isVector =
    vectorSimilarityWeightName.indexOf('vector_similarity_weight') > -1;

  return (
    <>
      <SliderInputFormField
        name={similarityName}
        label={t('similarityThreshold')}
        max={1}
        step={0.01}
        layout={FormLayout.Vertical}
        tooltip={isTooltipShown && t('similarityThresholdTip')}
      ></SliderInputFormField>
      <FormField
        control={form.control}
        name={vectorSimilarityWeightName}
        defaultValue={0}
        render={({ field }) => (
          <FormItem
          // className={cn({ 'flex items-center gap-1 space-y-0': isHorizontal })}
          >
            <FormLabel
              tooltip={
                isTooltipShown &&
                t(
                  isVector
                    ? 'vectorSimilarityWeightTip'
                    : 'keywordSimilarityWeightTip',
                )
              }
            >
              {t(
                isVector ? 'vectorSimilarityWeight' : 'keywordSimilarityWeight',
              )}
            </FormLabel>
            <div className={cn('flex items-end gap-14 justify-between')}>
              <FormControl>
                <div className="flex flex-col flex-1 gap-2">
                  <div className="flex justify-between items-center">
                    <div className="flex items-center gap-1">
                      <label className="italic text-xs text-text-secondary">
                        vector
                      </label>
                      <span className="bg-bg-card rounded-md p-1 w-10 text-center text-xs">
                        {field.value.toFixed(2)}
                      </span>
                    </div>
                    <div className="flex  items-center gap-1">
                      <label className="italic text-xs text-text-secondary">
                        full-text
                      </label>
                      <span className="bg-bg-card rounded-md p-1 w-10 text-center text-xs">
                        {(1 - field.value).toFixed(2)}
                      </span>
                    </div>
                  </div>
                  <SingleFormSlider
                    {...field}
                    max={1}
                    step={0.01}
                    min={0}
                  ></SingleFormSlider>
                </div>
              </FormControl>
              <FormControl>
                <NumberInput
                  className={cn(
                    'h-6 w-10 p-0 text-center bg-bg-input border-border-default border text-text-secondary',
                    '[appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none',
                  )}
                  max={1}
                  min={0}
                  step={0.01}
                  {...field}
                ></NumberInput>
              </FormControl>
            </div>
            <FormMessage />
          </FormItem>
        )}
      />
    </>
  );
}

```

## High-Level Overview

  // vector_similarity_weight?: number;

## Detailed Walkthrough

### Exports (7)

- `initialSimilarityThresholdValue`: Exported entity
- `initialKeywordsSimilarityWeightValue`: Exported entity
- `similarityThresholdSchema`: Exported entity
- `keywordsSimilarityWeightSchema`: Exported entity
- `vectorSimilarityWeightSchema`: Exported entity
- `initialVectorSimilarityWeightValue`: Exported entity
- `SimilaritySliderFormField`: Exported entity

### Functions (2)

- `SimilaritySlider()`: Function definition
- `SimilaritySliderFormField()`: Function definition

### Imports (10)

- `import { FormLayout } from '@/constants/form';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { cn } from '@/lib/utils';`
- `import { Form, Slider } from 'antd';`
- `import { useFormContext } from 'react-hook-form';`
- `import { z } from 'zod';`
- `import { SliderInputFormField } from '../slider-input-form-field';`
- `import { SingleFormSlider } from '../ui/dual-range-slider';`
- `import {`
- `import { NumberInput } from '../ui/input';`

## Code Structure Analysis

- Total lines: 176
- Blank lines: 14 (8.0%)
- Comment lines: ~2 (1.1%)
- Code lines: ~160


## Dependencies and Imports

- `@/constants/form`
- `@/hooks/common-hooks`
- `@/lib/utils`
- `antd`
- `react-hook-form`
- `zod`
- `../slider-input-form-field`
- `../ui/dual-range-slider`
- `../ui/input`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/similarity-slider`.

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

- Other files in `web/src/components/similarity-slider/` directory
- Potential test file: `test_index.tsx`

## Keywords

../slider-input-form-field, ../ui/dual-range-slider, ../ui/input, @/constants/form, @/hooks/common-hooks, @/lib/utils, FieldType, Form, FormControl, FormField, FormItem, FormLabel, FormLayout, FormMessage, IProps, Item, NumberInput, SimilaritySlider, SimilaritySliderFormField, SimilaritySliderFormFieldProps, SingleFormSlider, Slider, SliderInputFormField, TypeScript, Vertical, antd, form, initialKeywordsSimilarityWeightValue, initialSimilarityThresholdValue, initialVectorSimilarityWeightValue, isVector, keywordsSimilarityWeightSchema, react-hook-form, similarityThresholdSchema, vectorSimilarityWeightSchema, zod

---
*Generated by RAGFlow Repository Documentation Generator*
