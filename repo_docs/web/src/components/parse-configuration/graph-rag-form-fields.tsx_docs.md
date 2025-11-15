# File Documentation: web/src/components/parse-configuration/graph-rag-form-fields.tsx

## File Metadata

- **Path**: `web/src/components/parse-configuration/graph-rag-form-fields.tsx`
- **Extension**: `.tsx`
- **Lines**: 257
- **Characters**: 7,810
- **Size**: 7,810 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/components/parse-configuration/graph-rag-form-fields.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 257 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (4)

- `showTagItems`: Exported entity
- `excludedParseMethods`: Exported entity
- `showGraphRagItems`: Exported entity
- `UseGraphRagFormField`: Exported entity

### Functions (6)

- `showTagItems()`: Function definition
- `showGraphRagItems()`: Function definition
- `UseGraphRagFormField()`: Function definition
- `GraphRagItems()`: Function definition
- `methodOptions()`: Function definition
- `renderWideTooltip()`: Function definition

### Imports (12)

- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import { cn } from '@/lib/utils';`
- `import {`
- `import { upperFirst } from 'lodash';`
- `import { useCallback, useMemo } from 'react';`
- `import { useFormContext, useWatch } from 'react-hook-form';`
- `import { EntityTypesFormField } from '../entity-types-form-field';`
- `import { FormContainer } from '../form-container';`
- `import {`

## Code Structure Analysis

- Total lines: 257
- Blank lines: 17 (6.6%)
- Comment lines: ~1 (0.4%)
- Code lines: ~239


## Dependencies and Imports

- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `@/lib/utils`
- `lodash`
- `react`
- `react-hook-form`
- `../entity-types-form-field`
- `../form-container`
- `../ui/select`
- `../ui/switch`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/components/parse-configuration`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/components/parse-configuration/` directory
- Potential test file: `test_graph-rag-form-fields.tsx`

## Keywords

../entity-types-form-field, ../form-container, ../ui/select, ../ui/switch, @/constants/knowledge, @/hooks/common-hooks, @/lib/utils, DocumentParserType, EntityTypesFormField, FormContainer, FormControl, FormField, FormItem, FormLabel, FormMessage, General, GenerateLogButton, GenerateType, GraphRagItems, GraphRagItemsProps, IGenerateLogButtonProps, KnowledgeGraph, Light, MethodValue, Picture, RAGFlowSelect, React, ReactNode, Resume, Switch, Table, Tag, The, TypeScript, UseGraphRagFormField, enum, excludedParseMethods, excludedTagParseMethods, form, lodash, methodOptions, react, react-hook-form, renderWideTooltip, showGraphRagItems, showTagItems, useRaptor

---
*Generated by RAGFlow Repository Documentation Generator*
