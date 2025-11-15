# Documentation: web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx

## File Metadata

- **Path**: `web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx`
- **Size**: 3917 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/gobal-variable-sheet/component/add-variable-modal.tsx` is located in the `web/src/pages/agent/gobal-variable-sheet/component` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to component.

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
