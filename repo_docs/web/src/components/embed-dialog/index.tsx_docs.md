# File Documentation: web/src/components/embed-dialog/index.tsx

## File Metadata

- **Path**: `web/src/components/embed-dialog/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 269
- **Characters**: 8,953
- **Size**: 8,953 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import CopyToClipboard from '@/components/copy-to-clipboard';
import HightLightMarkdown from '@/components/highlight-markdown';
import { SelectWithSearch } from '@/components/originui/select-with-search';
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Label } from '@/components/ui/label';
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group';
import { Switch } from '@/components/ui/switch';
import { SharedFrom } from '@/constants/chat';
import {
  LanguageAbbreviation,
  LanguageAbbreviationMap,
} from '@/constants/common';
import { useTranslate } from '@/hooks/common-hooks';
import { IModalProps } from '@/interfaces/common';
import { Routes } from '@/routes';
import { zodResolver } from '@hookform/resolvers/zod';
import { memo, useCallback, useMemo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { z } from 'zod';

const FormSchema = z.object({
  visibleAvatar: z.boolean(),
  locale: z.string(),
  embedType: z.enum(['fullscreen', 'widget']),
  enableStreaming: z.boolean(),
});

type IProps = IModalProps<any> & {
  token: string;
  from: SharedFrom;
  beta: string;
  isAgent: boolean;
};

function EmbedDialog({
  hideModal,
  token = '',
  from,
  beta = '',
  isAgent,
}: IProps) {
  const { t } = useTranslate('chat');

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      visibleAvatar: false,
      locale: '',
      embedType: 'fullscreen' as const,
      enableStreaming: false,
    },
  });

  const values = useWatch({ control: form.control });

  const languageOptions = useMemo(() => {
    return Object.values(LanguageAbbreviation).map((x) => ({
      label: LanguageAbbreviationMap[x],
      value: x,
    }));
  }, []);

  const generateIframeSrc = useCallback(() => {
    const { visibleAvatar, locale, embedType, enableStreaming } = values;
    const baseRoute =
      embedType === 'widget'
        ? Routes.ChatWidget
        : from === SharedFrom.Agent
          ? Routes.AgentShare
          : Routes.ChatShare;
    let src = `${location.origin}${baseRoute}?shared_id=${token}&from=${from}&auth=${beta}`;
    if (visibleAvatar) {
      src += '&visible_avatar=1';
    }
    if (locale) {
      src += `&locale=${locale}`;
    }
    if (enableStreaming) {
      src += '&streaming=true';
    }
    return src;
  }, [beta, from, token, values]);

  const text = useMemo(() => {
    const iframeSrc = generateIframeSrc();
    const { embedType } = values;

    if (embedType === 'widget') {
      const { enableStreaming } = values;
      const streamingParam = enableStreaming
        ? '&streaming=true'
        : '&streaming=false';
      return `
  ~~~ html
  <iframe src="${iframeSrc}&mode=master${streamingParam}"
    style="position:fixed;bottom:0;right:0;width:100px;height:100px;border:none;background:transparent;z-index:9999"
    frameborder="0" allow="microphone;camera"></iframe>
  <script>
  window.addEventListener('message',e=>{
    if(e.origin!=='${location.origin.replace(/:\d+/, ':9222')}')return;
    if(e.data.type==='CREATE_CHAT_WINDOW'){
      if(document.getElementById('chat-win'))return;
      const i=document.createElement('iframe');
      i.id='chat-win';i.src=e.data.src;
      i.style.cssText='position:fixed;bottom:104px;right:24px;width:380px;height:500px;border:none;background:transparent;z-index:9998;display:none';
      i.frameBorder='0';i.allow='microphone;camera';
      document.body.appendChild(i);
    }else if(e.data.type==='TOGGLE_CHAT'){
      const w=document.getElementById('chat-win');
      if(w)w.style.display=e.data.isOpen?'block':'none';
    }else if(e.data.type==='SCROLL_PASSTHROUGH')window.scrollBy(0,e.data.deltaY);
  });
  </script>
~~~
  `;
    } else {
      return `
  ~~~ html
  <iframe
  src="${iframeSrc}"
  style="width: 100%; height: 100%; min-height: 600px"
  frameborder="0"
>
</iframe>
~~~
  `;
    }
  }, [generateIframeSrc, values]);

  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>
            {t('embedIntoSite', { keyPrefix: 'common' })}
          </DialogTitle>
        </DialogHeader>
        <section className="w-full overflow-auto space-y-5 text-sm text-text-secondary">
          <Form {...form}>
            <form className="space-y-5">
              <FormField
                control={form.control}
                name="embedType"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Embed Type</FormLabel>
                    <FormControl>
                      <RadioGroup
                        onValueChange={field.onChange}
                        value={field.value}
                        className="flex flex-col space-y-2"
                      >
                        <div className="flex items-center space-x-2">
                          <RadioGroupItem value="fullscreen" id="fullscreen" />
                          <Label htmlFor="fullscreen" className="text-sm">
                            Fullscreen Chat (Traditional iframe)
                          </Label>
                        </div>
                        <div className="flex items-center space-x-2">
                          <RadioGroupItem value="widget" id="widget" />
                          <Label htmlFor="widget" className="text-sm">
                            Floating Widget (Intercom-style)
                          </Label>
                        </div>
                      </RadioGroup>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="visibleAvatar"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>{t('avatarHidden')}</FormLabel>
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
              {values.embedType === 'widget' && (
                <FormField
                  control={form.control}
                  name="enableStreaming"
                  render={({ field }) => (
                    <FormItem>
                      <FormLabel>Enable Streaming Responses</FormLabel>
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
              )}
              <FormField
                control={form.control}
                name="locale"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>{t('locale')}</FormLabel>
                    <FormControl>
                      <SelectWithSearch
                        {...field}
                        options={languageOptions}
                      ></SelectWithSearch>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
            </form>
          </Form>
          <div className="max-h-[350px] overflow-auto">
            <span>{t('embedCode', { keyPrefix: 'search' })}</span>
            <div className="max-h-full overflow-y-auto">
              <HightLightMarkdown>{text}</HightLightMarkdown>
            </div>
          </div>
          <div className=" font-medium mt-4 mb-1">
            {t(isAgent ? 'flow' : 'chat', { keyPrefix: 'header' })}
            <span className="ml-1 inline-block">ID</span>
          </div>
          <div className="bg-bg-card rounded-lg flex justify-between p-2">
            <span>{token} </span>
            <CopyToClipboard text={token}></CopyToClipboard>
          </div>
          <a
            className="cursor-pointer text-accent-primary inline-block"
            href={
              isAgent
                ? 'https://ragflow.io/docs/dev/http_api_reference#create-session-with-agent'
                : 'https://ragflow.io/docs/dev/http_api_reference#create-session-with-chat-assistant'
            }
            target="_blank"
            rel="noreferrer"
          >
            {t('howUseId', { keyPrefix: isAgent ? 'flow' : 'chat' })}
          </a>
        </section>
      </DialogContent>
    </Dialog>
  );
}

export default memo(EmbedDialog);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/embed-dialog/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 269 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `EmbedDialog()`: Function definition
- `languageOptions()`: Function definition
- `generateIframeSrc()`: Function definition
- `text()`: Function definition

### Imports (17)

- `import CopyToClipboard from '@/components/copy-to-clipboard';`
- `import HightLightMarkdown from '@/components/highlight-markdown';`
- `import { SelectWithSearch } from '@/components/originui/select-with-search';`
- `import {`
- `import {`
- `import { Label } from '@/components/ui/label';`
- `import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group';`
- `import { Switch } from '@/components/ui/switch';`
- `import { SharedFrom } from '@/constants/chat';`
- `import {`

## Code Structure Analysis

- Total lines: 269
- Blank lines: 12 (4.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~257


## Dependencies and Imports

- `@/components/copy-to-clipboard`
- `@/components/highlight-markdown`
- `@/components/originui/select-with-search`
- `@/components/ui/label`
- `@/components/ui/radio-group`
- `@/components/ui/switch`
- `@/constants/chat`
- `@/hooks/common-hooks`
- `@/interfaces/common`
- `@/routes`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `zod`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/embed-dialog`.

This appears to be a UI component or frontend module.

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

- Other files in `web/src/components/embed-dialog/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/components/copy-to-clipboard, @/components/highlight-markdown, @/components/originui/select-with-search, @/components/ui/label, @/components/ui/radio-group, @/components/ui/switch, @/constants/chat, @/hooks/common-hooks, @/interfaces/common, @/routes, @hookform/resolvers/zod, Agent, AgentShare, CREATE_CHAT_WINDOW, Chat, ChatShare, ChatWidget, CopyToClipboard, Dialog, DialogContent, DialogHeader, DialogTitle, Embed, EmbedDialog, Enable, Floating, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, FormSchema, Fullscreen, HightLightMarkdown, IModalProps, IProps, Intercom, Label, LanguageAbbreviation, LanguageAbbreviationMap, Object, RadioGroup, RadioGroupItem, Responses, Routes, SCROLL_PASSTHROUGH, SelectWithSearch, SharedFrom, Streaming...

---
*Generated by RAGFlow Repository Documentation Generator*
