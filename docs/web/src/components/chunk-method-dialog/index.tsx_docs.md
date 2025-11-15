# Documentation: web/src/components/chunk-method-dialog/index.tsx

## File Metadata

- **Path**: `web/src/components/chunk-method-dialog/index.tsx`
- **Size**: 12227 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/chunk-method-dialog/index.tsx`.

## Original Source Code

```tsx
import {
  Dialog,
  DialogContent,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { DocumentParserType } from '@/constants/knowledge';
import { useFetchKnowledgeBaseConfiguration } from '@/hooks/use-knowledge-request';
import { IModalProps } from '@/interfaces/common';
import { IParserConfig } from '@/interfaces/database/document';
import { IChangeParserConfigRequestBody } from '@/interfaces/request/document';
import {
  ChunkMethodItem,
  EnableTocToggle,
  ParseTypeItem,
} from '@/pages/dataset/dataset-setting/configuration/common-item';
import { zodResolver } from '@hookform/resolvers/zod';
import omit from 'lodash/omit';
import {} from 'module';
import { useEffect, useMemo } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import {
  AutoKeywordsFormField,
  AutoQuestionsFormField,
} from '../auto-keywords-form-field';
import { DataFlowSelect } from '../data-pipeline-select';
import { DelimiterFormField } from '../delimiter-form-field';
import { EntityTypesFormField } from '../entity-types-form-field';
import { ExcelToHtmlFormField } from '../excel-to-html-form-field';
import { FormContainer } from '../form-container';
import { LayoutRecognizeFormField } from '../layout-recognize-form-field';
import { MaxTokenNumberFormField } from '../max-token-number-from-field';
import { ButtonLoading } from '../ui/button';
import { Input } from '../ui/input';
import { DynamicPageRange } from './dynamic-page-range';
import { useShowAutoKeywords } from './hooks';
import {
  useDefaultParserValues,
  useFillDefaultValueOnMount,
} from './use-default-parser-values';

const FormId = 'ChunkMethodDialogForm';

interface IProps
  extends IModalProps<{
    parserId: string;
    parserConfig: IChangeParserConfigRequestBody;
  }> {
  loading: boolean;
  parserId: string;
  pipelineId?: string;
  parserConfig: IParserConfig;
  documentExtension: string;
  documentId: string;
}

const hidePagesChunkMethods = [
  DocumentParserType.Qa,
  DocumentParserType.Table,
  DocumentParserType.Picture,
  DocumentParserType.Resume,
  DocumentParserType.One,
  DocumentParserType.KnowledgeGraph,
];

export function ChunkMethodDialog({
  hideModal,
  onOk,
  parserId,
  pipelineId,
  documentExtension,
  visible,
  parserConfig,
  loading,
}: IProps) {
  const { t } = useTranslation();

  const { data: knowledgeDetails } = useFetchKnowledgeBaseConfiguration();

  const useGraphRag = useMemo(() => {
    return knowledgeDetails.parser_config?.graphrag?.use_graphrag;
  }, [knowledgeDetails.parser_config?.graphrag?.use_graphrag]);

  const defaultParserValues = useDefaultParserValues();

  const fillDefaultParserValue = useFillDefaultValueOnMount();

  const FormSchema = z
    .object({
      parseType: z.number(),
      parser_id: z
        .string()
        .min(1, {
          message: t('common.pleaseSelect'),
        })
        .trim(),
      pipeline_id: z.string().optional(),
      parser_config: z.object({
        task_page_size: z.coerce.number().optional(),
        layout_recognize: z.string().optional(),
        chunk_token_num: z.coerce.number().optional(),
        delimiter: z.string().optional(),
        auto_keywords: z.coerce.number().optional(),
        auto_questions: z.coerce.number().optional(),
        html4excel: z.boolean().optional(),
        toc_extraction: z.boolean().optional(),
        // raptor: z
        //   .object({
        //     use_raptor: z.boolean().optional(),
        //     prompt: z.string().optional().optional(),
        //     max_token: z.coerce.number().optional(),
        //     threshold: z.coerce.number().optional(),
        //     max_cluster: z.coerce.number().optional(),
        //     random_seed: z.coerce.number().optional(),
        //   })
        //   .optional(),
        // graphrag: z.object({
        //   use_graphrag: z.boolean().optional(),
        // }),
        entity_types: z.array(z.string()).optional(),
        pages: z
          .array(z.object({ from: z.coerce.number(), to: z.coerce.number() }))
          .optional(),
      }),
    })
    .superRefine((data, ctx) => {
      if (data.parseType === 2 && !data.pipeline_id) {
        ctx.addIssue({
          path: ['pipeline_id'],
          message: t('common.pleaseSelect'),
          code: 'custom',
        });
      }
    });

  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
    defaultValues: {
      parser_id: parserId || '',
      pipeline_id: pipelineId || '',
      parseType: pipelineId ? 2 : 1,
      parser_config: defaultParserValues,
    },
  });

  const layoutRecognize = useWatch({
    name: 'parser_config.layout_recognize',
    control: form.control,
  });

  const selectedTag = useWatch({
    name: 'parser_id',
    control: form.control,
  });

  const isPdf = documentExtension === 'pdf';

  const showPages = useMemo(() => {
    return isPdf && hidePagesChunkMethods.every((x) => x !== selectedTag);
  }, [selectedTag, isPdf]);

  const showOne = useMemo(() => {
    return (
      isPdf &&
      hidePagesChunkMethods
        .filter((x) => x !== DocumentParserType.One)
        .every((x) => x !== selectedTag)
    );
  }, [selectedTag, isPdf]);

  const showMaxTokenNumber =
    selectedTag === DocumentParserType.Naive ||
    selectedTag === DocumentParserType.KnowledgeGraph;

  const showEntityTypes = selectedTag === DocumentParserType.KnowledgeGraph;

  const showExcelToHtml =
    selectedTag === DocumentParserType.Naive && documentExtension === 'xlsx';

  const showAutoKeywords = useShowAutoKeywords();

  async function onSubmit(data: z.infer<typeof FormSchema>) {
    console.log('🚀 ~ onSubmit ~ data:', data);
    const nextData = {
      ...data,
      parser_config: {
        ...data.parser_config,
        pages: data.parser_config?.pages?.map((x: any) => [x.from, x.to]) ?? [],
      },
    };
    console.log('🚀 ~ onSubmit ~ nextData:', nextData);
    const ret = await onOk?.(nextData);
    if (ret) {
      hideModal?.();
    }
  }

  useEffect(() => {
    if (visible) {
      const pages =
        parserConfig?.pages?.map((x) => ({ from: x[0], to: x[1] })) ?? [];
      form.reset({
        parser_id: parserId || '',
        pipeline_id: pipelineId || '',
        parseType: pipelineId ? 2 : 1,
        parser_config: fillDefaultParserValue({
          pages: pages.length > 0 ? pages : [{ from: 1, to: 1024 }],
          ...omit(parserConfig, 'pages'),
          // graphrag: {
          //   use_graphrag: get(
          //     parserConfig,
          //     'graphrag.use_graphrag',
          //     useGraphRag,
          //   ),
          // },
        }),
      });
    }
  }, [
    fillDefaultParserValue,
    form,
    knowledgeDetails.parser_config,
    parserConfig,
    parserId,
    pipelineId,
    useGraphRag,
    visible,
  ]);
  const parseType = useWatch({
    control: form.control,
    name: 'parseType',
    defaultValue: pipelineId ? 2 : 1,
  });
  useEffect(() => {
    if (parseType === 1) {
      form.setValue('pipeline_id', '');
    }
  }, [parseType, form]);
  return (
    <Dialog open onOpenChange={hideModal}>
      <DialogContent className="max-w-[50vw] text-text-primary">
        <DialogHeader>
          <DialogTitle>{t('knowledgeDetails.chunkMethod')}</DialogTitle>
        </DialogHeader>
        <Form {...form}>
          <form
            onSubmit={form.handleSubmit(onSubmit)}
            className="space-y-6 max-h-[70vh] overflow-auto"
            id={FormId}
          >
            <FormContainer>
              <ParseTypeItem />
              {parseType === 1 && <ChunkMethodItem></ChunkMethodItem>}
              {parseType === 2 && (
                <DataFlowSelect
                  isMult={false}
                  // toDataPipeline={navigateToAgents}
                  formFieldName="pipeline_id"
                />
              )}

              {/* <FormField
                control={form.control}
                name="parser_id"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>{t('knowledgeDetails.chunkMethod')}</FormLabel>
                    <FormControl>
                      <RAGFlowSelect
                        {...field}
                        options={parserList}
                      ></RAGFlowSelect>
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              /> */}
              {showPages && parseType === 1 && (
                <DynamicPageRange></DynamicPageRange>
              )}
              {showPages && parseType === 1 && layoutRecognize && (
                <FormField
                  control={form.control}
                  name="parser_config.task_page_size"
                  render={({ field }) => (
                    <FormItem>
                      <FormLabel
                        tooltip={t('knowledgeDetails.taskPageSizeTip')}
                      >
                        {t('knowledgeDetails.taskPageSize')}
                      </FormLabel>
                      <FormControl>
                        <Input
                          {...field}
                          type={'number'}
                          min={1}
                          max={128}
                        ></Input>
                      </FormControl>
                      <FormMessage />
                    </FormItem>
                  )}
                />
              )}
            </FormContainer>
            {parseType === 1 && (
              <>
                <FormContainer
                  show={showOne || showMaxTokenNumber}
                  className="space-y-3"
                >
                  {showOne && (
                    <LayoutRecognizeFormField></LayoutRecognizeFormField>
                  )}
                  {showMaxTokenNumber && (
                    <>
                      <MaxTokenNumberFormField
                        max={
                          selectedTag === DocumentParserType.KnowledgeGraph
                            ? 8192 * 2
                            : 2048
                        }
                      ></MaxTokenNumberFormField>
                      <DelimiterFormField></DelimiterFormField>
                    </>
                  )}
                </FormContainer>
                <FormContainer
                  show={showAutoKeywords(selectedTag) || showExcelToHtml}
                  className="space-y-3"
                >
                  {selectedTag === DocumentParserType.Naive && (
                    <EnableTocToggle />
                  )}
                  {showAutoKeywords(selectedTag) && (
                    <>
                      <AutoKeywordsFormField></AutoKeywordsFormField>
                      <AutoQuestionsFormField></AutoQuestionsFormField>
                    </>
                  )}
                  {showExcelToHtml && (
                    <ExcelToHtmlFormField></ExcelToHtmlFormField>
                  )}
                </FormContainer>
                {/* {showRaptorParseConfiguration(
                  selectedTag as DocumentParserType,
                ) && (
                  <FormContainer>
                    <RaptorFormFields></RaptorFormFields>
                  </FormContainer>
                )} */}
                {/* {showGraphRagItems(selectedTag as DocumentParserType) &&
                  useGraphRag && (
                    <FormContainer>
                      <UseGraphRagFormField></UseGraphRagFormField>
                    </FormContainer>
                  )} */}
                {showEntityTypes && (
                  <EntityTypesFormField></EntityTypesFormField>
                )}
              </>
            )}
          </form>
        </Form>
        <DialogFooter>
          <ButtonLoading type="submit" form={FormId} loading={loading}>
            {t('common.save')}
          </ButtonLoading>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/chunk-method-dialog/index.tsx` is located in the `web/src/components/chunk-method-dialog` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-method-dialog.

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

- [dynamic-page-range.tsx](dynamic-page-range.tsx_docs.md)
- [hooks.ts](hooks.ts_docs.md)
- [use-default-parser-values.ts](use-default-parser-values.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
