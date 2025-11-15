# File Documentation: web/src/pages/agent/form/jin10-form/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/form/jin10-form/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 146
- **Characters**: 4,439
- **Size**: 4,439 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/form/jin10-form/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 146 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (7)

- `Jin10Form()`: Function definition
- `jin10TypeOptions()`: Function definition
- `jin10FlashTypeOptions()`: Function definition
- `jin10CalendarTypeOptions()`: Function definition
- `jin10CalendarDatashapeOptions()`: Function definition
- `jin10SymbolsTypeOptions()`: Function definition
- `jin10SymbolsDatatypeOptions()`: Function definition

### Imports (6)

- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { Form, Input, Select } from 'antd';`
- `import { useMemo } from 'react';`
- `import { IOperatorForm } from '../../interface';`
- `import {`
- `import DynamicInputVariable from '../components/dynamic-input-variable';`

## Code Structure Analysis

- Total lines: 146
- Blank lines: 14 (9.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~132


## Dependencies and Imports

- `@/hooks/common-hooks`
- `antd`
- `react`
- `../../interface`
- `../components/dynamic-input-variable`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/form/jin10-form`.

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

- Other files in `web/src/pages/agent/form/jin10-form/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../interface, ../components/dynamic-input-variable, @/hooks/common-hooks, DynamicInputVariable, Form, IOperatorForm, Input, Item, Jin10CalendarDatashapeOptions, Jin10CalendarTypeOptions, Jin10FlashTypeOptions, Jin10Form, Jin10SymbolsDatatypeOptions, Jin10SymbolsTypeOptions, Jin10TypeOptions, Select, TypeScript, antd, jin10CalendarDatashapeOptions, jin10CalendarTypeOptions, jin10FlashTypeOptions, jin10SymbolsDatatypeOptions, jin10SymbolsTypeOptions, jin10TypeOptions, react, type

---
*Generated by RAGFlow Repository Documentation Generator*
