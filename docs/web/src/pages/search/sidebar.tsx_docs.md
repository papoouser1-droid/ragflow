# Documentation: web/src/pages/search/sidebar.tsx

## File Metadata

- **Path**: `web/src/pages/search/sidebar.tsx`
- **Size**: 4554 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/search/sidebar.tsx`.

## Original Source Code

```tsx
import { useFetchKnowledgeList } from '@/hooks/knowledge-hooks';
import { UserOutlined } from '@ant-design/icons';
import type { TreeDataNode, TreeProps } from 'antd';
import { Avatar, Layout, Space, Spin, Tree, Typography } from 'antd';
import classNames from 'classnames';
import {
  Dispatch,
  SetStateAction,
  useCallback,
  useEffect,
  useMemo,
  useState,
} from 'react';

import styles from './index.less';

const { Sider } = Layout;

interface IProps {
  isFirstRender: boolean;
  checkedList: string[];
  setCheckedList: Dispatch<SetStateAction<string[]>>;
}

const SearchSidebar = ({
  isFirstRender,
  checkedList,
  setCheckedList,
}: IProps) => {
  const { list, loading } = useFetchKnowledgeList();

  const groupedList = useMemo(() => {
    return list.reduce((pre: TreeDataNode[], cur) => {
      const parentItem = pre.find((x) => x.key === cur.embd_id);
      const childItem: TreeDataNode = {
        title: cur.name,
        key: cur.id,
        isLeaf: true,
      };
      if (parentItem) {
        parentItem.children?.push(childItem);
      } else {
        pre.push({
          title: cur.embd_id,
          key: cur.embd_id,
          isLeaf: false,
          children: [childItem],
        });
      }

      return pre;
    }, []);
  }, [list]);

  const [expandedKeys, setExpandedKeys] = useState<React.Key[]>([]);
  const [selectedKeys, setSelectedKeys] = useState<React.Key[]>([]);
  const [autoExpandParent, setAutoExpandParent] = useState<boolean>(true);

  const onExpand: TreeProps['onExpand'] = (expandedKeysValue) => {
    // if not set autoExpandParent to false, if children expanded, parent can not collapse.
    // or, you can remove all expanded children keys.
    setExpandedKeys(expandedKeysValue);
    setAutoExpandParent(false);
  };

  const onCheck: TreeProps['onCheck'] = (checkedKeysValue, info) => {
    console.log('onCheck', checkedKeysValue, info);
    const currentCheckedKeysValue = checkedKeysValue as string[];

    let nextSelectedKeysValue: string[] = [];
    const { isLeaf, checked, key, children } = info.node;
    if (isLeaf) {
      const item = list.find((x) => x.id === key);
      if (!checked) {
        const embeddingIds = currentCheckedKeysValue
          .filter((x) => list.some((y) => y.id === x))
          .map((x) => list.find((y) => y.id === x)?.embd_id);

        if (embeddingIds.some((x) => x !== item?.embd_id)) {
          nextSelectedKeysValue = [key as string];
        } else {
          nextSelectedKeysValue = currentCheckedKeysValue;
        }
      } else {
        nextSelectedKeysValue = currentCheckedKeysValue;
      }
    } else {
      if (!checked) {
        nextSelectedKeysValue = [
          key as string,
          ...(children?.map((x) => x.key as string) ?? []),
        ];
      } else {
        nextSelectedKeysValue = [];
      }
    }

    setCheckedList(nextSelectedKeysValue);
  };

  const onSelect: TreeProps['onSelect'] = (selectedKeysValue, info) => {
    console.log('onSelect', info);

    setSelectedKeys(selectedKeysValue);
  };

  const renderTitle = useCallback(
    (node: TreeDataNode) => {
      const item = list.find((x) => x.id === node.key);
      return (
        <Space>
          {node.isLeaf && (
            <Avatar size={24} icon={<UserOutlined />} src={item?.avatar} />
          )}
          <Typography.Text
            ellipsis={{ tooltip: node.title as string }}
            className={node.isLeaf ? styles.knowledgeName : styles.embeddingId}
          >
            {node.title as string}
          </Typography.Text>
        </Space>
      );
    },
    [list],
  );

  useEffect(() => {
    const firstGroup = groupedList[0]?.children?.map((x) => x.key as string);
    if (firstGroup) {
      setCheckedList(firstGroup);
    }
    setExpandedKeys(groupedList.map((x) => x.key));
  }, [groupedList, setExpandedKeys, setCheckedList]);

  return (
    <Sider
      className={classNames(styles.searchSide, {
        [styles.transparentSearchSide]: isFirstRender,
      })}
      theme={'light'}
      width={'20%'}
    >
      <Spin spinning={loading}>
        <Tree
          className={styles.list}
          checkable
          onExpand={onExpand}
          expandedKeys={expandedKeys}
          autoExpandParent={autoExpandParent}
          onCheck={onCheck}
          checkedKeys={checkedList}
          onSelect={onSelect}
          selectedKeys={selectedKeys}
          treeData={groupedList}
          titleRender={renderTitle}
        />
      </Spin>
    </Sider>
  );
};

export default SearchSidebar;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/search/sidebar.tsx` is located in the `web/src/pages/search` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to search.

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

- [hooks.ts](hooks.ts_docs.md)
- [index.less](index.less_docs.md)
- [index.tsx](index.tsx_docs.md)
- [mindmap-drawer.tsx](mindmap-drawer.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
