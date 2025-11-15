# File Documentation: web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-toolbar/index.tsx

## File Metadata

- **Path**: `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-toolbar/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 222
- **Characters**: 5,495
- **Size**: 5,495 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-toolbar/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 222 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (8)

- `ChunkToolBar()`: Function definition
- `handleSelectAllCheck()`: Function definition
- `handleSearchIconClick()`: Function definition
- `handleSearchBlur()`: Function definition
- `handleDelete()`: Function definition
- `handleEnabledClick()`: Function definition
- `handleDisabledClick()`: Function definition
- `handleFilterChange()`: Function definition

### Imports (10)

- `import { ReactComponent as FilterIcon } from '@/assets/filter.svg';`
- `import { KnowledgeRouteKey } from '@/constants/knowledge';`
- `import { IChunkListResult, useSelectChunkList } from '@/hooks/chunk-hooks';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { useKnowledgeBaseId } from '@/hooks/knowledge-hooks';`
- `import {`
- `import {`
- `import { useCallback, useMemo, useState } from 'react';`
- `import { Link } from 'umi';`
- `import { ChunkTextMode } from '../../constant';`

## Code Structure Analysis

- Total lines: 222
- Blank lines: 17 (7.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~205


## Dependencies and Imports

- `@/assets/filter.svg`
- `@/constants/knowledge`
- `@/hooks/chunk-hooks`
- `@/hooks/common-hooks`
- `@/hooks/knowledge-hooks`
- `react`
- `umi`
- `../../constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-toolbar`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-toolbar/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, @/assets/filter.svg, @/constants/knowledge, @/hooks/chunk-hooks, @/hooks/common-hooks, @/hooks/knowledge-hooks, ArrowLeftOutlined, Button, CheckCircleOutlined, Checkbox, ChunkTextMode, ChunkToolBar, CloseCircleOutlined, Dataset, DeleteOutlined, DownOutlined, Ellipse, FilePdfOutlined, FilterIcon, Flex, Full, Group, IChunkListResult, IProps, Input, KnowledgeRouteKey, Link, Menu, MenuProps, Pick, PlusOutlined, Popover, Radio, RadioChangeEvent, ReactComponent, SearchOutlined, Segmented, SegmentedProps, Space, Text, TypeScript, Typography, ant, content, data, documentInfo, filterContent, handleDelete, handleDisabledClick, handleEnabledClick...

---
*Generated by RAGFlow Repository Documentation Generator*
