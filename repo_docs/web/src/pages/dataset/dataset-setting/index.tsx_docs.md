# File Documentation: web/src/pages/dataset/dataset-setting/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 325
- **Characters**: 11,302
- **Size**: 11,314 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { DataFlowSelect } from '@/components/data-pipeline-select';
import GraphRagItems from '@/components/parse-configuration/graph-rag-form-fields';
import RaptorFormFields from '@/components/parse-configuration/raptor-form-fields';
import { Button } from '@/components/ui/button';
import Divider from '@/components/ui/divider';
import { Form } from '@/components/ui/form';
import { FormLayout } from '@/constants/form';
import { DocumentParserType } from '@/constants/knowledge';
import { PermissionRole } from '@/constants/permission';
import { IConnector } from '@/interfaces/database/knowledge';
import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';
import { IDataSourceBase } from '@/pages/user-setting/data-source/interface';
import { zodResolver } from '@hookform/resolvers/zod';
import { useEffect, useState } from 'react';
import { useForm, useWatch } from 'react-hook-form';
import { useTranslation } from 'react-i18next';
import { z } from 'zod';
import { TopTitle } from '../dataset-title';
import {
  GenerateType,
  IGenerateLogButtonProps,
} from '../dataset/generate-button/generate';
import { ChunkMethodForm } from './chunk-method-form';
import ChunkMethodLearnMore from './chunk-method-learn-more';
import LinkDataSource, {
  IDataSourceNodeProps,
} from './components/link-data-source';
import { MainContainer } from './configuration-form-container';
import { ChunkMethodItem, ParseTypeItem } from './configuration/common-item';
import { formSchema } from './form-schema';
import { GeneralForm } from './general-form';
import { useFetchKnowledgeConfigurationOnMount } from './hooks';
import { SavingButton } from './saving-button';
const enum DocumentType {
  DeepDOC = 'DeepDOC',
  PlainText = 'Plain Text',
}

const initialEntityTypes = [
  'organization',
  'person',
  'geo',
  'event',
  'category',
];

const enum MethodValue {
  General = 'general',
  Light = 'light',
}

export default function DatasetSettings() {
  const { t } = useTranslation();

  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      name: '',
      parser_id: DocumentParserType.Naive,
      permission: PermissionRole.Me,
      parser_config: {
        layout_recognize: DocumentType.DeepDOC,
        chunk_token_num: 512,
        delimiter: `\n`,
        auto_keywords: 0,
        auto_questions: 0,
        html4excel: false,
        topn_tags: 3,
        toc_extraction: false,
        raptor: {
          use_raptor: true,
          max_token: 256,
          threshold: 0.1,
          max_cluster: 64,
          random_seed: 0,
          scope: 'file',
          prompt: t('knowledgeConfiguration.promptText'),
        },
        graphrag: {
          use_graphrag: true,
          entity_types: initialEntityTypes,
          method: MethodValue.Light,
        },
      },
      pipeline_id: '',
      parseType: 1,
      pagerank: 0,
      connectors: [],
    },
  });
  const knowledgeDetails = useFetchKnowledgeConfigurationOnMount(form);
  // const [pipelineData, setPipelineData] = useState<IDataPipelineNodeProps>();
  const [sourceData, setSourceData] = useState<IDataSourceNodeProps[]>();
  const [graphRagGenerateData, setGraphRagGenerateData] =
    useState<IGenerateLogButtonProps>();
  const [raptorGenerateData, setRaptorGenerateData] =
    useState<IGenerateLogButtonProps>();

  useEffect(() => {
    console.log('🚀 ~ DatasetSettings ~ knowledgeDetails:', knowledgeDetails);
    if (knowledgeDetails) {
      // const data: IDataPipelineNodeProps = {
      //   id: knowledgeDetails.pipeline_id,
      //   name: knowledgeDetails.pipeline_name,
      //   avatar: knowledgeDetails.pipeline_avatar,
      //   linked: true,
      // };
      // setPipelineData(data);

      const source_data: IDataSourceNodeProps[] =
        knowledgeDetails?.connectors?.map((connector) => {
          return {
            ...connector,
            icon:
              DataSourceInfo[connector.source as keyof typeof DataSourceInfo]
                ?.icon || '',
          };
        });

      setSourceData(source_data);

      setGraphRagGenerateData({
        finish_at: knowledgeDetails.graphrag_task_finish_at,
        task_id: knowledgeDetails.graphrag_task_id,
      } as IGenerateLogButtonProps);
      setRaptorGenerateData({
        finish_at: knowledgeDetails.raptor_task_finish_at,
        task_id: knowledgeDetails.raptor_task_id,
      } as IGenerateLogButtonProps);
      form.setValue('parseType', knowledgeDetails.pipeline_id ? 2 : 1);
      form.setValue('pipeline_id', knowledgeDetails.pipeline_id || '');
    }
  }, [knowledgeDetails, form]);

  async function onSubmit(data: z.infer<typeof formSchema>) {
    try {
      console.log('Form validation passed, submit data', data);
    } catch (error) {
      console.error('An error occurred during submission:', error);
    }
  }
  // const handleLinkOrEditSubmit = (
  //   data: IDataPipelineSelectNode | undefined,
  // ) => {
  //   console.log('🚀 ~ DatasetSettings ~ data:', data);
  //   if (data) {
  //     setPipelineData(data);
  //     form.setValue('pipeline_id', data.id || '');
  //     // form.setValue('pipeline_name', data.name || '');
  //     // form.setValue('pipeline_avatar', data.avatar || '');
  //   }
  // };

  const handleLinkOrEditSubmit = (data: IConnector[] | undefined) => {
    if (data) {
      const connectors = data.map((connector) => {
        return {
          ...connector,
          auto_parse: connector.auto_parse === '0' ? '0' : '1',
          icon:
            DataSourceInfo[connector.source as keyof typeof DataSourceInfo]
              ?.icon || '',
        };
      });
      setSourceData(connectors as IDataSourceNodeProps[]);
      form.setValue('connectors', connectors || []);
      // form.setValue('pipeline_name', data.name || '');
      // form.setValue('pipeline_avatar', data.avatar || '');
    }
  };

  const handleDeletePipelineTask = (type: GenerateType) => {
    if (type === GenerateType.KnowledgeGraph) {
      setGraphRagGenerateData({
        finish_at: '',
        task_id: '',
      } as IGenerateLogButtonProps);
    } else if (type === GenerateType.Raptor) {
      setRaptorGenerateData({
        finish_at: '',
        task_id: '',
      } as IGenerateLogButtonProps);
    }
  };

  const parseType = useWatch({
    control: form.control,
    name: 'parseType',
    defaultValue: knowledgeDetails.pipeline_id ? 2 : 1,
  });
  const selectedTag = useWatch({
    name: 'parser_id',
    control: form.control,
  });
  useEffect(() => {
    if (parseType === 1) {
      form.setValue('pipeline_id', '');
    }
    console.log('parseType', parseType);
  }, [parseType, form]);

  const unbindFunc = (data: IDataSourceBase) => {
    if (data) {
      const connectors = sourceData?.filter((connector) => {
        return connector.id !== data.id;
      });
      console.log('🚀 ~ DatasetSettings ~ connectors:', connectors);
      setSourceData(connectors as IDataSourceNodeProps[]);
      form.setValue('connectors', connectors || []);
      // form.setValue('pipeline_name', data.name || '');
      // form.setValue('pipeline_avatar', data.avatar || '');
    }
  };
  const handleAutoParse = ({
    source_id,
    isAutoParse,
  }: {
    source_id: string;
    isAutoParse: boolean;
  }) => {
    if (source_id) {
      const connectors = sourceData?.map((connector) => {
        if (connector.id === source_id) {
          return {
            ...connector,
            auto_parse: isAutoParse ? '1' : '0',
          };
        }
        return connector;
      });
      console.log('🚀 ~ DatasetSettings ~ connectors:', connectors);
      setSourceData(connectors as IDataSourceNodeProps[]);
      form.setValue('connectors', connectors || []);
      // form.setValue('pipeline_name', data.name || '');
      // form.setValue('pipeline_avatar', data.avatar || '');
    }
  };

  return (
    <section className="p-5 h-full flex flex-col">
      <TopTitle
        title={t('knowledgeDetails.configuration')}
        description={t('knowledgeConfiguration.titleDescription')}
      ></TopTitle>
      <div className="flex gap-14 flex-1 min-h-0">
        <Form {...form}>
          <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6 ">
            <div className="w-[768px] h-[calc(100vh-240px)] pr-1 overflow-y-auto scrollbar-auto">
              <MainContainer className="text-text-secondary">
                <div className="text-base font-medium text-text-primary">
                  {t('knowledgeConfiguration.baseInfo')}
                </div>
                <GeneralForm></GeneralForm>
                <Divider />
                <div className="text-base font-medium text-text-primary">
                  {t('knowledgeConfiguration.globalIndex')}
                </div>
                <GraphRagItems
                  className="border-none p-0"
                  data={graphRagGenerateData as IGenerateLogButtonProps}
                  onDelete={() =>
                    handleDeletePipelineTask(GenerateType.KnowledgeGraph)
                  }
                ></GraphRagItems>
                <Divider />
                <RaptorFormFields
                  data={raptorGenerateData as IGenerateLogButtonProps}
                  onDelete={() => handleDeletePipelineTask(GenerateType.Raptor)}
                ></RaptorFormFields>
                <Divider />
                <div className="text-base font-medium text-text-primary">
                  {t('knowledgeConfiguration.dataPipeline')}
                </div>
                <ParseTypeItem line={1} />
                {parseType === 1 && (
                  <ChunkMethodItem line={1}></ChunkMethodItem>
                )}
                {parseType === 2 && (
                  <DataFlowSelect
                    isMult={false}
                    showToDataPipeline={true}
                    formFieldName="pipeline_id"
                    layout={FormLayout.Horizontal}
                  />
                )}

                {/* <Divider /> */}
                {parseType === 1 && <ChunkMethodForm />}

                {/* <LinkDataPipeline
                  data={pipelineData}
                  handleLinkOrEditSubmit={handleLinkOrEditSubmit}
                /> */}

                <Divider />
                <LinkDataSource
                  data={sourceData}
                  handleLinkOrEditSubmit={handleLinkOrEditSubmit}
                  unbindFunc={unbindFunc}
                  handleAutoParse={handleAutoParse}
                />
              </MainContainer>
            </div>
            <div className="text-right items-center flex justify-end gap-3 w-[768px]">
              <Button
                type="reset"
                className="bg-transparent text-color-white hover:bg-transparent border-gray-500 border-[1px]"
                onClick={() => {
                  form.reset();
                }}
              >
                {t('knowledgeConfiguration.cancel')}
              </Button>
              <SavingButton></SavingButton>
            </div>
          </form>
        </Form>
        <div className="flex-1">
          {parseType === 1 && <ChunkMethodLearnMore parserId={selectedTag} />}
        </div>
      </div>
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-setting/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 325 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `DatasetSettings`: Exported entity

### Functions (11)

- `DatasetSettings()`: Function definition
- `onSubmit()`: Function definition
- `handleLinkOrEditSubmit()`: Function definition
- `handleLinkOrEditSubmit()`: Function definition
- `connectors()`: Function definition
- `handleDeletePipelineTask()`: Function definition
- `selectedTag()`: Function definition
- `unbindFunc()`: Function definition
- `connectors()`: Function definition
- `handleAutoParse()`: Function definition
- `connectors()`: Function definition

### Imports (28)

- `import { DataFlowSelect } from '@/components/data-pipeline-select';`
- `import GraphRagItems from '@/components/parse-configuration/graph-rag-form-fields';`
- `import RaptorFormFields from '@/components/parse-configuration/raptor-form-fields';`
- `import { Button } from '@/components/ui/button';`
- `import Divider from '@/components/ui/divider';`
- `import { Form } from '@/components/ui/form';`
- `import { FormLayout } from '@/constants/form';`
- `import { DocumentParserType } from '@/constants/knowledge';`
- `import { PermissionRole } from '@/constants/permission';`
- `import { IConnector } from '@/interfaces/database/knowledge';`

## Code Structure Analysis

- Total lines: 325
- Blank lines: 18 (5.5%)
- Comment lines: ~25 (7.7%)
- Code lines: ~282


## Dependencies and Imports

- `@/components/data-pipeline-select`
- `@/components/parse-configuration/graph-rag-form-fields`
- `@/components/parse-configuration/raptor-form-fields`
- `@/components/ui/button`
- `@/components/ui/divider`
- `@/components/ui/form`
- `@/constants/form`
- `@/constants/knowledge`
- `@/constants/permission`
- `@/interfaces/database/knowledge`
- `@/pages/user-setting/data-source/contant`
- `@/pages/user-setting/data-source/interface`
- `@hookform/resolvers/zod`
- `react`
- `react-hook-form`
- `react-i18next`
- `zod`
- `../dataset-title`
- `./chunk-method-form`
- `./chunk-method-learn-more`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting`.

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

- Other files in `web/src/pages/dataset/dataset-setting/` directory
- Potential test file: `test_index.tsx`

## Keywords

../dataset-title, ./chunk-method-form, ./chunk-method-learn-more, ./configuration-form-container, ./configuration/common-item, ./form-schema, ./general-form, ./hooks, ./saving-button, @/components/data-pipeline-select, @/components/parse-configuration/graph-rag-form-fields, @/components/parse-configuration/raptor-form-fields, @/components/ui/button, @/components/ui/divider, @/components/ui/form, @/constants/form, @/constants/knowledge, @/constants/permission, @/interfaces/database/knowledge, @/pages/user-setting/data-source/contant, @/pages/user-setting/data-source/interface, @hookform/resolvers/zod, Button, ChunkMethodForm, ChunkMethodItem, ChunkMethodLearnMore, DataFlowSelect, DataSourceInfo, DatasetSettings, DeepDOC, Divider, DocumentParserType, DocumentType, Form, FormLayout, General, GeneralForm, GenerateType, GraphRagItems, Horizontal, IConnector, IDataPipelineNodeProps, IDataPipelineSelectNode, IDataSourceBase, IDataSourceNodeProps, IGenerateLogButtonProps, KnowledgeGraph, Light, LinkDataPipeline, LinkDataSource...

---
*Generated by RAGFlow Repository Documentation Generator*
