# Documentation: web/src/pages/agent/gobal-variable-sheet/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/gobal-variable-sheet/index.tsx`
- **Size**: 6018 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/gobal-variable-sheet/index.tsx`.

## Original Source Code

```tsx
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import { FormFieldConfig } from '@/components/dynamic-form';
import { BlockButton, Button } from '@/components/ui/button';
import {
  Sheet,
  SheetContent,
  SheetHeader,
  SheetTitle,
} from '@/components/ui/sheet';
import { useSetModalState } from '@/hooks/common-hooks';
import { useFetchAgent } from '@/hooks/use-agent-request';
import { GlobalVariableType } from '@/interfaces/database/agent';
import { cn } from '@/lib/utils';
import { t } from 'i18next';
import { Trash2 } from 'lucide-react';
import { useState } from 'react';
import { FieldValues } from 'react-hook-form';
import { useSaveGraph } from '../hooks/use-save-graph';
import { AddVariableModal } from './component/add-variable-modal';
import {
  GlobalFormFields,
  GlobalVariableFormDefaultValues,
  TypeMaps,
  TypesWithArray,
} from './constant';
import { useObjectFields } from './hooks/use-object-fields';

export type IGlobalParamModalProps = {
  data: any;
  hideModal: (open: boolean) => void;
};
export const GlobalParamSheet = (props: IGlobalParamModalProps) => {
  const { hideModal } = props;
  const { data, refetch } = useFetchAgent();
  const { visible, showModal, hideModal: hideAddModal } = useSetModalState();
  const [fields, setFields] = useState<FormFieldConfig[]>(GlobalFormFields);
  const [defaultValues, setDefaultValues] = useState<FieldValues>(
    GlobalVariableFormDefaultValues,
  );
  const { handleCustomValidate, handleCustomSchema, handleRender } =
    useObjectFields();
  const { saveGraph } = useSaveGraph();

  const handleDeleteGlobalVariable = async (key: string) => {
    const param = {
      ...(data.dsl?.variables || {}),
    } as Record<string, GlobalVariableType>;
    delete param[key];
    const res = await saveGraph(undefined, {
      globalVariables: param,
    });
    if (res.code === 0) {
      refetch();
    }
  };

  const handleEditGlobalVariable = (item: FieldValues) => {
    const newFields = fields.map((field) => {
      let newField = field;
      newField.render = undefined;
      newField.schema = undefined;
      newField.customValidate = undefined;
      if (newField.name === 'value') {
        newField = {
          ...newField,
          type: TypeMaps[item.type as keyof typeof TypeMaps],
          render: handleRender(item.type),
          customValidate: handleCustomValidate(item.type),
          schema: handleCustomSchema(item.type),
        };
      }
      return newField;
    });
    setFields(newFields);
    setDefaultValues(item);
    showModal();
  };
  return (
    <>
      <Sheet open onOpenChange={hideModal} modal={false}>
        <SheetContent
          className={cn('top-20 h-auto flex flex-col p-0 gap-0')}
          onInteractOutside={(e) => e.preventDefault()}
        >
          <SheetHeader className="p-5">
            <SheetTitle className="flex items-center gap-2.5">
              {t('flow.conversationVariable')}
            </SheetTitle>
          </SheetHeader>

          <div className="px-5 pb-5">
            <BlockButton
              onClick={() => {
                setFields(GlobalFormFields);
                setDefaultValues(GlobalVariableFormDefaultValues);
                showModal();
              }}
            >
              {t('flow.add')}
            </BlockButton>
          </div>

          <div className="flex flex-col gap-2 px-5 ">
            {data?.dsl?.variables &&
              Object.keys(data.dsl.variables).map((key) => {
                const item = data.dsl.variables[key];
                return (
                  <div
                    key={key}
                    className="flex items-center gap-3 min-h-14 justify-between px-5 py-3 border border-border-default rounded-lg  hover:bg-bg-card group"
                    onClick={() => {
                      handleEditGlobalVariable(item);
                    }}
                  >
                    <div className="flex flex-col">
                      <div className="flex items-center gap-2">
                        <span className=" font-medium">{item.name}</span>
                        <span className="text-sm font-medium text-text-secondary">
                          {item.type}
                        </span>
                      </div>
                      {![
                        TypesWithArray.Object,
                        TypesWithArray.ArrayObject,
                        TypesWithArray.ArrayString,
                        TypesWithArray.ArrayNumber,
                        TypesWithArray.ArrayBoolean,
                      ].includes(item.type as TypesWithArray) && (
                        <div>
                          <span className="text-text-primary">
                            {item.value}
                          </span>
                        </div>
                      )}
                    </div>
                    <div>
                      <ConfirmDeleteDialog
                        onOk={() => handleDeleteGlobalVariable(key)}
                      >
                        <Button
                          variant={'secondary'}
                          className="bg-transparent hidden text-text-secondary border-none group-hover:bg-bg-card group-hover:text-text-primary group-hover:border group-hover:block"
                          onClick={(e) => {
                            e.stopPropagation();
                          }}
                        >
                          <Trash2 className="w-4 h-4" />
                        </Button>
                      </ConfirmDeleteDialog>
                    </div>
                  </div>
                );
              })}
          </div>
        </SheetContent>
        <AddVariableModal
          visible={visible}
          hideModal={hideAddModal}
          fields={fields}
          setFields={setFields}
          defaultValues={defaultValues}
          setDefaultValues={setDefaultValues}
        />
      </Sheet>
    </>
  );
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/gobal-variable-sheet/index.tsx` is located in the `web/src/pages/agent/gobal-variable-sheet` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to gobal-variable-sheet.

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

- [constant.ts](constant.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
