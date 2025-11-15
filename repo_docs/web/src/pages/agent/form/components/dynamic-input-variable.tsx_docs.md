# File Documentation: web/src/pages/agent/form/components/dynamic-input-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/dynamic-input-variable.tsx`
- **Extension**: `.tsx`
- **Lines**: 128
- **Characters**: 3,774
- **Size**: 3,774 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { RAGFlowNodeType } from '@/interfaces/database/flow';
import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';
import { Button, Collapse, Flex, Form, Input, Select } from 'antd';
import { PropsWithChildren, useCallback } from 'react';
import { useTranslation } from 'react-i18next';
import { useBuildVariableOptions } from '../../hooks/use-get-begin-query';

import styles from './index.less';

interface IProps {
  node?: RAGFlowNodeType;
}

enum VariableType {
  Reference = 'reference',
  Input = 'input',
}

const getVariableName = (type: string) =>
  type === VariableType.Reference ? 'component_id' : 'value';

const DynamicVariableForm = ({ node }: IProps) => {
  const { t } = useTranslation();
  const valueOptions = useBuildVariableOptions(node?.id, node?.parentId);
  const form = Form.useFormInstance();

  const options = [
    { value: VariableType.Reference, label: t('flow.reference') },
    { value: VariableType.Input, label: t('flow.text') },
  ];

  const handleTypeChange = useCallback(
    (name: number) => () => {
      setTimeout(() => {
        form.setFieldValue(['query', name, 'component_id'], undefined);
        form.setFieldValue(['query', name, 'value'], undefined);
      }, 0);
    },
    [form],
  );

  return (
    <Form.List name="query">
      {(fields, { add, remove }) => (
        <>
          {fields.map(({ key, name, ...restField }) => (
            <Flex key={key} gap={10} align={'baseline'}>
              <Form.Item
                {...restField}
                name={[name, 'type']}
                className={styles.variableType}
              >
                <Select
                  options={options}
                  onChange={handleTypeChange(name)}
                ></Select>
              </Form.Item>
              <Form.Item noStyle dependencies={[name, 'type']}>
                {({ getFieldValue }) => {
                  const type = getFieldValue(['query', name, 'type']);
                  return (
                    <Form.Item
                      {...restField}
                      name={[name, getVariableName(type)]}
                      className={styles.variableValue}
                    >
                      {type === VariableType.Reference ? (
                        <Select
                          placeholder={t('common.pleaseSelect')}
                          options={valueOptions}
                        ></Select>
                      ) : (
                        <Input placeholder={t('common.pleaseInput')} />
                      )}
                    </Form.Item>
                  );
                }}
              </Form.Item>
              <MinusCircleOutlined onClick={() => remove(name)} />
            </Flex>
          ))}
          <Form.Item>
            <Button
              type="dashed"
              onClick={() => add({ type: VariableType.Reference })}
              block
              icon={<PlusOutlined />}
              className={styles.addButton}
            >
              {t('flow.addVariable')}
            </Button>
          </Form.Item>
        </>
      )}
    </Form.List>
  );
};

export function FormCollapse({
  children,
  title,
}: PropsWithChildren<{ title: string }>) {
  return (
    <Collapse
      className={styles.dynamicInputVariable}
      defaultActiveKey={['1']}
      items={[
        {
          key: '1',
          label: <span className={styles.title}>{title}</span>,
          children,
        },
      ]}
    />
  );
}

const DynamicInputVariable = ({ node }: IProps) => {
  const { t } = useTranslation();
  return (
    <FormCollapse title={t('flow.input')}>
      <DynamicVariableForm node={node}></DynamicVariableForm>
    </FormCollapse>
  );
};

export default DynamicInputVariable;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/components/dynamic-input-variable.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 128 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `FormCollapse`: Exported entity

### Functions (5)

- `getVariableName()`: Function definition
- `DynamicVariableForm()`: Function definition
- `handleTypeChange()`: Function definition
- `FormCollapse()`: Function definition
- `DynamicInputVariable()`: Function definition

### Imports (7)

- `import { RAGFlowNodeType } from '@/interfaces/database/flow';`
- `import { MinusCircleOutlined, PlusOutlined } from '@ant-design/icons';`
- `import { Button, Collapse, Flex, Form, Input, Select } from 'antd';`
- `import { PropsWithChildren, useCallback } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import { useBuildVariableOptions } from '../../hooks/use-get-begin-query';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 12 (9.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~116


## Dependencies and Imports

- `@/interfaces/database/flow`
- `@ant-design/icons`
- `antd`
- `react`
- `react-i18next`
- `../../hooks/use-get-begin-query`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/components`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Contains database queries - ensure proper indexing and query optimization

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/form/components/` directory
- Potential test file: `test_dynamic-input-variable.tsx`

## Keywords

../../hooks/use-get-begin-query, ./index.less, @/interfaces/database/flow, @ant-design/icons, Button, Collapse, DynamicInputVariable, DynamicVariableForm, Flex, Form, FormCollapse, IProps, Input, Item, List, MinusCircleOutlined, PlusOutlined, PropsWithChildren, RAGFlowNodeType, Reference, Select, TypeScript, VariableType, ant, antd, form, getVariableName, handleTypeChange, options, react, react-i18next, type, valueOptions

---
*Generated by RAGFlow Repository Documentation Generator*
