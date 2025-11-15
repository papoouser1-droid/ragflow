# File Documentation: web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 194
- **Characters**: 6,243
- **Size**: 6,243 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import BackButton from '@/components/back-button';
import {
  DynamicForm,
  DynamicFormRef,
  FormFieldConfig,
  FormFieldType,
} from '@/components/dynamic-form';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';
import { RunningStatus } from '@/constants/knowledge';
import { t } from 'i18next';
import { debounce } from 'lodash';
import { CirclePause, Repeat } from 'lucide-react';
import { useCallback, useEffect, useMemo, useRef, useState } from 'react';
import { FieldValues } from 'react-hook-form';
import {
  DataSourceFormBaseFields,
  DataSourceFormDefaultValues,
  DataSourceFormFields,
  DataSourceInfo,
} from '../contant';
import {
  useAddDataSource,
  useDataSourceResume,
  useFetchDataSourceDetail,
} from '../hooks';
import { DataSourceLogsTable } from './log-table';

const SourceDetailPage = () => {
  const formRef = useRef<DynamicFormRef>(null);

  const { data: detail } = useFetchDataSourceDetail();
  const { handleResume } = useDataSourceResume();

  const detailInfo = useMemo(() => {
    if (detail) {
      return DataSourceInfo[detail.source];
    }
  }, [detail]);

  const [fields, setFields] = useState<FormFieldConfig[]>([]);
  const [defaultValues, setDefaultValues] = useState<FieldValues>(
    DataSourceFormDefaultValues[
      detail?.source as keyof typeof DataSourceFormDefaultValues
    ] as FieldValues,
  );

  const runSchedule = useCallback(() => {
    handleResume({
      resume:
        detail?.status === RunningStatus.RUNNING ||
        detail?.status === RunningStatus.SCHEDULE
          ? false
          : true,
    });
  }, [detail, handleResume]);

  const customFields = useMemo(() => {
    return [
      {
        label: 'Refresh Freq',
        name: 'refresh_freq',
        type: FormFieldType.Number,
        required: false,
        render: (fieldProps: FormFieldConfig) => (
          <div className="flex items-center  gap-1 w-full relative">
            <Input {...fieldProps} type={FormFieldType.Number} />
            <span className="absolute right-0 -translate-x-[58px] text-text-secondary italic ">
              minutes
            </span>
            <button
              type="button"
              className="text-text-secondary bg-bg-input rounded-sm text-xs h-full p-2 border border-border-button "
              onClick={() => {
                runSchedule();
              }}
            >
              {detail?.status === RunningStatus.RUNNING ||
              detail?.status === RunningStatus.SCHEDULE ? (
                <CirclePause size={12} />
              ) : (
                <Repeat size={12} />
              )}
            </button>
          </div>
        ),
      },
      {
        label: 'Prune Freq',
        name: 'prune_freq',
        type: FormFieldType.Number,
        required: false,
        hidden: true,
        render: (fieldProps: FormFieldConfig) => {
          return (
            <div className="flex items-center  gap-1 w-full relative">
              <Input {...fieldProps} type={FormFieldType.Number} />
              <span className="absolute right-0 -translate-x-6 text-text-secondary italic ">
                hours
              </span>
            </div>
          );
        },
      },
      {
        label: 'Timeout Secs',
        name: 'timeout_secs',
        type: FormFieldType.Number,
        required: false,
        render: (fieldProps: FormFieldConfig) => (
          <div className="flex items-center  gap-1 w-full relative">
            <Input {...fieldProps} type={FormFieldType.Number} />
            <span className="absolute right-0 -translate-x-6 text-text-secondary italic ">
              seconds
            </span>
          </div>
        ),
      },
    ];
  }, [detail, runSchedule]);

  const { handleAddOk } = useAddDataSource();

  const onSubmit = useCallback(() => {
    formRef?.current?.submit();
  }, [formRef]);

  useEffect(() => {
    if (detail) {
      const fields = [
        ...DataSourceFormBaseFields,
        ...DataSourceFormFields[
          detail.source as keyof typeof DataSourceFormFields
        ],
        ...customFields,
      ] as FormFieldConfig[];

      const neweFields = fields.map((field) => {
        return {
          ...field,
          horizontal: true,
          onChange: () => {
            onSubmit();
          },
        };
      });
      setFields(neweFields);

      const defultValueTemp = {
        ...(DataSourceFormDefaultValues[
          detail?.source as keyof typeof DataSourceFormDefaultValues
        ] as FieldValues),
        ...detail,
      };
      console.log('defaultValue', defultValueTemp);
      setDefaultValues(defultValueTemp);
    }
  }, [detail, customFields, onSubmit]);

  return (
    <div className="px-10 py-5">
      <BackButton />
      <Card className="bg-transparent border border-border-button px-5 pt-[10px] pb-5 rounded-md mt-5">
        <CardHeader className="flex flex-row items-center justify-between space-y-0 p-0 pb-3">
          {/* <Users className="mr-2 h-5 w-5 text-[#1677ff]" /> */}
          <CardTitle className="text-2xl text-text-primary flex gap-1 items-center font-normal pb-3">
            {detailInfo?.icon}
            {detail?.name}
          </CardTitle>
        </CardHeader>
        <Separator className="border-border-button bg-border-button w-[calc(100%+2rem)] -translate-x-4 -translate-y-4" />
        <CardContent className="p-2 flex flex-col gap-2 max-h-[calc(100vh-190px)] overflow-y-auto scrollbar-auto">
          <div className="max-w-[1200px]">
            <DynamicForm.Root
              ref={formRef}
              fields={fields}
              onSubmit={debounce((data) => {
                handleAddOk(data);
              }, 500)}
              defaultValues={defaultValues}
            />
          </div>
          <section className="flex flex-col gap-2 mt-6">
            <div className="text-2xl text-text-primary">{t('setting.log')}</div>
            <DataSourceLogsTable refresh_freq={detail?.refresh_freq || false} />
          </section>
        </CardContent>
      </Card>
    </div>
  );
};
export default SourceDetailPage;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 194 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (6)

- `SourceDetailPage()`: Function definition
- `detailInfo()`: Function definition
- `runSchedule()`: Function definition
- `customFields()`: Function definition
- `onSubmit()`: Function definition
- `neweFields()`: Function definition

### Imports (14)

- `import BackButton from '@/components/back-button';`
- `import {`
- `import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';`
- `import { Input } from '@/components/ui/input';`
- `import { Separator } from '@/components/ui/separator';`
- `import { RunningStatus } from '@/constants/knowledge';`
- `import { t } from 'i18next';`
- `import { debounce } from 'lodash';`
- `import { CirclePause, Repeat } from 'lucide-react';`
- `import { useCallback, useEffect, useMemo, useRef, useState } from 'react';`

## Code Structure Analysis

- Total lines: 194
- Blank lines: 13 (6.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~181


## Dependencies and Imports

- `@/components/back-button`
- `@/components/ui/card`
- `@/components/ui/input`
- `@/components/ui/separator`
- `@/constants/knowledge`
- `i18next`
- `lodash`
- `lucide-react`
- `react`
- `react-hook-form`
- `./log-table`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/user-setting/data-source/data-source-detail-page`.

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

- Other files in `web/src/pages/user-setting/data-source/data-source-detail-page/` directory
- Potential test file: `test_index.tsx`

## Keywords

./log-table, @/components/back-button, @/components/ui/card, @/components/ui/input, @/components/ui/separator, @/constants/knowledge, BackButton, Card, CardContent, CardHeader, CardTitle, CirclePause, DataSourceFormBaseFields, DataSourceFormDefaultValues, DataSourceFormFields, DataSourceInfo, DataSourceLogsTable, DynamicForm, DynamicFormRef, FieldValues, FormFieldConfig, FormFieldType, Freq, Input, Number, Prune, RUNNING, Refresh, Repeat, Root, RunningStatus, SCHEDULE, Secs, Separator, SourceDetailPage, Timeout, TypeScript, Users, customFields, defultValueTemp, detailInfo, fields, formRef, i18next, lodash, lucide-react, neweFields, onSubmit, react, react-hook-form...

---
*Generated by RAGFlow Repository Documentation Generator*
