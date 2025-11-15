# Documentation: web/src/components/parse-configuration/graph-rag-form-fields.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/graph-rag-form-fields.tsx`
- **Size**: 7810 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/parse-configuration/graph-rag-form-fields.tsx`.

## Original Source Code

```tsx
import { DocumentParserType } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import {
  GenerateLogButton,
  GenerateType,
  IGenerateLogButtonProps,
} from '@/pages/dataset/dataset/generate-button/generate';
import { upperFirst } from 'lodash';
import { useCallback, useMemo } from 'react';
import { useFormContext, useWatch } from 'react-hook-form';
import { EntityTypesFormField } from '../entity-types-form-field';
import { FormContainer } from '../form-container';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '../ui/form';
import { RAGFlowSelect } from '../ui/select';
import { Switch } from '../ui/switch';

const excludedTagParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Tag,
];

export const showTagItems = (parserId: DocumentParserType) => {
  return !excludedTagParseMethods.includes(parserId);
};

const enum MethodValue {
  General = 'general',
  Light = 'light',
}

export const excludedParseMethods = [
  DocumentParserType.Table,
  DocumentParserType.Resume,
  DocumentParserType.Picture,
  DocumentParserType.KnowledgeGraph,
  DocumentParserType.Qa,
  DocumentParserType.Tag,
];

export const showGraphRagItems = (parserId: DocumentParserType | undefined) => {
  return !excludedParseMethods.some((x) => x === parserId);
};

type GraphRagItemsProps = {
  marginBottom?: boolean;
  className?: string;
  data: IGenerateLogButtonProps;
  onDelete?: () => void;
};

export function UseGraphRagFormField({
  data,
  onDelete,
}: {
  data: IGenerateLogButtonProps;
  onDelete?: () => void;
}) {
  const form = useFormContext();
  const { t } = useTranslate('knowledgeConfiguration');

  return (
    <FormField
      control={form.control}
      name="parser_config.graphrag.use_graphrag"
      render={() => (
        <FormItem defaultChecked={false} className=" items-center space-y-0 ">
          <div className="flex items-center gap-1">
            <FormLabel
              tooltip={t('useGraphRagTip')}
              className="text-sm whitespace-break-spaces w-1/4"
            >
              {t('useGraphRag')}
            </FormLabel>
            <div className="w-3/4">
              <FormControl>
                {/* <Switch
                  checked={field.value}
                  onCheckedChange={field.onChange}
                ></Switch> */}
                <GenerateLogButton
                  {...data}
                  onDelete={onDelete}
                  className="w-full text-text-secondary"
                  status={1}
                  type={GenerateType.KnowledgeGraph}
                />
              </FormControl>
            </div>
          </div>
          <div className="flex pt-1">
            <div className="w-1/4"></div>
            <FormMessage />
          </div>
        </FormItem>
      )}
    />
  );
}

// The three types "table", "resume" and "one" do not display this configuration.
const GraphRagItems = ({
  marginBottom = false,
  className = 'p-10',
  data,
  onDelete,
}: GraphRagItemsProps) => {
  const { t } = useTranslate('knowledgeConfiguration');
  const form = useFormContext();

  const useRaptor = useWatch({
    control: form.control,
    name: 'parser_config.graphrag.use_graphrag',
  });

  const methodOptions = useMemo(() => {
    return [MethodValue.Light, MethodValue.General].map((x) => ({
      value: x,
      label: upperFirst(x),
    }));
  }, []);

  const renderWideTooltip = useCallback(
    (title: React.ReactNode | string) => {
      return typeof title === 'string' ? t(title) : title;
    },
    [t],
  );

  return (
    <FormContainer className={cn({ 'mb-4': marginBottom }, className)}>
      <UseGraphRagFormField
        data={data}
        onDelete={onDelete}
      ></UseGraphRagFormField>
      {useRaptor && (
        <>
          <EntityTypesFormField name="parser_config.graphrag.entity_types"></EntityTypesFormField>
          <FormField
            control={form.control}
            name="parser_config.graphrag.method"
            render={({ field }) => (
              <FormItem className=" items-center space-y-0 ">
                <div className="flex items-center">
                  <FormLabel
                    className="text-sm whitespace-nowrap w-1/4"
                    tooltip={renderWideTooltip(
                      <div
                        dangerouslySetInnerHTML={{
                          __html: t('graphRagMethodTip'),
                        }}
                      ></div>,
                    )}
                  >
                    {t('graphRagMethod')}
                  </FormLabel>
                  <div className="w-3/4">
                    <FormControl>
                      <RAGFlowSelect
                        {...field}
                        options={methodOptions}
                      ></RAGFlowSelect>
                    </FormControl>
                  </div>
                </div>
                <div className="flex pt-1">
                  <div className="w-1/4"></div>
                  <FormMessage />
                </div>
              </FormItem>
            )}
          />

          <FormField
            control={form.control}
            name="parser_config.graphrag.resolution"
            render={({ field }) => (
              <FormItem className=" items-center space-y-0 ">
                <div className="flex items-center">
                  <FormLabel
                    tooltip={renderWideTooltip('resolutionTip')}
                    className="text-sm whitespace-nowrap w-1/4"
                  >
                    {t('resolution')}
                  </FormLabel>
                  <div className="w-3/4">
                    <FormControl>
                      <Switch
                        checked={field.value}
                        onCheckedChange={field.onChange}
                      ></Switch>
                    </FormControl>
                  </div>
                </div>
                <div className="flex pt-1">
                  <div className="w-1/4"></div>
                  <FormMessage />
                </div>
              </FormItem>
            )}
          />

          <FormField
            control={form.control}
            name="parser_config.graphrag.community"
            render={({ field }) => (
              <FormItem className=" items-center space-y-0 ">
                <div className="flex items-center">
                  <FormLabel
                    tooltip={renderWideTooltip('communityTip')}
                    className="text-sm whitespace-nowrap w-1/4"
                  >
                    {t('community')}
                  </FormLabel>
                  <div className="w-3/4">
                    <FormControl>
                      <Switch
                        checked={field.value}
                        onCheckedChange={field.onChange}
                      ></Switch>
                    </FormControl>
                  </div>
                </div>
                <div className="flex pt-1">
                  <div className="w-1/4"></div>
                  <FormMessage />
                </div>
              </FormItem>
            )}
          />
          {/* {showGenerateItem && (
            <div className="w-full flex items-center">
              <div className="text-sm whitespace-nowrap w-1/4">
                {t('extractKnowledgeGraph')}
              </div>
              <GenerateLogButton
                className="w-3/4 text-text-secondary"
                status={1}
                type={GenerateType.KnowledgeGraph}
              />
            </div>
          )} */}
        </>
      )}
    </FormContainer>
  );
};

export default GraphRagItems;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/parse-configuration/graph-rag-form-fields.tsx` is located in the `web/src/components/parse-configuration` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to parse-configuration.

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

- [graph-rag-items.tsx](graph-rag-items.tsx_docs.md)
- [index.tsx](index.tsx_docs.md)
- [raptor-form-fields-old.tsx](raptor-form-fields-old.tsx_docs.md)
- [raptor-form-fields.tsx](raptor-form-fields.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
