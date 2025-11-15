# File Documentation: web/src/pages/agent/gobal-variable-sheet/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/gobal-variable-sheet/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 168
- **Characters**: 6,018
- **Size**: 6,018 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/gobal-variable-sheet/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 168 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `GlobalParamSheet`: Exported entity

### Functions (4)

- `GlobalParamSheet()`: Function definition
- `handleDeleteGlobalVariable()`: Function definition
- `handleEditGlobalVariable()`: Function definition
- `newFields()`: Function definition

### Imports (16)

- `import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';`
- `import { FormFieldConfig } from '@/components/dynamic-form';`
- `import { BlockButton, Button } from '@/components/ui/button';`
- `import {`
- `import { useSetModalState } from '@/hooks/common-hooks';`
- `import { useFetchAgent } from '@/hooks/use-agent-request';`
- `import { GlobalVariableType } from '@/interfaces/database/agent';`
- `import { cn } from '@/lib/utils';`
- `import { t } from 'i18next';`
- `import { Trash2 } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 168
- Blank lines: 6 (3.6%)
- Comment lines: ~0 (0.0%)
- Code lines: ~162


## Dependencies and Imports

- `@/components/confirm-delete-dialog`
- `@/components/dynamic-form`
- `@/components/ui/button`
- `@/hooks/common-hooks`
- `@/hooks/use-agent-request`
- `@/interfaces/database/agent`
- `@/lib/utils`
- `i18next`
- `lucide-react`
- `react`
- `react-hook-form`
- `../hooks/use-save-graph`
- `./component/add-variable-modal`
- `./hooks/use-object-fields`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/gobal-variable-sheet`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/gobal-variable-sheet/` directory
- Potential test file: `test_index.tsx`

## Keywords

../hooks/use-save-graph, ./component/add-variable-modal, ./hooks/use-object-fields, @/components/confirm-delete-dialog, @/components/dynamic-form, @/components/ui/button, @/hooks/common-hooks, @/hooks/use-agent-request, @/interfaces/database/agent, @/lib/utils, AddVariableModal, ArrayBoolean, ArrayNumber, ArrayObject, ArrayString, BlockButton, Button, ConfirmDeleteDialog, FieldValues, FormFieldConfig, GlobalFormFields, GlobalParamSheet, GlobalVariableFormDefaultValues, GlobalVariableType, IGlobalParamModalProps, Object, Record, Sheet, SheetContent, SheetHeader, SheetTitle, Trash2, TypeMaps, TypeScript, TypesWithArray, as, handleDeleteGlobalVariable, handleEditGlobalVariable, i18next, item, lucide-react, newField, newFields, param, react, react-hook-form, res

---
*Generated by RAGFlow Repository Documentation Generator*
