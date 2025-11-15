# Documentation: web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx`
- **Size**: 3781 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/add-knowledge/components/knowledge-sidebar/index.tsx` is located in the `web/src/pages/add-knowledge/components/knowledge-sidebar` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge-sidebar.

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

- [index.less](index.less_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
