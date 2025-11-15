# File Documentation: web/src/pages/knowledge/knowledge-card/index.tsx

## File Metadata

- **Path**: `web/src/pages/knowledge/knowledge-card/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 113
- **Characters**: 3,783
- **Size**: 3,783 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { KnowledgeRouteKey } from '@/constants/knowledge';
import { IKnowledge } from '@/interfaces/database/knowledge';
import { formatDate } from '@/utils/date';
import {
  CalendarOutlined,
  FileTextOutlined,
  UserOutlined,
} from '@ant-design/icons';
import { Avatar, Badge, Card, Space } from 'antd';
import classNames from 'classnames';
import { useTranslation } from 'react-i18next';
import { useNavigate } from 'umi';

import OperateDropdown from '@/components/operate-dropdown';
import { useTheme } from '@/components/theme-provider';
import { useDeleteKnowledge } from '@/hooks/knowledge-hooks';
import { useFetchUserInfo } from '@/hooks/user-setting-hooks';
import styles from './index.less';

interface IProps {
  item: IKnowledge;
}

const KnowledgeCard = ({ item }: IProps) => {
  const navigate = useNavigate();
  const { t } = useTranslation();
  const { data: userInfo } = useFetchUserInfo();
  const { theme } = useTheme();
  const { deleteKnowledge } = useDeleteKnowledge();

  const removeKnowledge = async () => {
    return deleteKnowledge(item.id);
  };

  const handleCardClick = () => {
    navigate(`/knowledge/${KnowledgeRouteKey.Dataset}?id=${item.id}`, {
      state: { from: 'list' },
    });
  };

  return (
    <Badge.Ribbon
      text={item?.nickname}
      color={userInfo?.nickname === item?.nickname ? '#1677ff' : 'pink'}
      className={classNames(styles.ribbon, {
        [styles.hideRibbon]: item.permission !== 'team',
      })}
    >
      <Card className={styles.card} onClick={handleCardClick}>
        <div className={styles.container}>
          <div className={styles.content}>
            <Avatar size={34} icon={<UserOutlined />} src={item.avatar} />
            <OperateDropdown deleteItem={removeKnowledge}></OperateDropdown>
          </div>
          <div className={styles.titleWrapper}>
            <span
              className={theme === 'dark' ? styles.titledark : styles.title}
            >
              {item.name}
            </span>
            <p
              className={
                theme === 'dark' ? styles.descriptiondark : styles.description
              }
            >
              {item.description}
            </p>
          </div>
          <div className={styles.footer}>
            <div className={styles.footerTop}>
              <div className={styles.bottomLeft}>
                <FileTextOutlined className={styles.leftIcon} />
                <span className={styles.rightText}>
                  <Space>
                    {item.doc_num}
                    {t('knowledgeList.doc')}
                  </Space>
                </span>
              </div>
            </div>
            <div className={styles.bottom}>
              <div className={styles.bottomLeft}>
                <CalendarOutlined className={styles.leftIcon} />
                <span className={styles.rightText}>
                  {formatDate(item.update_time)}
                </span>
              </div>
              {/* <Avatar.Group size={25}>
                <Avatar src="https://api.dicebear.com/7.x/miniavs/svg?seed=1" />
                <a href="https://ant.design">
                  <Avatar style={{ backgroundColor: '#f56a00' }}>K</Avatar>
                </a>
                <Tooltip title="Ant User" placement="top">
                  <Avatar
                    style={{ backgroundColor: '#87d068' }}
                    icon={<UserOutlined />}
                  />
                </Tooltip>
                <Avatar
                  style={{ backgroundColor: '#1677ff' }}
                  icon={<AntDesignOutlined />}
                />
              </Avatar.Group> */}
            </div>
          </div>
        </div>
      </Card>
    </Badge.Ribbon>
  );
};

export default KnowledgeCard;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/knowledge/knowledge-card/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 113 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (3)

- `KnowledgeCard()`: Function definition
- `removeKnowledge()`: Function definition
- `handleCardClick()`: Function definition

### Imports (13)

- `import { KnowledgeRouteKey } from '@/constants/knowledge';`
- `import { IKnowledge } from '@/interfaces/database/knowledge';`
- `import { formatDate } from '@/utils/date';`
- `import {`
- `import { Avatar, Badge, Card, Space } from 'antd';`
- `import classNames from 'classnames';`
- `import { useTranslation } from 'react-i18next';`
- `import { useNavigate } from 'umi';`
- `import OperateDropdown from '@/components/operate-dropdown';`
- `import { useTheme } from '@/components/theme-provider';`

## Code Structure Analysis

- Total lines: 113
- Blank lines: 8 (7.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~105


## Dependencies and Imports

- `@/constants/knowledge`
- `@/interfaces/database/knowledge`
- `@/utils/date`
- `antd`
- `classnames`
- `react-i18next`
- `umi`
- `@/components/operate-dropdown`
- `@/components/theme-provider`
- `@/hooks/knowledge-hooks`
- `@/hooks/user-setting-hooks`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/knowledge/knowledge-card`.

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

- Other files in `web/src/pages/knowledge/knowledge-card/` directory
- Potential test file: `test_index.tsx`

## Keywords

./index.less, @/components/operate-dropdown, @/components/theme-provider, @/constants/knowledge, @/hooks/knowledge-hooks, @/hooks/user-setting-hooks, @/interfaces/database/knowledge, @/utils/date, Ant, AntDesignOutlined, Avatar, Badge, CalendarOutlined, Card, Dataset, FileTextOutlined, Group, IKnowledge, IProps, KnowledgeCard, KnowledgeRouteKey, OperateDropdown, Ribbon, Space, Tooltip, TypeScript, User, UserOutlined, ant, antd, classnames, handleCardClick, navigate, react-i18next, removeKnowledge, umi

---
*Generated by RAGFlow Repository Documentation Generator*
