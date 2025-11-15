# Documentation: web/src/components/layout-recognize-form-field.tsx

## File Metadata

- **Path**: `web/src/components/layout-recognize-form-field.tsx`
- **Size**: 3227 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/layout-recognize-form-field.tsx`.

## Original Source Code

```tsx
import { LlmModelType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { useSelectLlmOptionsByModelType } from '@/hooks/llm-hooks';
import { cn } from '@/lib/utils';
import { camelCase } from 'lodash';
import { ReactNode, useMemo } from 'react';
import { useFormContext } from 'react-hook-form';
import { SelectWithSearch } from './originui/select-with-search';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from './ui/form';

export const enum ParseDocumentType {
  DeepDOC = 'DeepDOC',
  PlainText = 'Plain Text',
  MinerU = 'MinerU',
  Docling = 'Docling',
  TCADPParser = 'TCADP Parser',
}

export function LayoutRecognizeFormField({
  name = 'parser_config.layout_recognize',
  horizontal = true,
  optionsWithoutLLM,
  label,
}: {
  name?: string;
  horizontal?: boolean;
  optionsWithoutLLM?: { value: string; label: string }[];
  label?: ReactNode;
}) {
  const form = useFormContext();

  const { t } = useTranslate('knowledgeDetails');
  const allOptions = useSelectLlmOptionsByModelType();

  const options = useMemo(() => {
    const list = optionsWithoutLLM
      ? optionsWithoutLLM
      : [
          ParseDocumentType.DeepDOC,
          ParseDocumentType.PlainText,
          ParseDocumentType.MinerU,
          ParseDocumentType.Docling,
          ParseDocumentType.TCADPParser,
        ].map((x) => ({
          label: x === ParseDocumentType.PlainText ? t(camelCase(x)) : x,
          value: x,
        }));

    const image2TextList = allOptions[LlmModelType.Image2text].map((x) => {
      return {
        ...x,
        options: x.options.map((y) => {
          return {
            ...y,
            label: (
              <div className="flex justify-between items-center gap-2">
                {y.label}
                <span className="text-red-500 text-sm">Experimental</span>
              </div>
            ),
          };
        }),
      };
    });

    return [...list, ...image2TextList];
  }, [allOptions, optionsWithoutLLM, t]);

  return (
    <FormField
      control={form.control}
      name={name}
      render={({ field }) => {
        return (
          <FormItem className={'items-center space-y-0 '}>
            <div
              className={cn('flex', {
                'flex-col ': !horizontal,
                'items-center': horizontal,
              })}
            >
              <FormLabel
                tooltip={t('layoutRecognizeTip')}
                className={cn('text-sm text-text-secondary whitespace-wrap', {
                  ['w-1/4']: horizontal,
                })}
              >
                {label || t('layoutRecognize')}
              </FormLabel>
              <div className={horizontal ? 'w-3/4' : 'w-full'}>
                <FormControl>
                  <SelectWithSearch
                    {...field}
                    options={options}
                  ></SelectWithSearch>
                </FormControl>
              </div>
            </div>
            <div className="flex pt-1">
              <div className={horizontal ? 'w-1/4' : 'w-full'}></div>
              <FormMessage />
            </div>
          </FormItem>
        );
      }}
    />
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/layout-recognize-form-field.tsx` is located in the `web/src/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [auto-keywords-form-field.tsx](auto-keywords-form-field.tsx_docs.md)
- [auto-keywords-item.tsx](auto-keywords-item.tsx_docs.md)
- [avatar-upload.tsx](avatar-upload.tsx_docs.md)
- [bulk-operate-bar.tsx](bulk-operate-bar.tsx_docs.md)
- [card-container.tsx](card-container.tsx_docs.md)
- [collapse.tsx](collapse.tsx_docs.md)
- [confirm-delete-dialog.tsx](confirm-delete-dialog.tsx_docs.md)
- [copy-to-clipboard.tsx](copy-to-clipboard.tsx_docs.md)
- [cross-language-form-field.tsx](cross-language-form-field.tsx_docs.md)
- [cross-language-item.tsx](cross-language-item.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
