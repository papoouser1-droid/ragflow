# Documentation: web/src/components/svg-icon.tsx

## File Metadata

- **Path**: `web/src/components/svg-icon.tsx`
- **Size**: 3402 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/svg-icon.tsx`.

## Original Source Code

```tsx
import { IconMap, LLMFactory } from '@/constants/llm';
import { cn } from '@/lib/utils';
import Icon, { UserOutlined } from '@ant-design/icons';
import { IconComponentProps } from '@ant-design/icons/lib/components/Icon';
import { Avatar } from 'antd';
import { AvatarSize } from 'antd/es/avatar/AvatarContext';
import { useMemo } from 'react';
import { IconFontFill } from './icon-font';
import { useIsDarkTheme } from './theme-provider';

const importAll = (requireContext: __WebpackModuleApi.RequireContext) => {
  const list = requireContext.keys().map((key) => {
    const name = key.replace(/\.\/(.*)\.\w+$/, '$1');
    return { name, value: requireContext(key) };
  });
  return list;
};

let routeList: { name: string; value: string }[] = [];

try {
  routeList = importAll(require.context('@/assets/svg', true, /\.svg$/));
} catch (error) {
  console.warn(error);
  routeList = [];
}

interface IProps extends IconComponentProps {
  name: string;
  width: string | number;
  height?: string | number;
  imgClass?: string;
}

const SvgIcon = ({ name, width, height, imgClass, ...restProps }: IProps) => {
  const ListItem = routeList.find((item) => item.name === name);
  return (
    <Icon
      component={() => (
        <img
          src={ListItem?.value}
          alt=""
          width={width}
          height={height}
          className={cn(imgClass, 'max-w-full')}
        />
      )}
      {...(restProps as any)}
    />
  );
};

export const LlmIcon = ({
  name,
  height = 48,
  width = 48,
  size = 'large',
  imgClass,
}: {
  name: string;
  height?: number;
  width?: number;
  size?: AvatarSize;
  imgClass?: string;
}) => {
  const isDark = useIsDarkTheme();
  const themeIcons = [
    LLMFactory.FishAudio,
    LLMFactory.TogetherAI,
    LLMFactory.Meituan,
    LLMFactory.Longcat,
  ];
  let icon = useMemo(() => {
    const icontemp = IconMap[name as keyof typeof IconMap];
    if (themeIcons.includes(name as LLMFactory)) {
      if (isDark) {
        return icontemp + '-dark';
      } else {
        return icontemp + '-bright';
      }
    }
    return icontemp;
  }, [name, isDark]);

  const svgIcons = [
    LLMFactory.LocalAI,
    // LLMFactory.VolcEngine,
    // LLMFactory.MiniMax,
    LLMFactory.Gemini,
    LLMFactory.StepFun,
    // LLMFactory.DeerAPI,
  ];
  if (svgIcons.includes(name as LLMFactory)) {
    return (
      <SvgIcon
        name={`llm/${icon}`}
        width={width}
        height={height}
        imgClass={imgClass}
      ></SvgIcon>
    );
  }

  return icon ? (
    <IconFontFill
      name={icon}
      className={cn('size-8 flex items-center justify-center', imgClass)}
    />
  ) : (
    <IconFontFill
      name={'moxing-default'}
      className={cn('size-8 flex items-center justify-center', imgClass)}
    />
    // <Avatar shape="square" size={size} icon={<UserOutlined />} />
  );
};

export const HomeIcon = ({
  name,
  height = '32',
  width = '32',
  size = 'large',
  imgClass,
}: {
  name: string;
  height?: string;
  width?: string;
  size?: AvatarSize;
  imgClass?: string;
}) => {
  const isDark = useIsDarkTheme();
  const icon = isDark ? name : `${name}-bri`;

  return icon ? (
    <SvgIcon
      name={`home-icon/${icon}`}
      width={width}
      height={height}
      imgClass={imgClass}
    ></SvgIcon>
  ) : (
    <Avatar shape="square" size={size} icon={<UserOutlined />} />
  );
};

export default SvgIcon;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/svg-icon.tsx` is located in the `web/src/components` directory.

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
