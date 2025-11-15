# File Documentation: web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 136
- **Characters**: 3,781
- **Size**: 3,781 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ReactComponent as ConfigurationIcon } from '@/assets/svg/knowledge-configration.svg';
import { ReactComponent as DatasetIcon } from '@/assets/svg/knowledge-dataset.svg';
import { ReactComponent as TestingIcon } from '@/assets/svg/knowledge-testing.svg';
import {
  useFetchKnowledgeBaseConfiguration,
  useFetchKnowledgeGraph,
} from '@/hooks/knowledge-hooks';
import {
  useGetKnowledgeSearchParams,
  useSecondPathName,
} from '@/hooks/route-hook';
import { getWidth } from '@/utils';
import { Avatar, Menu, MenuProps, Space } from 'antd';
import classNames from 'classnames';
import { useCallback, useEffect, useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import { useNavigate } from 'umi';
import { KnowledgeRouteKey } from '../../constant';

import { isEmpty } from 'lodash';
import { GitGraph } from 'lucide-react';
import styles from './index.less';

const KnowledgeSidebar = () => {
  let navigate = useNavigate();
  const activeKey = useSecondPathName();
  const { knowledgeId } = useGetKnowledgeSearchParams();

  const [windowWidth, setWindowWidth] = useState(getWidth());
  const { t } = useTranslation();
  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration();

  const handleSelect: MenuProps['onSelect'] = (e) => {
    navigate(`/knowledge/${e.key}?id=${knowledgeId}`);
  };

  const { data } = useFetchKnowledgeGraph();

  type MenuItem = Required<MenuProps>['items'][number];

  const getItem = useCallback(
    (
      label: string,
      key: React.Key,
      icon?: React.ReactNode,
      disabled?: boolean,
      children?: MenuItem[],
      type?: 'group',
    ): MenuItem => {
      return {
        key,
        icon,
        children,
        label: t(`knowledgeDetails.${label}`),
        type,
        disabled,
      } as MenuItem;
    },
    [t],
  );

  const items: MenuItem[] = useMemo(() => {
    const list = [
      getItem(
        KnowledgeRouteKey.Dataset, // TODO: Change icon color when selected
        KnowledgeRouteKey.Dataset,
        <DatasetIcon />,
      ),
      getItem(
        KnowledgeRouteKey.Testing,
        KnowledgeRouteKey.Testing,
        <TestingIcon />,
      ),
      getItem(
        KnowledgeRouteKey.Configuration,
        KnowledgeRouteKey.Configuration,
        <ConfigurationIcon />,
      ),
    ];

    if (!isEmpty(data?.graph)) {
      list.push(
        getItem(
          KnowledgeRouteKey.KnowledgeGraph,
          KnowledgeRouteKey.KnowledgeGraph,
          <GitGraph />,
        ),
      );
    }

    return list;
  }, [data, getItem]);

  useEffect(() => {
    const widthSize = () => {
      const width = getWidth();

      setWindowWidth(width);
    };
    window.addEventListener('resize', widthSize);
    return () => {
      window.removeEventListener('resize', widthSize);
    };
  }, []);

  return (
    <div className={styles.sidebarWrapper}>
      <div className={styles.sidebarTop}>
        <Space size={8} direction="vertical">
          <Avatar size={64} src={knowledgeDetails.avatar} />
          <div className={styles.knowledgeTitle}>{knowledgeDetails.name}</div>
        </Space>
        <p className={styles.knowledgeDescription}>
          {knowledgeDetails.description}
        </p>
      </div>
      <div className={styles.divider}></div>
      <div className={styles.menuWrapper}>
        <Menu
          selectedKeys={[activeKey]}
          // mode="inline"
          className={classNames(styles.menu, {
            [styles.defaultWidth]: windowWidth.width > 957,
            [styles.minWidth]: windowWidth.width <= 957,
          })}
          // inlineCollapsed={collapsed}
          items={items}
          onSelect={handleSelect}
        />
      </div>
    </div>
  );
};

export default KnowledgeSidebar;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 136 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `KnowledgeSidebar()`: Function definition
- `getItem()`: Function definition
- `list()`: Function definition
- `widthSize()`: Function definition
- `width()`: Function definition

### Imports (15)

- `import { ReactComponent as ConfigurationIcon } from '@/assets/svg/knowledge-configration.svg';`
- `import { ReactComponent as DatasetIcon } from '@/assets/svg/knowledge-dataset.svg';`
- `import { ReactComponent as TestingIcon } from '@/assets/svg/knowledge-testing.svg';`
- `import {`
- `import {`
- `import { getWidth } from '@/utils';`
- `import { Avatar, Menu, MenuProps, Space } from 'antd';`
- `import classNames from 'classnames';`
- `import { useCallback, useEffect, useMemo, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 136
- Blank lines: 15 (11.0%)
- Comment lines: ~2 (1.5%)
- Code lines: ~119


## Dependencies and Imports

- `@/assets/svg/knowledge-configration.svg`
- `@/assets/svg/knowledge-dataset.svg`
- `@/assets/svg/knowledge-testing.svg`
- `@/utils`
- `antd`
- `classnames`
- `react`
- `react-i18next`
- `umi`
- `../../constant`
- `lodash`
- `lucide-react`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-sidebar`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-sidebar/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ./index.less, @/assets/svg/knowledge-configration.svg, @/assets/svg/knowledge-dataset.svg, @/assets/svg/knowledge-testing.svg, @/utils, Avatar, Change, Configuration, ConfigurationIcon, Dataset, DatasetIcon, GitGraph, Key, KnowledgeGraph, KnowledgeRouteKey, KnowledgeSidebar, Menu, MenuItem, MenuProps, React, ReactComponent, ReactNode, Required, Space, TODO, Testing, TestingIcon, TypeScript, activeKey, antd, classnames, getItem, handleSelect, items, list, lodash, lucide-react, navigate, react, react-i18next, umi, width, widthSize

---
*Generated by RAGFlow Repository Documentation Generator*
