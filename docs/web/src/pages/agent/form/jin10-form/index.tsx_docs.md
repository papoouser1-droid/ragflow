# Documentation: web/src/pages/agent/form/jin10-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/jin10-form/index.tsx`
- **Size**: 4439 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/form/jin10-form/index.tsx`.

## Original Source Code

```tsx
import { useTranslate } from '@/hooks/common-hooks';
import { Form, Input, Select } from 'antd';
import { useMemo } from 'react';
import { IOperatorForm } from '../../interface';
import {
  Jin10CalendarDatashapeOptions,
  Jin10CalendarTypeOptions,
  Jin10FlashTypeOptions,
  Jin10SymbolsDatatypeOptions,
  Jin10SymbolsTypeOptions,
  Jin10TypeOptions,
} from '../../options';
import DynamicInputVariable from '../components/dynamic-input-variable';

const Jin10Form = ({ onValuesChange, form, node }: IOperatorForm) => {
  const { t } = useTranslate('flow');

  const jin10TypeOptions = useMemo(() => {
    return Jin10TypeOptions.map((x) => ({
      value: x,
      label: t(`jin10TypeOptions.${x}`),
    }));
  }, [t]);

  const jin10FlashTypeOptions = useMemo(() => {
    return Jin10FlashTypeOptions.map((x) => ({
      value: x,
      label: t(`jin10FlashTypeOptions.${x}`),
    }));
  }, [t]);

  const jin10CalendarTypeOptions = useMemo(() => {
    return Jin10CalendarTypeOptions.map((x) => ({
      value: x,
      label: t(`jin10CalendarTypeOptions.${x}`),
    }));
  }, [t]);

  const jin10CalendarDatashapeOptions = useMemo(() => {
    return Jin10CalendarDatashapeOptions.map((x) => ({
      value: x,
      label: t(`jin10CalendarDatashapeOptions.${x}`),
    }));
  }, [t]);

  const jin10SymbolsTypeOptions = useMemo(() => {
    return Jin10SymbolsTypeOptions.map((x) => ({
      value: x,
      label: t(`jin10SymbolsTypeOptions.${x}`),
    }));
  }, [t]);

  const jin10SymbolsDatatypeOptions = useMemo(() => {
    return Jin10SymbolsDatatypeOptions.map((x) => ({
      value: x,
      label: t(`jin10SymbolsDatatypeOptions.${x}`),
    }));
  }, [t]);

  return (
    <Form
      name="basic"
      autoComplete="off"
      form={form}
      onValuesChange={onValuesChange}
      layout={'vertical'}
    >
      <DynamicInputVariable node={node}></DynamicInputVariable>
      <Form.Item label={t('type')} name={'type'} initialValue={'flash'}>
        <Select options={jin10TypeOptions}></Select>
      </Form.Item>
      <Form.Item label={t('secretKey')} name={'secret_key'}>
        <Input></Input>
      </Form.Item>
      <Form.Item noStyle dependencies={['type']}>
        {({ getFieldValue }) => {
          const type = getFieldValue('type');
          switch (type) {
            case 'flash':
              return (
                <>
                  <Form.Item label={t('flashType')} name={'flash_type'}>
                    <Select options={jin10FlashTypeOptions}></Select>
                  </Form.Item>
                  <Form.Item label={t('contain')} name={'contain'}>
                    <Input></Input>
                  </Form.Item>
                  <Form.Item label={t('filter')} name={'filter'}>
                    <Input></Input>
                  </Form.Item>
                </>
              );

            case 'calendar':
              return (
                <>
                  <Form.Item label={t('calendarType')} name={'calendar_type'}>
                    <Select options={jin10CalendarTypeOptions}></Select>
                  </Form.Item>
                  <Form.Item
                    label={t('calendarDatashape')}
                    name={'calendar_datashape'}
                  >
                    <Select options={jin10CalendarDatashapeOptions}></Select>
                  </Form.Item>
                </>
              );

            case 'symbols':
              return (
                <>
                  <Form.Item label={t('symbolsType')} name={'symbols_type'}>
                    <Select options={jin10SymbolsTypeOptions}></Select>
                  </Form.Item>
                  <Form.Item
                    label={t('symbolsDatatype')}
                    name={'symbols_datatype'}
                  >
                    <Select options={jin10SymbolsDatatypeOptions}></Select>
                  </Form.Item>
                </>
              );

            case 'news':
              return (
                <>
                  <Form.Item label={t('contain')} name={'contain'}>
                    <Input></Input>
                  </Form.Item>
                  <Form.Item label={t('filter')} name={'filter'}>
                    <Input></Input>
                  </Form.Item>
                </>
              );

            default:
              return <></>;
          }
        }}
      </Form.Item>
    </Form>
  );
};

export default Jin10Form;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/form/jin10-form/index.tsx` is located in the `web/src/pages/agent/form/jin10-form` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to jin10-form.

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
