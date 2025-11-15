# File Documentation: web/src/components/api-service/embed-modal/index.tsx

## File Metadata

- **Path**: `web/src/components/api-service/embed-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 171
- **Characters**: 4,239
- **Size**: 4,239 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import CopyToClipboard from '@/components/copy-to-clipboard';
import HightLightMarkdown from '@/components/highlight-markdown';
import { SharedFrom } from '@/constants/chat';
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import {
  Card,
  Checkbox,
  Form,
  Modal,
  Select,
  Tabs,
  TabsProps,
  Typography,
} from 'antd';
import { useMemo, useState } from 'react';

import { useIsDarkTheme } from '@/components/theme-provider';
import {
  LanguageAbbreviation,
  LanguageAbbreviationMap,
} from '@/constants/common';
import { cn } from '@/lib/utils';
import styles from './index.less';

const { Paragraph, Link } = Typography;

const EmbedModal = ({
  visible,
  hideModal,
  token = '',
  form,
  beta = '',
  isAgent,
}: IModalProps<any> & {
  token: string;
  form: SharedFrom;
  beta: string;
  isAgent: boolean;
}) => {
  const { t } = useTranslate('chat');
  const isDarkTheme = useIsDarkTheme();

  const [visibleAvatar, setVisibleAvatar] = useState(false);
  const [locale, setLocale] = useState('');

  const languageOptions = useMemo(() => {
    return Object.values(LanguageAbbreviation).map((x) => ({
      label: LanguageAbbreviationMap[x],
      value: x,
    }));
  }, []);

  const generateIframeSrc = () => {
    let src = `${location.origin}/chat/share?shared_id=${token}&from=${form}&auth=${beta}`;
    if (visibleAvatar) {
      src += '&visible_avatar=1';
    }
    if (locale) {
      src += `&locale=${locale}`;
    }
    return src;
  };

  const iframeSrc = generateIframeSrc();

  const text = `
  ~~~ html
  <iframe
  src="${iframeSrc}"
  style="width: 100%; height: 100%; min-height: 600px"
  frameborder="0"
>
</iframe>
~~~
  `;

  const items: TabsProps['items'] = [
    {
      key: '1',
      label: t('fullScreenTitle'),
      children: (
        <Card
          title={t('fullScreenDescription')}
          extra={<CopyToClipboard text={text}></CopyToClipboard>}
          className={styles.codeCard}
        >
          <div className="p-2">
            <h2 className="mb-3">Option:</h2>

            <Form.Item
              label={t('avatarHidden')}
              labelCol={{ span: 6 }}
              wrapperCol={{ span: 18 }}
            >
              <Checkbox
                checked={visibleAvatar}
                onChange={(e) => setVisibleAvatar(e.target.checked)}
              ></Checkbox>
            </Form.Item>
            <Form.Item
              label={t('locale')}
              labelCol={{ span: 6 }}
              wrapperCol={{ span: 18 }}
            >
              <Select
                placeholder="Select a locale"
                onChange={(value) => setLocale(value)}
                options={languageOptions}
                style={{ width: '100%' }}
              />
            </Form.Item>
          </div>
          <HightLightMarkdown>{text}</HightLightMarkdown>
        </Card>
      ),
    },
    {
      key: '2',
      label: t('partialTitle'),
      children: t('comingSoon'),
    },
    {
      key: '3',
      label: t('extensionTitle'),
      children: t('comingSoon'),
    },
  ];

  const onChange = (key: string) => {
    console.log(key);
  };

  return (
    <Modal
      title={t('embedIntoSite', { keyPrefix: 'common' })}
      open={visible}
      style={{ top: 300 }}
      width={'50vw'}
      onOk={hideModal}
      onCancel={hideModal}
    >
      <Tabs defaultActiveKey="1" items={items} onChange={onChange} />
      <div className="text-base font-medium mt-4 mb-1">
        {t(isAgent ? 'flow' : 'chat', { keyPrefix: 'header' })}
        <span className="ml-1 inline-block">ID</span>
      </div>
      <Paragraph
        copyable={{ text: token }}
        className={cn(styles.id, {
          [styles.darkId]: isDarkTheme,
        })}
      >
        {token}
      </Paragraph>
      <Link
        href={
          isAgent
            ? 'https://ragflow.io/docs/dev/http_api_reference#create-session-with-agent'
            : 'https://ragflow.io/docs/dev/http_api_reference#create-session-with-chat-assistant'
        }
        target="_blank"
      >
        {t('howUseId', { keyPrefix: isAgent ? 'flow' : 'chat' })}
      </Link>
    </Modal>
  );
};

export default EmbedModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/api-service/embed-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 171 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `EmbedModal()`: Function definition
- `languageOptions()`: Function definition
- `generateIframeSrc()`: Function definition
- `onChange()`: Function definition

### Imports (11)

- `import CopyToClipboard from '@/components/copy-to-clipboard';`
- `import HightLightMarkdown from '@/components/highlight-markdown';`
- `import { SharedFrom } from '@/constants/chat';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IModalProps } from '@/interfaces/common';`
- `import {`
- `import { useMemo, useState } from 'react';`
- `import { useIsDarkTheme } from '@/components/theme-provider';`
- `import {`
- `import { cn } from '@/lib/utils';`

## Code Structure Analysis

- Total lines: 171
- Blank lines: 14 (8.2%)
- Comment lines: ~0 (0.0%)
- Code lines: ~157


## Dependencies and Imports

- `@/components/copy-to-clipboard`
- `@/components/highlight-markdown`
- `@/constants/chat`
- `@/hooks/common-hooks`
- `@/interfaces/common`
- `react`
- `@/components/theme-provider`
- `@/lib/utils`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/api-service/embed-modal`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/api-service/embed-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

./index.less, @/components/copy-to-clipboard, @/components/highlight-markdown, @/components/theme-provider, @/constants/chat, @/hooks/common-hooks, @/interfaces/common, @/lib/utils, Card, Checkbox, CopyToClipboard, EmbedModal, Form, HightLightMarkdown, IModalProps, Item, LanguageAbbreviation, LanguageAbbreviationMap, Link, Modal, Object, Option, Paragraph, Select, SharedFrom, Tabs, TabsProps, TypeScript, Typography, generateIframeSrc, iframeSrc, isDarkTheme, items, languageOptions, onChange, react, src, text

---
*Generated by RAGFlow Repository Documentation Generator*
