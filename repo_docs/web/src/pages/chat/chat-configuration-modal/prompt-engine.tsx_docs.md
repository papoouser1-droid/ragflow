# File Documentation: web/src/pages/chat/chat-configuration-modal/prompt-engine.tsx

## File Metadata

- **Path**: `web/src/pages/chat/chat-configuration-modal/prompt-engine.tsx`
- **Extension**: `.tsx`
- **Lines**: 223
- **Characters**: 5,804
- **Size**: 5,804 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import SimilaritySlider from '@/components/similarity-slider';
import { DeleteOutlined, QuestionCircleOutlined } from '@ant-design/icons';
import {
  Button,
  Col,
  Divider,
  Form,
  Input,
  Row,
  Switch,
  Table,
  TableProps,
  Tooltip,
} from 'antd';
import classNames from 'classnames';
import {
  ForwardedRef,
  forwardRef,
  useEffect,
  useImperativeHandle,
  useState,
} from 'react';
import { v4 as uuid } from 'uuid';
import {
  VariableTableDataType as DataType,
  IPromptConfigParameters,
  ISegmentedContentProps,
} from '../interface';
import { EditableCell, EditableRow } from './editable-cell';

import { CrossLanguageItem } from '@/components/cross-language-item';
import Rerank from '@/components/rerank';
import TopNItem from '@/components/top-n-item';
import { UseKnowledgeGraphItem } from '@/components/use-knowledge-graph-item';
import { useTranslate } from '@/hooks/common-hooks';
import { useSelectPromptConfigParameters } from '../hooks';
import styles from './index.less';

const PromptEngine = (
  { show }: ISegmentedContentProps,
  ref: ForwardedRef<Array<IPromptConfigParameters>>,
) => {
  const [dataSource, setDataSource] = useState<DataType[]>([]);
  const parameters = useSelectPromptConfigParameters();
  const { t } = useTranslate('chat');

  const components = {
    body: {
      row: EditableRow,
      cell: EditableCell,
    },
  };

  const handleRemove = (key: string) => () => {
    const newData = dataSource.filter((item) => item.key !== key);
    setDataSource(newData);
  };

  const handleSave = (row: DataType) => {
    const newData = [...dataSource];
    const index = newData.findIndex((item) => row.key === item.key);
    const item = newData[index];
    newData.splice(index, 1, {
      ...item,
      ...row,
    });
    setDataSource(newData);
  };

  const handleAdd = () => {
    setDataSource((state) => [
      ...state,
      {
        key: uuid(),
        variable: '',
        optional: true,
      },
    ]);
  };

  const handleOptionalChange = (row: DataType) => (checked: boolean) => {
    const newData = [...dataSource];
    const index = newData.findIndex((item) => row.key === item.key);
    const item = newData[index];
    newData.splice(index, 1, {
      ...item,
      optional: checked,
    });
    setDataSource(newData);
  };

  useImperativeHandle(
    ref,
    () => {
      return dataSource
        .filter((x) => x.variable.trim() !== '')
        .map((x) => ({ key: x.variable, optional: x.optional }));
    },
    [dataSource],
  );

  const columns: TableProps<DataType>['columns'] = [
    {
      title: t('key'),
      dataIndex: 'variable',
      key: 'variable',
      onCell: (record: DataType) => ({
        record,
        editable: true,
        dataIndex: 'variable',
        title: 'key',
        handleSave,
      }),
    },
    {
      title: t('optional'),
      dataIndex: 'optional',
      key: 'optional',
      width: 40,
      align: 'center',
      render(text, record) {
        return (
          <Switch
            size="small"
            checked={text}
            onChange={handleOptionalChange(record)}
          />
        );
      },
    },
    {
      title: t('operation'),
      dataIndex: 'operation',
      width: 30,
      key: 'operation',
      align: 'center',
      render(_, record) {
        return <DeleteOutlined onClick={handleRemove(record.key)} />;
      },
    },
  ];

  useEffect(() => {
    setDataSource(parameters);
  }, [parameters]);

  return (
    <section
      className={classNames({
        [styles.segmentedHidden]: !show,
      })}
    >
      <Form.Item
        label={t('system')}
        rules={[{ required: true, message: t('systemMessage') }]}
        tooltip={t('systemTip')}
        name={['prompt_config', 'system']}
        initialValue={t('systemInitialValue')}
      >
        <Input.TextArea autoSize={{ maxRows: 8, minRows: 5 }} />
      </Form.Item>
      <Divider></Divider>
      <SimilaritySlider isTooltipShown></SimilaritySlider>
      <TopNItem></TopNItem>
      <Form.Item
        label={t('multiTurn')}
        tooltip={t('multiTurnTip')}
        name={['prompt_config', 'refine_multiturn']}
        initialValue={false}
      >
        <Switch></Switch>
      </Form.Item>
      <UseKnowledgeGraphItem
        filedName={['prompt_config', 'use_kg']}
      ></UseKnowledgeGraphItem>
      <Form.Item
        label={t('reasoning')}
        tooltip={t('reasoningTip')}
        name={['prompt_config', 'reasoning']}
        initialValue={false}
      >
        <Switch></Switch>
      </Form.Item>
      <Rerank></Rerank>
      <CrossLanguageItem></CrossLanguageItem>
      <section className={classNames(styles.variableContainer)}>
        <Row align={'middle'} justify="end">
          <Col span={9} className={styles.variableAlign}>
            <label className={styles.variableLabel}>
              {t('variable')}
              <Tooltip title={t('variableTip')}>
                <QuestionCircleOutlined className={styles.variableIcon} />
              </Tooltip>
            </label>
          </Col>
          <Col span={15} className={styles.variableAlign}>
            <Button size="small" onClick={handleAdd}>
              {t('add')}
            </Button>
          </Col>
        </Row>
        {dataSource.length > 0 && (
          <Row>
            <Col span={7}> </Col>
            <Col span={17}>
              <Table
                dataSource={dataSource}
                columns={columns}
                rowKey={'key'}
                className={styles.variableTable}
                components={components}
                rowClassName={() => styles.editableRow}
              />
            </Col>
          </Row>
        )}
      </section>
    </section>
  );
};

export default forwardRef(PromptEngine);

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/chat/chat-configuration-modal/prompt-engine.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 223 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (9)

- `PromptEngine()`: Function definition
- `handleRemove()`: Function definition
- `newData()`: Function definition
- `handleSave()`: Function definition
- `index()`: Function definition
- `handleAdd()`: Function definition
- `handleOptionalChange()`: Function definition
- `index()`: Function definition
- `item()`: Function definition

### Imports (15)

- `import SimilaritySlider from '@/components/similarity-slider';`
- `import { DeleteOutlined, QuestionCircleOutlined } from '@ant-design/icons';`
- `import {`
- `import classNames from 'classnames';`
- `import {`
- `import { v4 as uuid } from 'uuid';`
- `import {`
- `import { EditableCell, EditableRow } from './editable-cell';`
- `import { CrossLanguageItem } from '@/components/cross-language-item';`
- `import Rerank from '@/components/rerank';`

## Code Structure Analysis

- Total lines: 223
- Blank lines: 13 (5.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~210


## Dependencies and Imports

- `@/components/similarity-slider`
- `@ant-design/icons`
- `classnames`
- `uuid`
- `./editable-cell`
- `@/components/cross-language-item`
- `@/components/rerank`
- `@/components/top-n-item`
- `@/components/use-knowledge-graph-item`
- `@/hooks/common-hooks`
- `../hooks`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/chat/chat-configuration-modal`.

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

- Other files in `web/src/pages/chat/chat-configuration-modal/` directory
- Potential test file: `test_prompt-engine.tsx`

## Keywords

../hooks, ./editable-cell, ./index.less, @/components/cross-language-item, @/components/rerank, @/components/similarity-slider, @/components/top-n-item, @/components/use-knowledge-graph-item, @/hooks/common-hooks, @ant-design/icons, Array, Button, Col, CrossLanguageItem, DataType, DeleteOutlined, Divider, EditableCell, EditableRow, Form, ForwardedRef, IPromptConfigParameters, ISegmentedContentProps, Input, Item, PromptEngine, QuestionCircleOutlined, Rerank, Row, SimilaritySlider, Switch, Table, TableProps, TextArea, Tooltip, TopNItem, TypeScript, UseKnowledgeGraphItem, VariableTableDataType, ant, classnames, columns, components, handleAdd, handleOptionalChange, handleRemove, handleSave, index, item, newData...

---
*Generated by RAGFlow Repository Documentation Generator*
