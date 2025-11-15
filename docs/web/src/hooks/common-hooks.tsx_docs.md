# Documentation: web/src/hooks/common-hooks.tsx

## File Metadata

- **Path**: `web/src/hooks/common-hooks.tsx`
- **Size**: 3342 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/hooks/common-hooks.tsx`.

## Original Source Code

```tsx
import { ExclamationCircleFilled } from '@ant-design/icons';
import { App } from 'antd';
import isEqual from 'lodash/isEqual';
import { ReactNode, useCallback, useEffect, useRef, useState } from 'react';
import { useTranslation } from 'react-i18next';

export const useSetModalState = (initialVisible = false) => {
  const [visible, setVisible] = useState(initialVisible);

  const showModal = useCallback(() => {
    setVisible(true);
  }, []);
  const hideModal = useCallback(() => {
    setVisible(false);
  }, []);

  const switchVisible = useCallback(() => {
    setVisible(!visible);
  }, [visible]);

  return { visible, showModal, hideModal, switchVisible };
};

export const useDeepCompareEffect = (
  effect: React.EffectCallback,
  deps: React.DependencyList,
) => {
  const ref = useRef<React.DependencyList>();
  let callback: ReturnType<React.EffectCallback> = () => {};
  if (!isEqual(deps, ref.current)) {
    callback = effect();
    ref.current = deps;
  }
  useEffect(() => {
    return () => {
      if (callback) {
        callback();
      }
    };
  }, []);
};

export interface UseDynamicSVGImportOptions {
  onCompleted?: (
    name: string,
    SvgIcon: React.FC<React.SVGProps<SVGSVGElement>> | undefined,
  ) => void;
  onError?: (err: Error) => void;
}

export function useDynamicSVGImport(
  name: string,
  options: UseDynamicSVGImportOptions = {},
) {
  const ImportedIconRef = useRef<React.FC<React.SVGProps<SVGSVGElement>>>();
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error>();

  const { onCompleted, onError } = options;
  useEffect(() => {
    setLoading(true);
    const importIcon = async (): Promise<void> => {
      try {
        ImportedIconRef.current = (await import(name)).ReactComponent;
        onCompleted?.(name, ImportedIconRef.current);
      } catch (err: any) {
        onError?.(err);
        setError(err);
      } finally {
        setLoading(false);
      }
    };
    importIcon();
  }, [name, onCompleted, onError]);

  return { error, loading, SvgIcon: ImportedIconRef.current };
}

interface IProps {
  title?: string;
  content?: ReactNode;
  onOk?: (...args: any[]) => any;
  onCancel?: (...args: any[]) => any;
}

export const useShowDeleteConfirm = () => {
  const { modal } = App.useApp();
  const { t } = useTranslation();

  const showDeleteConfirm = useCallback(
    ({ title, content, onOk, onCancel }: IProps): Promise<number> => {
      return new Promise((resolve, reject) => {
        modal.confirm({
          title: title ?? t('common.deleteModalTitle'),
          icon: <ExclamationCircleFilled />,
          content,
          okText: t('common.yes'),
          okType: 'danger',
          cancelText: t('common.no'),
          async onOk() {
            try {
              const ret = await onOk?.();
              resolve(ret);
              console.info(ret);
            } catch (error) {
              reject(error);
            }
          },
          onCancel() {
            onCancel?.();
          },
        });
      });
    },
    [t, modal],
  );

  return showDeleteConfirm;
};

export const useTranslate = (keyPrefix: string) => {
  return useTranslation('translation', { keyPrefix });
};

export const useCommonTranslation = () => {
  return useTranslation('translation', { keyPrefix: 'common' });
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/hooks/common-hooks.tsx` is located in the `web/src/hooks` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to hooks.

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

- [auth-hooks.ts](auth-hooks.ts_docs.md)
- [chat-hooks.ts](chat-hooks.ts_docs.md)
- [chunk-hooks.ts](chunk-hooks.ts_docs.md)
- [document-hooks.ts](document-hooks.ts_docs.md)
- [file-manager-hooks.ts](file-manager-hooks.ts_docs.md)
- [flow-hooks.ts](flow-hooks.ts_docs.md)
- [knowledge-hooks.ts](knowledge-hooks.ts_docs.md)
- [llm-hooks.tsx](llm-hooks.tsx_docs.md)
- [logic-hooks.ts](logic-hooks.ts_docs.md)
- [login-hooks.ts](login-hooks.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
