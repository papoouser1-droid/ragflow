# File Documentation: web/src/layouts/components/header/index.tsx

## File Metadata

- **Path**: `web/src/layouts/components/header/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 117
- **Characters**: 3,688
- **Size**: 3,688 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ReactComponent as FileIcon } from '@/assets/svg/file-management.svg';
import { ReactComponent as GraphIcon } from '@/assets/svg/graph.svg';
import { ReactComponent as KnowledgeBaseIcon } from '@/assets/svg/knowledge-base.svg';
import { useTranslate } from '@/hooks/common-hooks';
import { useFetchAppConf } from '@/hooks/logic-hooks';
import { useNavigateWithFromState } from '@/hooks/route-hook';
import { MessageOutlined, SearchOutlined } from '@ant-design/icons';
import { Flex, Layout, Radio, Space, theme } from 'antd';
import { MouseEventHandler, useCallback, useMemo } from 'react';
import { useLocation } from 'umi';
import Toolbar from '../right-toolbar';

import { useTheme } from '@/components/theme-provider';
import styles from './index.less';

const { Header } = Layout;

const RagHeader = () => {
  const {
    token: { colorBgContainer },
  } = theme.useToken();
  const navigate = useNavigateWithFromState();
  const { pathname } = useLocation();
  const { t } = useTranslate('header');
  const appConf = useFetchAppConf();
  const { theme: themeRag } = useTheme();
  const tagsData = useMemo(
    () => [
      { path: '/knowledge', name: t('knowledgeBase'), icon: KnowledgeBaseIcon },
      { path: '/chat', name: t('chat'), icon: MessageOutlined },
      { path: '/search', name: t('search'), icon: SearchOutlined },
      { path: '/agent-list', name: t('flow'), icon: GraphIcon },
      { path: '/file', name: t('fileManager'), icon: FileIcon },
    ],
    [t],
  );

  const currentPath = useMemo(() => {
    return (
      tagsData.find((x) => pathname.startsWith(x.path))?.name || 'knowledge'
    );
  }, [pathname, tagsData]);

  const handleChange = useCallback(
    (path: string): MouseEventHandler =>
      (e) => {
        e.preventDefault();
        navigate(path);
      },
    [navigate],
  );

  const handleLogoClick = useCallback(() => {
    navigate('/');
  }, [navigate]);

  return (
    <Header
      style={{
        padding: '0 16px',
        background: colorBgContainer,
        display: 'flex',
        justifyContent: 'space-between',
        alignItems: 'center',
        height: '72px',
      }}
    >
      <a href={window.location.origin}>
        <Space
          size={12}
          onClick={handleLogoClick}
          className={styles.logoWrapper}
        >
          <img src="/logo.svg" alt="" className={styles.appIcon} />
          <span className={styles.appName}>{appConf.appName}</span>
        </Space>
      </a>
      <Space size={[0, 8]} wrap>
        <Radio.Group
          defaultValue="a"
          buttonStyle="solid"
          className={
            themeRag === 'dark' ? styles.radioGroupDark : styles.radioGroup
          }
          value={currentPath}
        >
          {tagsData.map((item, index) => (
            <Radio.Button
              className={`${themeRag === 'dark' ? 'dark' : 'light'} ${index === 0 ? 'first' : ''} ${index === tagsData.length - 1 ? 'last' : ''}`}
              value={item.name}
              key={item.name}
            >
              <a href={item.path}>
                <Flex
                  align="center"
                  gap={8}
                  onClick={handleChange(item.path)}
                  className="cursor-pointer"
                >
                  <item.icon
                    className={styles.radioButtonIcon}
                    stroke={item.name === currentPath ? 'black' : 'white'}
                  ></item.icon>
                  {item.name}
                </Flex>
              </a>
            </Radio.Button>
          ))}
        </Radio.Group>
      </Space>
      <Toolbar></Toolbar>
    </Header>
  );
};

export default RagHeader;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/layouts/components/header/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 117 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `RagHeader()`: Function definition
- `tagsData()`: Function definition
- `currentPath()`: Function definition
- `handleChange()`: Function definition
- `handleLogoClick()`: Function definition

### Imports (13)

- `import { ReactComponent as FileIcon } from '@/assets/svg/file-management.svg';`
- `import { ReactComponent as GraphIcon } from '@/assets/svg/graph.svg';`
- `import { ReactComponent as KnowledgeBaseIcon } from '@/assets/svg/knowledge-base.svg';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useFetchAppConf } from '@/hooks/logic-hooks';`
- `import { useNavigateWithFromState } from '@/hooks/route-hook';`
- `import { MessageOutlined, SearchOutlined } from '@ant-design/icons';`
- `import { Flex, Layout, Radio, Space, theme } from 'antd';`
- `import { MouseEventHandler, useCallback, useMemo } from 'react';`
- `import { useLocation } from 'umi';`

## Code Structure Analysis

- Total lines: 117
- Blank lines: 9 (7.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~108


## Dependencies and Imports

- `@/assets/svg/file-management.svg`
- `@/assets/svg/graph.svg`
- `@/assets/svg/knowledge-base.svg`
- `@/hooks/common-hooks`
- `@/hooks/logic-hooks`
- `@/hooks/route-hook`
- `@ant-design/icons`
- `antd`
- `react`
- `umi`
- `../right-toolbar`
- `@/components/theme-provider`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/layouts/components/header`.

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

- Other files in `web/src/layouts/components/header/` directory
- Potential test file: `test_index.tsx`

## Keywords

../right-toolbar, ./index.less, @/assets/svg/file-management.svg, @/assets/svg/graph.svg, @/assets/svg/knowledge-base.svg, @/components/theme-provider, @/hooks/common-hooks, @/hooks/logic-hooks, @/hooks/route-hook, @ant-design/icons, Button, FileIcon, Flex, GraphIcon, Group, Header, KnowledgeBaseIcon, Layout, MessageOutlined, MouseEventHandler, Radio, RagHeader, ReactComponent, SearchOutlined, Space, Toolbar, TypeScript, ant, antd, appConf, currentPath, handleChange, handleLogoClick, navigate, react, tagsData, umi

---
*Generated by RAGFlow Repository Documentation Generator*
