# Documentation: web/src/pages/agent/form/components/dynamic-input-variable.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/components/dynamic-input-variable.tsx`
- **Size**: 3774 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/components/dynamic-input-variable.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/components/dynamic-input-variable.tsx` is located in the `web/src/pages/agent/form/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [api-key-field.tsx](api-key-field.tsx_docs.md)
- [description-field.tsx](description-field.tsx_docs.md)
- [dynamic-fom-header.tsx](dynamic-fom-header.tsx_docs.md)
- [form-wrapper.tsx](form-wrapper.tsx_docs.md)
- [index.less](index.less_docs.md)
- [next-dynamic-input-variable.tsx](next-dynamic-input-variable.tsx_docs.md)
- [output.tsx](output.tsx_docs.md)
- [query-variable-list.tsx](query-variable-list.tsx_docs.md)
- [query-variable.tsx](query-variable.tsx_docs.md)
- [select-with-secondary-menu.tsx](select-with-secondary-menu.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
