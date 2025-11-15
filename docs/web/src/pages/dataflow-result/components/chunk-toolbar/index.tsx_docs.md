# Documentation: web/src/pages/dataflow-result/components/chunk-toolbar/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/chunk-toolbar/index.tsx`
- **Size**: 5495 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataflow-result/components/chunk-toolbar/index.tsx`.

## Original Source Code

```tsx
import { ReactComponent as FilterIcon } from '@/assets/filter.svg';
import { KnowledgeRouteKey } from '@/constants/knowledge';
import { IChunkListResult, useSelectChunkList } from '@/hooks/chunk-hooks';
import { useTranslate } from '@/hooks/common-hooks';
import { useKnowledgeBaseId } from '@/hooks/knowledge-hooks';
import {
  ArrowLeftOutlined,
  CheckCircleOutlined,
  CloseCircleOutlined,
  DeleteOutlined,
  DownOutlined,
  FilePdfOutlined,
  PlusOutlined,
  SearchOutlined,
} from '@ant-design/icons';
import {
  Button,
  Checkbox,
  Flex,
  Input,
  Menu,
  MenuProps,
  Popover,
  Radio,
  RadioChangeEvent,
  Segmented,
  SegmentedProps,
  Space,
  Typography,
} from 'antd';
import { useCallback, useMemo, useState } from 'react';
import { Link } from 'umi';
import { ChunkTextMode } from '../../constant';

const { Text } = Typography;

interface IProps
  extends Pick<
    IChunkListResult,
    'searchString' | 'handleInputChange' | 'available' | 'handleSetAvailable'
  > {
  checked: boolean;
  selectAllChunk: (checked: boolean) => void;
  createChunk: () => void;
  removeChunk: () => void;
  switchChunk: (available: number) => void;
  changeChunkTextMode(mode: ChunkTextMode): void;
}

const ChunkToolBar = ({
  selectAllChunk,
  checked,
  createChunk,
  removeChunk,
  switchChunk,
  changeChunkTextMode,
  available,
  handleSetAvailable,
  searchString,
  handleInputChange,
}: IProps) => {
  const data = useSelectChunkList();
  const documentInfo = data?.documentInfo;
  const knowledgeBaseId = useKnowledgeBaseId();
  const [isShowSearchBox, setIsShowSearchBox] = useState(false);
  const { t } = useTranslate('chunk');

  const handleSelectAllCheck = useCallback(
    (e: any) => {
      selectAllChunk(e.target.checked);
    },
    [selectAllChunk],
  );

  const handleSearchIconClick = () => {
    setIsShowSearchBox(true);
  };

  const handleSearchBlur = () => {
    if (!searchString?.trim()) {
      setIsShowSearchBox(false);
    }
  };

  const handleDelete = useCallback(() => {
    removeChunk();
  }, [removeChunk]);

  const handleEnabledClick = useCallback(() => {
    switchChunk(1);
  }, [switchChunk]);

  const handleDisabledClick = useCallback(() => {
    switchChunk(0);
  }, [switchChunk]);

  const items: MenuProps['items'] = useMemo(() => {
    return [
      {
        key: '1',
        label: (
          <>
            <Checkbox onChange={handleSelectAllCheck} checked={checked}>
              <b>{t('selectAll')}</b>
            </Checkbox>
          </>
        ),
      },
      { type: 'divider' },
      {
        key: '2',
        label: (
          <Space onClick={handleEnabledClick}>
            <CheckCircleOutlined />
            <b>{t('enabledSelected')}</b>
          </Space>
        ),
      },
      {
        key: '3',
        label: (
          <Space onClick={handleDisabledClick}>
            <CloseCircleOutlined />
            <b>{t('disabledSelected')}</b>
          </Space>
        ),
      },
      { type: 'divider' },
      {
        key: '4',
        label: (
          <Space onClick={handleDelete}>
            <DeleteOutlined />
            <b>{t('deleteSelected')}</b>
          </Space>
        ),
      },
    ];
  }, [
    checked,
    handleSelectAllCheck,
    handleDelete,
    handleEnabledClick,
    handleDisabledClick,
    t,
  ]);

  const content = (
    <Menu style={{ width: 200 }} items={items} selectable={false} />
  );

  const handleFilterChange = (e: RadioChangeEvent) => {
    selectAllChunk(false);
    handleSetAvailable(e.target.value);
  };

  const filterContent = (
    <Radio.Group onChange={handleFilterChange} value={available}>
      <Space direction="vertical">
        <Radio value={undefined}>{t('all')}</Radio>
        <Radio value={1}>{t('enabled')}</Radio>
        <Radio value={0}>{t('disabled')}</Radio>
      </Space>
    </Radio.Group>
  );

  return (
    <Flex justify="space-between" align="center">
      <Space size={'middle'}>
        <Link
          to={`/knowledge/${KnowledgeRouteKey.Dataset}?id=${knowledgeBaseId}`}
        >
          <ArrowLeftOutlined />
        </Link>
        <FilePdfOutlined />
        <Text ellipsis={{ tooltip: documentInfo?.name }} style={{ width: 150 }}>
          {documentInfo?.name}
        </Text>
      </Space>
      <Space>
        <Segmented
          options={[
            { label: t(ChunkTextMode.Full), value: ChunkTextMode.Full },
            { label: t(ChunkTextMode.Ellipse), value: ChunkTextMode.Ellipse },
          ]}
          onChange={changeChunkTextMode as SegmentedProps['onChange']}
        />
        <Popover content={content} placement="bottom" arrow={false}>
          <Button>
            {t('bulk')}
            <DownOutlined />
          </Button>
        </Popover>
        {isShowSearchBox ? (
          <Input
            size="middle"
            placeholder={t('search')}
            prefix={<SearchOutlined />}
            allowClear
            onChange={handleInputChange}
            onBlur={handleSearchBlur}
            value={searchString}
          />
        ) : (
          <Button icon={<SearchOutlined />} onClick={handleSearchIconClick} />
        )}

        <Popover content={filterContent} placement="bottom" arrow={false}>
          <Button icon={<FilterIcon />} />
        </Popover>
        <Button
          icon={<PlusOutlined />}
          type="primary"
          onClick={() => createChunk()}
        />
      </Space>
    </Flex>
  );
};

export default ChunkToolBar;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataflow-result/components/chunk-toolbar/index.tsx` is located in the `web/src/pages/dataflow-result/components/chunk-toolbar` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-toolbar.

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



## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
