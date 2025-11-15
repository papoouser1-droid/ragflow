# File Documentation: web/src/pages/next-chats/chat/app-settings/chat-prompt-engine.tsx

## File Metadata

- **Path**: `web/src/pages/next-chats/chat/app-settings/chat-prompt-engine.tsx`
- **Extension**: `.tsx`
- **Lines**: 64
- **Characters**: 2,106
- **Size**: 2,106 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
'use client';

import { CrossLanguageFormField } from '@/components/cross-language-form-field';
import { RerankFormFields } from '@/components/rerank';
import { SimilaritySliderFormField } from '@/components/similarity-slider';
import { SwitchFormField } from '@/components/switch-fom-field';
import { TopNFormField } from '@/components/top-n-item';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Textarea } from '@/components/ui/textarea';
import { UseKnowledgeGraphFormField } from '@/components/use-knowledge-graph-item';
import { useTranslate } from '@/hooks/common-hooks';
import { useFormContext } from 'react-hook-form';
import { DynamicVariableForm } from './dynamic-variable';

export function ChatPromptEngine() {
  const { t } = useTranslate('chat');
  const form = useFormContext();

  return (
    <div className="space-y-8">
      <FormField
        control={form.control}
        name="prompt_config.system"
        render={({ field }) => (
          <FormItem>
            <FormLabel>{t('system')}</FormLabel>
            <FormControl>
              <Textarea
                {...field}
                rows={8}
                placeholder={t('messagePlaceholder')}
                className="overflow-y-auto"
              />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
      <SimilaritySliderFormField isTooltipShown></SimilaritySliderFormField>
      <TopNFormField></TopNFormField>
      <SwitchFormField
        name={'prompt_config.refine_multiturn'}
        label={t('multiTurn')}
        tooltip={t('multiTurnTip')}
      ></SwitchFormField>
      <UseKnowledgeGraphFormField name="prompt_config.use_kg"></UseKnowledgeGraphFormField>
      <SwitchFormField
        name={'prompt_config.reasoning'}
        label={t('reasoning')}
        tooltip={t('reasoningTip')}
      ></SwitchFormField>
      <RerankFormFields></RerankFormFields>
      <CrossLanguageFormField></CrossLanguageFormField>
      <DynamicVariableForm></DynamicVariableForm>
    </div>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/next-chats/chat/app-settings/chat-prompt-engine.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 64 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ChatPromptEngine`: Exported entity

### Functions (1)

- `ChatPromptEngine()`: Function definition

### Imports (11)

- `import { CrossLanguageFormField } from '@/components/cross-language-form-field';`
- `import { RerankFormFields } from '@/components/rerank';`
- `import { SimilaritySliderFormField } from '@/components/similarity-slider';`
- `import { SwitchFormField } from '@/components/switch-fom-field';`
- `import { TopNFormField } from '@/components/top-n-item';`
- `import {`
- `import { Textarea } from '@/components/ui/textarea';`
- `import { UseKnowledgeGraphFormField } from '@/components/use-knowledge-graph-item';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useFormContext } from 'react-hook-form';`

## Code Structure Analysis

- Total lines: 64
- Blank lines: 4 (6.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~60


## Dependencies and Imports

- `@/components/cross-language-form-field`
- `@/components/rerank`
- `@/components/similarity-slider`
- `@/components/switch-fom-field`
- `@/components/top-n-item`
- `@/components/ui/textarea`
- `@/components/use-knowledge-graph-item`
- `@/hooks/common-hooks`
- `react-hook-form`
- `./dynamic-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-chats/chat/app-settings`.

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

- Other files in `web/src/pages/next-chats/chat/app-settings/` directory
- Potential test file: `test_chat-prompt-engine.tsx`

## Keywords

./dynamic-variable, @/components/cross-language-form-field, @/components/rerank, @/components/similarity-slider, @/components/switch-fom-field, @/components/top-n-item, @/components/ui/textarea, @/components/use-knowledge-graph-item, @/hooks/common-hooks, ChatPromptEngine, CrossLanguageFormField, DynamicVariableForm, FormControl, FormField, FormItem, FormLabel, FormMessage, RerankFormFields, SimilaritySliderFormField, SwitchFormField, Textarea, TopNFormField, TypeScript, UseKnowledgeGraphFormField, form, react-hook-form

---
*Generated by RAGFlow Repository Documentation Generator*
