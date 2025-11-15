# Documentation: web/src/pages/knowledge/knowledge-card/index.tsx

## File Metadata

- **Path**: `web/src/pages/knowledge/knowledge-card/index.tsx`
- **Size**: 3783 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/knowledge/knowledge-card/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/knowledge/knowledge-card/index.tsx` is located in the `web/src/pages/knowledge/knowledge-card` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to knowledge-card.

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
