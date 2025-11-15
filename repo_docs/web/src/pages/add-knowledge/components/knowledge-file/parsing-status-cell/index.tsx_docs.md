# File Documentation: web/src/pages/add-knowledge/components/knowledge-file/parsing-status-cell/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-file/parsing-status-cell/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 144
- **Characters**: 4,031
- **Size**: 4,031 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ReactComponent as CancelIcon } from '@/assets/svg/cancel.svg';
import { ReactComponent as RefreshIcon } from '@/assets/svg/refresh.svg';
import { ReactComponent as RunIcon } from '@/assets/svg/run.svg';
import { useTranslate } from '@/hooks/common-hooks';
import { IDocumentInfo } from '@/interfaces/database/document';
import {
  Badge,
  DescriptionsProps,
  Flex,
  Popconfirm,
  Popover,
  Space,
  Tag,
} from 'antd';
import classNames from 'classnames';
import { useTranslation } from 'react-i18next';
import reactStringReplace from 'react-string-replace';
import { DocumentType, RunningStatus, RunningStatusMap } from '../constant';
import { useHandleRunDocumentByIds } from '../hooks';
import { isParserRunning } from '../utils';
import styles from './index.less';

const iconMap = {
  [RunningStatus.UNSTART]: RunIcon,
  [RunningStatus.RUNNING]: CancelIcon,
  [RunningStatus.CANCEL]: RefreshIcon,
  [RunningStatus.DONE]: RefreshIcon,
  [RunningStatus.FAIL]: RefreshIcon,
};

interface IProps {
  record: IDocumentInfo;
}

const PopoverContent = ({ record }: IProps) => {
  const { t } = useTranslate('knowledgeDetails');

  const replaceText = (text: string) => {
    // Remove duplicate \n
    const nextText = text.replace(/(\n)\1+/g, '$1');

    const replacedText = reactStringReplace(
      nextText,
      /(\[ERROR\].+\s)/g,
      (match, i) => {
        return (
          <span key={i} className={styles.popoverContentErrorLabel}>
            {match}
          </span>
        );
      },
    );

    return replacedText;
  };

  const items: DescriptionsProps['items'] = [
    {
      key: 'process_begin_at',
      label: t('processBeginAt'),
      children: record.process_begin_at,
    },
    {
      key: 'process_duration',
      label: t('processDuration'),
      children: `${record.process_duration.toFixed(2)} s`,
    },
    {
      key: 'progress_msg',
      label: t('progressMsg'),
      children: replaceText(record.progress_msg.trim()),
    },
  ];

  return (
    <Flex vertical className={styles.popoverContent}>
      {items.map((x, idx) => {
        return (
          <div key={x.key} className={idx < 2 ? styles.popoverContentItem : ''}>
            <b>{x.label}:</b>
            <div className={styles.popoverContentText}>{x.children}</div>
          </div>
        );
      })}
    </Flex>
  );
};

export const ParsingStatusCell = ({ record }: IProps) => {
  const text = record.run;
  const runningStatus = RunningStatusMap[text];
  const { t } = useTranslation();
  const { handleRunDocumentByIds } = useHandleRunDocumentByIds(record.id);

  const isRunning = isParserRunning(text);

  const OperationIcon = iconMap[text];

  const label = t(`knowledgeDetails.runningStatus${text}`);

  const handleOperationIconClick =
    (shouldDelete: boolean = false) =>
    () => {
      handleRunDocumentByIds(record.id, isRunning, shouldDelete);
    };

  return record.type === DocumentType.Virtual ? null : (
    <Flex justify={'space-between'} align="center">
      <Popover content={<PopoverContent record={record}></PopoverContent>}>
        <Tag color={runningStatus.color}>
          {isRunning ? (
            <Space>
              <Badge color={runningStatus.color} />
              {label}
              <span>{(record.progress * 100).toFixed(2)}%</span>
            </Space>
          ) : (
            label
          )}
        </Tag>
      </Popover>
      <Popconfirm
        title={t(`knowledgeDetails.redo`, { chunkNum: record.chunk_num })}
        onConfirm={handleOperationIconClick(true)}
        onCancel={handleOperationIconClick(false)}
        disabled={record.chunk_num === 0}
        okText={t('common.yes')}
        cancelText={t('common.no')}
      >
        <div
          className={classNames(styles.operationIcon)}
          onClick={
            record.chunk_num === 0 ? handleOperationIconClick(false) : () => {}
          }
        >
          <OperationIcon />
        </div>
      </Popconfirm>
    </Flex>
  );
};

export default ParsingStatusCell;

```

## High-Level Overview

    // Remove duplicate \n

## Detailed Walkthrough

### Exports (1)

- `ParsingStatusCell`: Exported entity

### Functions (5)

- `PopoverContent()`: Function definition
- `replaceText()`: Function definition
- `replacedText()`: Function definition
- `ParsingStatusCell()`: Function definition
- `handleOperationIconClick()`: Function definition

### Imports (13)

- `import { ReactComponent as CancelIcon } from '@/assets/svg/cancel.svg';`
- `import { ReactComponent as RefreshIcon } from '@/assets/svg/refresh.svg';`
- `import { ReactComponent as RunIcon } from '@/assets/svg/run.svg';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import {`
- `import classNames from 'classnames';`
- `import { useTranslation } from 'react-i18next';`
- `import reactStringReplace from 'react-string-replace';`
- `import { DocumentType, RunningStatus, RunningStatusMap } from '../constant';`

## Code Structure Analysis

- Total lines: 144
- Blank lines: 16 (11.1%)
- Comment lines: ~1 (0.7%)
- Code lines: ~127


## Dependencies and Imports

- `@/assets/svg/cancel.svg`
- `@/assets/svg/refresh.svg`
- `@/assets/svg/run.svg`
- `@/hooks/common-hooks`
- `@/interfaces/database/document`
- `classnames`
- `react-i18next`
- `react-string-replace`
- `../constant`
- `../hooks`
- `../utils`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-file/parsing-status-cell`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-file/parsing-status-cell/` directory
- Potential test file: `test_index.tsx`

## Keywords

../constant, ../hooks, ../utils, ./index.less, @/assets/svg/cancel.svg, @/assets/svg/refresh.svg, @/assets/svg/run.svg, @/hooks/common-hooks, @/interfaces/database/document, Badge, CANCEL, CancelIcon, DONE, DescriptionsProps, DocumentType, ERROR, FAIL, Flex, IDocumentInfo, IProps, OperationIcon, ParsingStatusCell, Popconfirm, Popover, PopoverContent, RUNNING, ReactComponent, RefreshIcon, Remove, RunIcon, RunningStatus, RunningStatusMap, Space, Tag, TypeScript, UNSTART, Virtual, classnames, handleOperationIconClick, iconMap, isRunning, items, label, nextText, react-i18next, react-string-replace, replaceText, replacedText, runningStatus, text

---
*Generated by RAGFlow Repository Documentation Generator*
