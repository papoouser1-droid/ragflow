# File Documentation: web/src/pages/dataset/dataset-setting/components/link-data-pipeline.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/link-data-pipeline.tsx`
- **Extension**: `.tsx`
- **Lines**: 192
- **Characters**: 5,898
- **Size**: 5,898 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { IDataPipelineSelectNode } from '@/components/data-pipeline-select';
import { IconFont } from '@/components/icon-font';
import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import { Button } from '@/components/ui/button';
import { Modal } from '@/components/ui/modal/modal';
import { Link } from 'lucide-react';
import { useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import LinkDataPipelineModal from './link-data-pipline-modal';
export interface IDataPipelineNodeProps extends IDataPipelineSelectNode {
  isDefault?: boolean;
  linked?: boolean;
}

export interface ILinkDataPipelineProps {
  data?: IDataPipelineNodeProps;
  handleLinkOrEditSubmit?: (data: IDataPipelineNodeProps | undefined) => void;
}

interface DataPipelineItemProps extends IDataPipelineNodeProps {
  openLinkModalFunc?: (open: boolean, data?: IDataPipelineNodeProps) => void;
}

const DataPipelineItem = (props: DataPipelineItemProps) => {
  const { t } = useTranslation();
  const { name, avatar, isDefault, linked, openLinkModalFunc } = props;
  const openUnlinkModal = () => {
    Modal.show({
      visible: true,
      className: '!w-[560px]',
      title: t('dataflowParser.unlinkPipelineModalTitle'),
      children: (
        <div
          className="text-sm text-text-secondary"
          dangerouslySetInnerHTML={{
            __html: t('dataflowParser.unlinkPipelineModalContent'),
          }}
        ></div>
      ),
      onVisibleChange: () => {
        Modal.hide();
      },
      footer: (
        <div className="flex justify-end gap-2">
          <Button variant={'outline'} onClick={() => Modal.hide()}>
            {t('dataflowParser.changeStepModalCancelText')}
          </Button>
          <Button
            variant={'secondary'}
            className="!bg-state-error text-bg-base"
            onClick={() => {
              Modal.hide();
            }}
          >
            {t('dataflowParser.unlinkPipelineModalConfirmText')}
          </Button>
        </div>
      ),
    });
  };

  return (
    <div className="flex items-center justify-between gap-1 px-2 rounded-md border">
      <div className="flex items-center gap-1">
        <RAGFlowAvatar avatar={avatar} name={name} className="size-4" />
        <div>{name}</div>
        {/* {isDefault && (
          <div className="text-xs bg-text-secondary text-bg-base px-2 py-1 rounded-md">
            {t('knowledgeConfiguration.default')}
          </div>
        )} */}
      </div>
      {/* <div className="flex gap-1 items-center">
        <Button
          variant={'transparent'}
          className="border-none"
          type="button"
          onClick={() =>
            openLinkModalFunc?.(true, { ...omit(props, ['openLinkModalFunc']) })
          }
        >
          <Settings2 />
        </Button>
        {!isDefault && (
          <>
            {linked && (
              <Button
                type="button"
                variant={'transparent'}
                className="border-none"
                onClick={() => {
                  openUnlinkModal();
                }}
              >
                <Unlink />
              </Button>
            )}
          </>
        )}
      </div> */}
    </div>
  );
};

const LinkDataPipeline = (props: ILinkDataPipelineProps) => {
  const { data, handleLinkOrEditSubmit: submit } = props;
  const { t } = useTranslation();
  const [openLinkModal, setOpenLinkModal] = useState(false);
  const [currentDataPipeline, setCurrentDataPipeline] =
    useState<IDataPipelineNodeProps>();
  const pipelineNode: IDataPipelineNodeProps[] = useMemo(
    () => [
      {
        id: data?.id,
        name: data?.name,
        avatar: data?.avatar,
        isDefault: data?.isDefault,
        linked: true,
      },
    ],
    [data],
  );
  const openLinkModalFunc = (open: boolean, data?: IDataPipelineNodeProps) => {
    console.log('open', open, data);
    setOpenLinkModal(open);
    if (data) {
      setCurrentDataPipeline(data);
    } else {
      setCurrentDataPipeline(undefined);
    }
  };
  const handleLinkOrEditSubmit = (
    data: IDataPipelineSelectNode | undefined,
  ) => {
    console.log('handleLinkOrEditSubmit', data);
    submit?.(data);
    setOpenLinkModal(false);
  };
  return (
    <div className="flex flex-col gap-2">
      <section className="flex flex-col">
        <div className="flex items-center gap-1 text-text-primary text-sm">
          <IconFont name="Pipeline" />
          {t('knowledgeConfiguration.dataPipeline')}
        </div>
        <div className="flex justify-between items-center">
          <div className="text-center text-xs text-text-secondary">
            {t('knowledgeConfiguration.linkPipelineSetTip')}
          </div>
          <Button
            type="button"
            variant={'transparent'}
            onClick={() => {
              openLinkModalFunc?.(true);
            }}
          >
            <Link />
            <span className="text-xs text-text-primary">
              {t('knowledgeConfiguration.linkDataPipeline')}
            </span>
          </Button>
        </div>
      </section>
      <section className="flex flex-col gap-2">
        {pipelineNode.map(
          (item) =>
            item.id && (
              <DataPipelineItem
                key={item.id}
                openLinkModalFunc={openLinkModalFunc}
                id={item.id}
                name={item.name}
                avatar={item.avatar}
                isDefault={item.isDefault}
                linked={item.linked}
              />
            ),
        )}
      </section>
      <LinkDataPipelineModal
        data={currentDataPipeline}
        open={openLinkModal}
        setOpen={(open: boolean) => {
          openLinkModalFunc(open);
        }}
        onSubmit={handleLinkOrEditSubmit}
      />
    </div>
  );
};
export default LinkDataPipeline;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-setting/components/link-data-pipeline.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 192 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `DataPipelineItem()`: Function definition
- `openUnlinkModal()`: Function definition
- `LinkDataPipeline()`: Function definition
- `openLinkModalFunc()`: Function definition
- `handleLinkOrEditSubmit()`: Function definition

### Imports (9)

- `import { IDataPipelineSelectNode } from '@/components/data-pipeline-select';`
- `import { IconFont } from '@/components/icon-font';`
- `import { RAGFlowAvatar } from '@/components/ragflow-avatar';`
- `import { Button } from '@/components/ui/button';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { Link } from 'lucide-react';`
- `import { useMemo, useState } from 'react';`
- `import { useTranslation } from 'react-i18next';`
- `import LinkDataPipelineModal from './link-data-pipline-modal';`

## Code Structure Analysis

- Total lines: 192
- Blank lines: 6 (3.1%)
- Comment lines: ~0 (0.0%)
- Code lines: ~186


## Dependencies and Imports

- `@/components/data-pipeline-select`
- `@/components/icon-font`
- `@/components/ragflow-avatar`
- `@/components/ui/button`
- `@/components/ui/modal/modal`
- `lucide-react`
- `react`
- `react-i18next`
- `./link-data-pipline-modal`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset-setting/components`.

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

- Other files in `web/src/pages/dataset/dataset-setting/components/` directory
- Potential test file: `test_link-data-pipeline.tsx`

## Keywords

./link-data-pipline-modal, @/components/data-pipeline-select, @/components/icon-font, @/components/ragflow-avatar, @/components/ui/button, @/components/ui/modal/modal, Button, DataPipelineItem, DataPipelineItemProps, IDataPipelineNodeProps, IDataPipelineSelectNode, ILinkDataPipelineProps, IconFont, Link, LinkDataPipeline, LinkDataPipelineModal, Modal, Pipeline, RAGFlowAvatar, Settings2, TypeScript, Unlink, handleLinkOrEditSubmit, lucide-react, openLinkModalFunc, openUnlinkModal, pipelineNode, react, react-i18next

---
*Generated by RAGFlow Repository Documentation Generator*
