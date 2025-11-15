# File Documentation: web/src/pages/next-search/embed-app-modal.tsx

## File Metadata

- **Path**: `web/src/pages/next-search/embed-app-modal.tsx`
- **Extension**: `.tsx`
- **Lines**: 150
- **Characters**: 4,647
- **Size**: 4,647 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import HightLightMarkdown from '@/components/highlight-markdown';
import { Modal } from '@/components/ui/modal/modal';
import { RAGFlowSelect } from '@/components/ui/select';
import { Switch } from '@/components/ui/switch';
import {
  LanguageAbbreviation,
  LanguageAbbreviationMap,
} from '@/constants/common';
import { useTranslate } from '@/hooks/common-hooks';
import { message } from 'antd';
import { useCallback, useMemo, useState } from 'react';

type IEmbedAppModalProps = {
  open: any;
  url: string;
  token: string;
  from: string;
  setOpen: (e: any) => void;
  tenantId: string;
  beta?: string;
};

const EmbedAppModal = (props: IEmbedAppModalProps) => {
  const { t } = useTranslate('search');
  const { open, setOpen, token = '', from, url, tenantId, beta = '' } = props;

  const [hideAvatar, setHideAvatar] = useState(false);
  const [locale, setLocale] = useState('');

  const languageOptions = useMemo(() => {
    return Object.values(LanguageAbbreviation).map((x) => ({
      label: LanguageAbbreviationMap[x],
      value: x,
    }));
  }, []);

  const generateIframeSrc = useCallback(() => {
    // const { visibleAvatar, locale } = values;
    let src = `${location.origin}${url}?shared_id=${token}&from=${from}&auth=${beta}&tenantId=${tenantId}`;
    if (hideAvatar) {
      src += '&visible_avatar=1';
    }
    if (locale) {
      src += `&locale=${locale}`;
    }
    return src;
  }, [beta, from, token, hideAvatar, locale, url, tenantId]);

  // ... existing code ...
  const text = useMemo(() => {
    const iframeSrc = generateIframeSrc();
    return `\`\`\`html
<iframe
  src="${iframeSrc}"
  style="width: 100%; height: 100%; min-height: 600px"
  frameborder="0">
</iframe>
\`\`\``;
  }, [generateIframeSrc]);
  // ... existing code ...
  return (
    <Modal
      title={t('embedIntoSite', { keyPrefix: 'common' })}
      className="!bg-bg-base !text-text-disabled"
      open={open}
      onCancel={() => setOpen(false)}
      showfooter={false}
      footer={null}
    >
      <div className="w-full">
        {/* Hide Avatar Toggle */}
        <div className="mb-6">
          <label className="block text-sm font-medium mb-2">
            {t('profile')}
          </label>
          <div className="flex items-center">
            <Switch
              checked={hideAvatar}
              onCheckedChange={(value) => {
                setHideAvatar(value);
              }}
            />
          </div>
        </div>

        {/* Locale Select */}
        <div className="mb-6">
          <label className="block text-sm font-medium mb-2">
            {t('locale')}
          </label>
          <RAGFlowSelect
            placeholder="Select a locale"
            value={locale}
            onChange={(value) => setLocale(value)}
            options={languageOptions}
          ></RAGFlowSelect>
        </div>
        {/* Embed Code */}
        <div className="mb-6">
          <label className="block text-sm font-medium mb-2">
            {t('embedCode')}
          </label>
          {/* <div className=" border rounded-lg"> */}
          {/* <pre className="text-sm whitespace-pre-wrap">{text}</pre> */}
          <HightLightMarkdown>{text}</HightLightMarkdown>
          {/* </div> */}
        </div>

        {/* ID Field */}
        <div className="mb-4">
          <label className="block text-sm font-medium mb-2">{t('id')}</label>
          <div className="flex items-center border border-border rounded-lg bg-bg-base">
            <input
              type="text"
              value={token}
              readOnly
              className="flex-1 px-4 py-2 focus:outline-none bg-bg-base rounded-lg"
            />
            <button
              type="button"
              onClick={() => {
                navigator.clipboard.writeText(token);
                message.success(t('copySuccess'));
              }}
              className="ml-2 p-2 hover:text-white transition-colors"
              title="Copy ID"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                className="h-5 w-5"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  strokeLinecap="round"
                  strokeLinejoin="round"
                  strokeWidth={2}
                  d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h10a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </Modal>
  );
};
export default EmbedAppModal;

```

## High-Level Overview

    // const { visibleAvatar, locale } = values;

## Detailed Walkthrough


### Functions (4)

- `EmbedAppModal()`: Function definition
- `languageOptions()`: Function definition
- `generateIframeSrc()`: Function definition
- `text()`: Function definition

### Imports (8)

- `import HightLightMarkdown from '@/components/highlight-markdown';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { RAGFlowSelect } from '@/components/ui/select';`
- `import { Switch } from '@/components/ui/switch';`
- `import {`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { message } from 'antd';`
- `import { useCallback, useMemo, useState } from 'react';`

## Code Structure Analysis

- Total lines: 150
- Blank lines: 9 (6.0%)
- Comment lines: ~3 (2.0%)
- Code lines: ~138


## Dependencies and Imports

- `@/components/highlight-markdown`
- `@/components/ui/modal/modal`
- `@/components/ui/select`
- `@/components/ui/switch`
- `@/hooks/common-hooks`
- `antd`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/next-search`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/next-search/` directory
- Potential test file: `test_embed-app-modal.tsx`

## Keywords

@/components/highlight-markdown, @/components/ui/modal/modal, @/components/ui/select, @/components/ui/switch, @/hooks/common-hooks, Avatar, Code, Copy, Embed, EmbedAppModal, Field, Hide, HightLightMarkdown, IEmbedAppModalProps, LanguageAbbreviation, LanguageAbbreviationMap, Locale, Modal, Object, RAGFlowSelect, Select, Switch, Toggle, TypeScript, antd, generateIframeSrc, iframeSrc, languageOptions, react, src, text

---
*Generated by RAGFlow Repository Documentation Generator*
