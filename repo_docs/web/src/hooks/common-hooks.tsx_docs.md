# File Documentation: web/src/hooks/common-hooks.tsx

## File Metadata

- **Path**: `web/src/hooks/common-hooks.tsx`
- **Extension**: `.tsx`
- **Lines**: 128
- **Characters**: 3,342
- **Size**: 3,342 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/hooks/common-hooks.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 128 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (6)

- `useSetModalState`: Exported entity
- `useDeepCompareEffect`: Exported entity
- `useDynamicSVGImport`: Exported entity
- `useShowDeleteConfirm`: Exported entity
- `useTranslate`: Exported entity
- `useCommonTranslation`: Exported entity

### Functions (11)

- `useSetModalState()`: Function definition
- `showModal()`: Function definition
- `hideModal()`: Function definition
- `switchVisible()`: Function definition
- `useDeepCompareEffect()`: Function definition
- `useDynamicSVGImport()`: Function definition
- `importIcon()`: Function definition
- `useShowDeleteConfirm()`: Function definition
- `showDeleteConfirm()`: Function definition
- `useTranslate()`: Function definition
- `useCommonTranslation()`: Function definition

### Imports (5)

- `import { ExclamationCircleFilled } from '@ant-design/icons';`
- `import { App } from 'antd';`
- `import isEqual from 'lodash/isEqual';`
- `import { ReactNode, useCallback, useEffect, useRef, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 16 (12.5%)
- Comment lines: ~0 (0.0%)
- Code lines: ~112


## Dependencies and Imports

- `@ant-design/icons`
- `antd`
- `lodash/isEqual`
- `react`
- `react-i18next`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/hooks`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/hooks/` directory
- Potential test file: `test_common-hooks.tsx`

## Keywords

@ant-design/icons, App, DependencyList, EffectCallback, Error, ExclamationCircleFilled, IProps, ImportedIconRef, Promise, React, ReactComponent, ReactNode, ReturnType, SVGProps, SVGSVGElement, SvgIcon, TypeScript, UseDynamicSVGImportOptions, ant, antd, callback, hideModal, importIcon, lodash/isEqual, react, react-i18next, ref, ret, showDeleteConfirm, showModal, switchVisible, useCommonTranslation, useDeepCompareEffect, useDynamicSVGImport, useSetModalState, useShowDeleteConfirm, useTranslate

---
*Generated by RAGFlow Repository Documentation Generator*
