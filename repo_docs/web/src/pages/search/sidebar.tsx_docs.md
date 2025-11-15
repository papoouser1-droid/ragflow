# File Documentation: web/src/pages/search/sidebar.tsx

## File Metadata

- **Path**: `web/src/pages/search/sidebar.tsx`
- **Extension**: `.tsx`
- **Lines**: 163
- **Characters**: 4,554
- **Size**: 4,554 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/search/sidebar.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 163 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (8)

- `SearchSidebar()`: Function definition
- `groupedList()`: Function definition
- `parentItem()`: Function definition
- `item()`: Function definition
- `embeddingIds()`: Function definition
- `renderTitle()`: Function definition
- `item()`: Function definition
- `firstGroup()`: Function definition

### Imports (7)

- `import { useFetchKnowledgeList } from '@/hooks/knowledge-hooks';`
- `import { UserOutlined } from '@ant-design/icons';`
- `import type { TreeDataNode, TreeProps } from 'antd';`
- `import { Avatar, Layout, Space, Spin, Tree, Typography } from 'antd';`
- `import classNames from 'classnames';`
- `import {`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 163
- Blank lines: 19 (11.7%)
- Comment lines: ~2 (1.2%)
- Code lines: ~142


## Dependencies and Imports

- `@/hooks/knowledge-hooks`
- `@ant-design/icons`
- `antd`
- `antd`
- `classnames`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/search`.

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

- Other files in `web/src/pages/search/` directory
- Potential test file: `test_sidebar.tsx`

## Keywords

./index.less, @/hooks/knowledge-hooks, @ant-design/icons, Avatar, Dispatch, IProps, Key, Layout, React, SearchSidebar, SetStateAction, Sider, Space, Spin, Text, Tree, TreeDataNode, TreeProps, TypeScript, Typography, UserOutlined, ant, antd, childItem, classnames, currentCheckedKeysValue, embeddingIds, firstGroup, groupedList, item, nextSelectedKeysValue, onCheck, onExpand, onSelect, parentItem, renderTitle

---
*Generated by RAGFlow Repository Documentation Generator*
