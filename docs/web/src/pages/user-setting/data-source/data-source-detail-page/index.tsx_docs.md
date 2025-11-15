# Documentation: web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx

## File Metadata

- **Path**: `web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx`
- **Size**: 6243 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/user-setting/data-source/data-source-detail-page/index.tsx` is located in the `web/src/pages/user-setting/data-source/data-source-detail-page` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to data-source-detail-page.

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

- [log-table.tsx](log-table.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
