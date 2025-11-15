# File Documentation: web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx

## File Metadata

- **Path**: `web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx`
- **Extension**: `.tsx`
- **Lines**: 135
- **Characters**: 3,917
- **Size**: 3,917 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import {
  DynamicForm,
  DynamicFormRef,
  FormFieldConfig,
} from '@/components/dynamic-form';
import { Modal } from '@/components/ui/modal/modal';
import { t } from 'i18next';
import { useEffect, useRef } from 'react';
import { FieldValues } from 'react-hook-form';
import { TypeMaps, TypesWithArray } from '../constant';
import { useHandleForm } from '../hooks/use-form';
import { useObjectFields } from '../hooks/use-object-fields';

export const AddVariableModal = (props: {
  fields?: FormFieldConfig[];
  setFields: (value: any) => void;
  visible?: boolean;
  hideModal: () => void;
  defaultValues?: FieldValues;
  setDefaultValues?: (value: FieldValues) => void;
}) => {
  const {
    fields,
    setFields,
    visible,
    hideModal,
    defaultValues,
    setDefaultValues,
  } = props;

  const { handleSubmit: submitForm, loading } = useHandleForm();

  const { handleCustomValidate, handleCustomSchema, handleRender } =
    useObjectFields();

  const formRef = useRef<DynamicFormRef>(null);

  const handleFieldUpdate = (
    fieldName: string,
    updatedField: Partial<FormFieldConfig>,
  ) => {
    setFields((prevFields: any) =>
      prevFields.map((field: any) =>
        field.name === fieldName ? { ...field, ...updatedField } : field,
      ),
    );
  };

  useEffect(() => {
    const typeField = fields?.find((item) => item.name === 'type');

    if (typeField) {
      typeField.onChange = (value) => {
        handleFieldUpdate('value', {
          type: TypeMaps[value as keyof typeof TypeMaps],
          render: handleRender(value),
          customValidate: handleCustomValidate(value),
          schema: handleCustomSchema(value),
        });
        const values = formRef.current?.getValues();
        // setTimeout(() => {
        switch (value) {
          case TypesWithArray.Boolean:
            setDefaultValues?.({ ...values, value: false });
            break;
          case TypesWithArray.Number:
            setDefaultValues?.({ ...values, value: 0 });
            break;
          case TypesWithArray.Object:
            setDefaultValues?.({ ...values, value: {} });
            break;
          case TypesWithArray.ArrayString:
            setDefaultValues?.({ ...values, value: [''] });
            break;
          case TypesWithArray.ArrayNumber:
            setDefaultValues?.({ ...values, value: [''] });
            break;
          case TypesWithArray.ArrayBoolean:
            setDefaultValues?.({ ...values, value: [false] });
            break;
          case TypesWithArray.ArrayObject:
            setDefaultValues?.({ ...values, value: [] });
            break;
          default:
            setDefaultValues?.({ ...values, value: '' });
            break;
        }
        // }, 0);
      };
    }
  }, [fields]);

  const handleSubmit = async (fieldValue: FieldValues) => {
    await submitForm(fieldValue);
    hideModal();
  };

  return (
    <Modal
      title={t('flow.add') + t('flow.conversationVariable')}
      open={visible || false}
      onCancel={hideModal}
      showfooter={false}
    >
      <DynamicForm.Root
        ref={formRef}
        fields={fields || []}
        onSubmit={(data) => {
          console.log(data);
        }}
        defaultValues={defaultValues}
        onFieldUpdate={handleFieldUpdate}
      >
        <div className="flex items-center justify-end w-full gap-2">
          <DynamicForm.CancelButton
            handleCancel={() => {
              hideModal?.();
            }}
          />
          <DynamicForm.SavingButton
            submitLoading={loading || false}
            buttonText={t('common.ok')}
            submitFunc={(values: FieldValues) => {
              handleSubmit(values);
              // console.log(values);
              // console.log(nodes, edges);
              //   handleOk(values);
            }}
          />
        </div>
      </DynamicForm.Root>
    </Modal>
  );
};

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 135 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `AddVariableModal`: Exported entity

### Functions (5)

- `AddVariableModal()`: Function definition
- `handleFieldUpdate()`: Function definition
- `typeField()`: Function definition
- `values()`: Function definition
- `handleSubmit()`: Function definition

### Imports (8)

- `import {`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { t } from 'i18next';`
- `import { useEffect, useRef } from 'react';`
- `import { FieldValues } from 'react-hook-form';`
- `import { TypeMaps, TypesWithArray } from '../constant';`
- `import { useHandleForm } from '../hooks/use-form';`
- `import { useObjectFields } from '../hooks/use-object-fields';`

## Code Structure Analysis

- Total lines: 135
- Blank lines: 10 (7.4%)
- Comment lines: ~5 (3.7%)
- Code lines: ~120


## Dependencies and Imports

- `@/components/ui/modal/modal`
- `i18next`
- `react`
- `react-hook-form`
- `../constant`
- `../hooks/use-form`
- `../hooks/use-object-fields`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/gobal-variable-sheet/component`.

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

- Other files in `web/src/pages/agent/gobal-variable-sheet/component/` directory
- Potential test file: `test_add-variable-modal.tsx`

## Keywords

../constant, ../hooks/use-form, ../hooks/use-object-fields, @/components/ui/modal/modal, AddVariableModal, ArrayBoolean, ArrayNumber, ArrayObject, ArrayString, Boolean, CancelButton, DynamicForm, DynamicFormRef, FieldValues, FormFieldConfig, Modal, Number, Object, Partial, Root, SavingButton, TypeMaps, TypeScript, TypesWithArray, formRef, handleFieldUpdate, handleSubmit, i18next, react, react-hook-form, typeField, values

---
*Generated by RAGFlow Repository Documentation Generator*
