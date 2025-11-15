# Documentation: web/src/pages/agent/debug-content/index.tsx

## File Metadata

- **Path**: `web/src/pages/agent/debug-content/index.tsx`
- **Size**: 7531 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/agent/debug-content/index.tsx`.

## Original Source Code

```tsx
import { ButtonLoading } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { RAGFlowSelect } from '@/components/ui/select';
import { Switch } from '@/components/ui/switch';
import { Textarea } from '@/components/ui/textarea';
import { IMessage } from '@/pages/chat/interface';
import { zodResolver } from '@hookform/resolvers/zod';
import React, { ReactNode, useCallback, useMemo } from 'react';
import { useForm } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { BeginQueryType } from '../constant';
import { BeginQuery } from '../interface';
import { FileUploadDirectUpload } from './uploader';

const StringFields = [
  BeginQueryType.Line,
  BeginQueryType.Paragraph,
  BeginQueryType.Options,
];

interface IProps {
  parameters: BeginQuery[];
  message?: IMessage;
  ok(parameters: any[]): void;
  isNext?: boolean;
  loading?: boolean;
  submitButtonDisabled?: boolean;
  btnText?: ReactNode;
}

const DebugContent = ({
  parameters,
  message,
  ok,
  isNext = true,
  loading = false,
  submitButtonDisabled = false,
  btnText,
}: IProps) => {
  const { t } = useTranslation();

  const formSchemaValues = useMemo(() => {
    const obj = parameters.reduce<{
      schema: Record<string, z.ZodType>;
      values: Record<string, any>;
    }>(
      (pre, cur, idx) => {
        const type = cur.type;
        let fieldSchema;
        let value;
        if (StringFields.some((x) => x === type)) {
          fieldSchema = z.string().trim().min(1);
        } else if (type === BeginQueryType.Boolean) {
          fieldSchema = z.boolean();
          value = false;
        } else if (type === BeginQueryType.Integer || type === 'float') {
          fieldSchema = z.coerce.number();
        } else {
          fieldSchema = z.record(z.any());
        }

        if (cur.optional) {
          fieldSchema = fieldSchema.optional();
        }

        const index = idx.toString();

        pre.schema[index] = fieldSchema;
        pre.values[index] = value;

        return pre;
      },
      { schema: {}, values: {} },
    );

    return { schema: z.object(obj.schema), values: obj.values };
  }, [parameters]);

  const form = useForm<z.infer<typeof formSchemaValues.schema>>({
    defaultValues: formSchemaValues.values,
    resolver: zodResolver(formSchemaValues.schema),
  });

  const submittable = true;

  const renderWidget = useCallback(
    (q: BeginQuery, idx: string) => {
      const props = {
        key: idx,
        label: q.name ?? q.key,
        name: idx,
      };

      const BeginQueryTypeMap = {
        [BeginQueryType.Line]: (
          <FormField
            control={form.control}
            name={props.name}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{props.label}</FormLabel>
                <FormControl>
                  <Input {...field}></Input>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        ),
        [BeginQueryType.Paragraph]: (
          <FormField
            control={form.control}
            name={props.name}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{props.label}</FormLabel>
                <FormControl>
                  <Textarea rows={1} {...field}></Textarea>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        ),
        [BeginQueryType.Options]: (
          <FormField
            control={form.control}
            name={props.name}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{props.label}</FormLabel>
                <FormControl>
                  <RAGFlowSelect
                    allowClear
                    options={
                      q.options?.map((x) => ({
                        label: x,
                        value: x as string,
                      })) ?? []
                    }
                    {...field}
                  ></RAGFlowSelect>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        ),
        [BeginQueryType.File]: (
          <React.Fragment key={idx}>
            <FormField
              control={form.control}
              name={props.name}
              render={({ field }) => (
                <div className="space-y-6">
                  <FormItem className="w-full">
                    <FormLabel>{t('assistantAvatar')}</FormLabel>
                    <FormControl>
                      <FileUploadDirectUpload
                        value={field.value}
                        onChange={field.onChange}
                      ></FileUploadDirectUpload>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                </div>
              )}
            />
          </React.Fragment>
        ),
        [BeginQueryType.Integer]: (
          <FormField
            control={form.control}
            name={props.name}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{props.label}</FormLabel>
                <FormControl>
                  <Input type="number" {...field}></Input>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        ),
        [BeginQueryType.Boolean]: (
          <FormField
            control={form.control}
            name={props.name}
            render={({ field }) => (
              <FormItem className="flex-1">
                <FormLabel>{props.label}</FormLabel>
                <FormControl>
                  <Switch
                    checked={field.value}
                    onCheckedChange={field.onChange}
                  ></Switch>
                </FormControl>
                <FormMessage />
              </FormItem>
            )}
          />
        ),
      };

      return (
        BeginQueryTypeMap[q.type as BeginQueryType] ??
        BeginQueryTypeMap[BeginQueryType.Paragraph]
      );
    },
    [form, t],
  );

  const onSubmit = useCallback(
    (values: z.infer<typeof formSchemaValues.schema>) => {
      const nextValues = Object.entries(values).map(([key, value]) => {
        const item = parameters[Number(key)];
        return { ...item, value };
      });

      ok(nextValues);
    },
    [formSchemaValues, ok, parameters],
  );
  return (
    <>
      <section>
        {message?.data?.tips && <div className="mb-2">{message.data.tips}</div>}
        <Form {...form}>
          <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
            {parameters.map((x, idx) => {
              return <div key={idx}>{renderWidget(x, idx.toString())}</div>;
            })}
            <div>
              <ButtonLoading
                type="submit"
                loading={loading}
                disabled={!submittable || submitButtonDisabled}
                className="w-full mt-1"
              >
                {btnText || t(isNext ? 'common.next' : 'flow.run')}
              </ButtonLoading>
            </div>
          </form>
        </Form>
      </section>
    </>
  );
};

export default DebugContent;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/agent/debug-content/index.tsx` is located in the `web/src/pages/agent/debug-content` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to debug-content.

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

- [popover-form.tsx](popover-form.tsx_docs.md)
- [uploader.tsx](uploader.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
