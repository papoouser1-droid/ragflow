# File Documentation: web/src/pages/user-setting/setting-model/modal/api-key-modal/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/setting-model/modal/api-key-modal/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 178
- **Characters**: 4,974
- **Size**: 4,974 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IModalManagerChildrenProps } from '@/components/modal-manager';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { Modal } from '@/components/ui/modal/modal';
import { LLMFactory } from '@/constants/llm';
import { useTranslate } from '@/hooks/common-hooks';
import { KeyboardEventHandler, useCallback, useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { ApiKeyPostBody } from '../../../interface';

interface IProps extends Omit<IModalManagerChildrenProps, 'showModal'> {
  loading: boolean;
  initialValue: string;
  llmFactory: string;
  editMode?: boolean;
  onOk: (postBody: ApiKeyPostBody) => void;
  showModal?(): void;
}

type FieldType = {
  api_key?: string;
  base_url?: string;
  group_id?: string;
};

const modelsWithBaseUrl = [
  LLMFactory.OpenAI,
  LLMFactory.AzureOpenAI,
  LLMFactory.TongYiQianWen,
];

const ApiKeyModal = ({
  visible,
  hideModal,
  llmFactory,
  loading,
  initialValue,
  editMode = false,
  onOk,
}: IProps) => {
  const form = useForm<FieldType>();
  const { t } = useTranslate('setting');

  const handleOk = useCallback(async () => {
    await form.handleSubmit((values) => onOk(values))();
  }, [form, onOk]);

  const handleKeyDown: KeyboardEventHandler<HTMLInputElement> = useCallback(
    async (e) => {
      if (e.key === 'Enter') {
        await handleOk();
      }
    },
    [handleOk],
  );

  useEffect(() => {
    if (visible) {
      form.setValue('api_key', initialValue);
    }
  }, [initialValue, form, visible]);

  return (
    <Modal
      title={t('configureModelTitle')}
      open={visible}
      onOpenChange={(open) => !open && hideModal()}
      onOk={handleOk}
      onCancel={hideModal}
      confirmLoading={loading}
      okText={t('save')}
      cancelText={t('cancel')}
      className="!w-[600px]"
    >
      <Form {...form}>
        <div className="space-y-4 py-4">
          <FormField
            name="api_key"
            rules={{ required: t('apiKeyMessage') }}
            render={({ field }) => (
              <FormItem>
                <FormLabel
                  className="text-sm font-medium text-text-secondary"
                  required
                >
                  {t('apiKey')}
                </FormLabel>
                <FormControl>
                  <Input
                    {...field}
                    onKeyDown={handleKeyDown}
                    className="w-full"
                  />
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />

          {modelsWithBaseUrl.some((x) => x === llmFactory) && (
            <FormField
              name="base_url"
              render={({ field }) => (
                <FormItem>
                  <FormLabel className="text-sm font-medium text-text-primary">
                    {t('baseUrl')}
                  </FormLabel>
                  <FormControl>
                    <Input
                      {...field}
                      placeholder={
                        llmFactory === LLMFactory.TongYiQianWen
                          ? t('tongyiBaseUrlPlaceholder')
                          : 'https://api.openai.com/v1'
                      }
                      onKeyDown={handleKeyDown}
                      className="w-full"
                    />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          )}

          {llmFactory?.toLowerCase() === 'Anthropic'.toLowerCase() && (
            <FormField
              name="base_url"
              render={({ field }) => (
                <FormItem>
                  <FormLabel className="text-sm font-medium text-text-primary">
                    {t('baseUrl')}
                  </FormLabel>
                  <FormControl>
                    <Input
                      {...field}
                      placeholder="https://api.anthropic.com/v1"
                      onKeyDown={handleKeyDown}
                      className="w-full"
                    />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          )}

          {llmFactory?.toLowerCase() === 'Minimax'.toLowerCase() && (
            <FormField
              name="group_id"
              render={({ field }) => (
                <FormItem>
                  <FormLabel className="text-sm font-medium text-text-primary">
                    Group ID
                  </FormLabel>
                  <FormControl>
                    <Input {...field} className="w-full" />
                  </FormControl>
                  <FormMessage />
                </FormItem>
              )}
            />
          )}
        </div>
      </Form>
    </Modal>
  );
};

export default ApiKeyModal;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/setting-model/modal/api-key-modal/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 178 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (2)

- `ApiKeyModal()`: Function definition
- `handleOk()`: Function definition

### Imports (9)

- `import { IModalManagerChildrenProps } from '@/components/modal-manager';`
- `import {`
- `import { Input } from '@/components/ui/input';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { LLMFactory } from '@/constants/llm';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { KeyboardEventHandler, useCallback, useEffect } from 'react';`
- `import { useForm } from 'react-hook-form';`
- `import { ApiKeyPostBody } from '../../../interface';`

## Code Structure Analysis

- Total lines: 178
- Blank lines: 13 (7.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~165


## Dependencies and Imports

- `@/components/modal-manager`
- `@/components/ui/input`
- `@/components/ui/modal/modal`
- `@/constants/llm`
- `@/hooks/common-hooks`
- `react`
- `react-hook-form`
- `../../../interface`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/setting-model/modal/api-key-modal`.

As part of the API layer, this file likely handles HTTP requests, business logic, or data access.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/user-setting/setting-model/modal/api-key-modal/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../../interface, @/components/modal-manager, @/components/ui/input, @/components/ui/modal/modal, @/constants/llm, @/hooks/common-hooks, Anthropic, ApiKeyModal, ApiKeyPostBody, AzureOpenAI, Enter, FieldType, Form, FormControl, FormField, FormItem, FormLabel, FormMessage, Group, HTMLInputElement, IModalManagerChildrenProps, IProps, Input, KeyboardEventHandler, LLMFactory, Minimax, Modal, Omit, OpenAI, TongYiQianWen, TypeScript, form, handleKeyDown, handleOk, modelsWithBaseUrl, react, react-hook-form

---
*Generated by RAGFlow Repository Documentation Generator*
