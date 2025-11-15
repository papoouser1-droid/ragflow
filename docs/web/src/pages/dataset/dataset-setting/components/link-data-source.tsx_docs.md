# Documentation: web/src/pages/dataset/dataset-setting/components/link-data-source.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/link-data-source.tsx`
- **Size**: 7079 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataset/dataset-setting/components/link-data-source.tsx`.

## Original Source Code

```tsx
import { IconFontFill } from '@/components/icon-font';
import { Button } from '@/components/ui/button';
import { Switch } from '@/components/ui/switch';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';
import { IConnector } from '@/interfaces/database/knowledge';
import { delSourceModal } from '@/pages/user-setting/data-source/component/delete-source-modal';
import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';
import { useDataSourceRebuild } from '@/pages/user-setting/data-source/hooks';
import { IDataSourceBase } from '@/pages/user-setting/data-source/interface';
import { Link, Settings, Unlink } from 'lucide-react';
import { useMemo, useState } from 'react';
import { useTranslation } from 'react-i18next';
import LinkDataSourceModal from './link-data-source-modal';

export type IDataSourceNodeProps = IConnector & {
  icon: React.ReactNode;
};

export interface ILinkDataSourceProps {
  data?: IConnector[];
  handleLinkOrEditSubmit?: (data: IDataSourceBase[] | undefined) => void;
  unbindFunc?: (item: DataSourceItemProps) => void;
  handleAutoParse?: (option: {
    source_id: string;
    isAutoParse: boolean;
  }) => void;
}

interface DataSourceItemProps extends IDataSourceNodeProps {
  openLinkModalFunc?: (open: boolean, data?: IDataSourceNodeProps) => void;
  unbindFunc?: (item: DataSourceItemProps) => void;
  handleAutoParse?: (option: {
    source_id: string;
    isAutoParse: boolean;
  }) => void;
}

const DataSourceItem = (props: DataSourceItemProps) => {
  const { t } = useTranslation();
  const { id, name, icon, source, auto_parse, unbindFunc, handleAutoParse } =
    props;

  const { navigateToDataSourceDetail } = useNavigatePage();
  const { handleRebuild } = useDataSourceRebuild();
  const toDetail = (id: string) => {
    navigateToDataSourceDetail(id);
  };

  return (
    <div className="flex items-center justify-between gap-1 px-2 h-10 rounded-md border group hover:bg-bg-card">
      <div className="flex items-center gap-1">
        <div className="w-6 h-6 flex-shrink-0">{icon}</div>
        <div className="text-base text-text-primary">
          {DataSourceInfo[source].name}
        </div>
        <div>{name}</div>
      </div>
      <div className="flex items-center ">
        <div className="items-center gap-1 hidden mr-5 group-hover:flex">
          <div className="text-xs text-text-secondary">
            {t('knowledgeConfiguration.autoParse')}
          </div>
          <Switch
            checked={auto_parse === '1'}
            onCheckedChange={(isAutoParse) => {
              handleAutoParse?.({ source_id: id, isAutoParse });
            }}
            className="w-8 h-4"
          />
        </div>
        <Tooltip>
          <TooltipTrigger>
            <Button
              variant={'transparent'}
              className="border-none hidden group-hover:block"
              type="button"
              onClick={() => {
                handleRebuild({ source_id: id });
              }}
            >
              {/* <Settings /> */}
              <IconFontFill name="reparse" className="text-text-primary" />
            </Button>
          </TooltipTrigger>
          <TooltipContent>
            {t('knowledgeConfiguration.rebuildTip')}
          </TooltipContent>
        </Tooltip>
        <Button
          variant={'transparent'}
          className="border-none hidden group-hover:block"
          type="button"
          onClick={() => {
            toDetail(id);
          }}
          // onClick={() =>
          //   openLinkModalFunc?.(true, { ...omit(props, ['openLinkModalFunc']) })
          // }
        >
          <Settings />
        </Button>
        <>
          <Button
            type="button"
            variant={'transparent'}
            className="border-none hidden group-hover:block"
            onClick={() => {
              // openUnlinkModal();
              delSourceModal({
                data: props,
                type: 'unlink',
                onOk: (data) => unbindFunc?.(data as DataSourceItemProps),
              });
            }}
          >
            <Unlink />
          </Button>
        </>
      </div>
    </div>
  );
};

const LinkDataSource = (props: ILinkDataSourceProps) => {
  const {
    data,
    handleLinkOrEditSubmit: submit,
    unbindFunc,
    handleAutoParse,
  } = props;
  const { t } = useTranslation();
  const [openLinkModal, setOpenLinkModal] = useState(false);

  const pipelineNode: IDataSourceNodeProps[] = useMemo(() => {
    if (data && data.length > 0) {
      return data.map((item) => {
        return {
          ...item,
          id: item?.id,
          name: item?.name,
          icon:
            DataSourceInfo[item?.source as keyof typeof DataSourceInfo]?.icon ||
            '',
        } as IDataSourceNodeProps;
      });
    }
    return [];
  }, [data]);

  const openLinkModalFunc = (open: boolean, data?: IDataSourceNodeProps) => {
    console.log('open', open, data);
    setOpenLinkModal(open);
    // if (data) {
    //   setCurrentDataSource(data);
    // } else {
    //   setCurrentDataSource(undefined);
    // }
  };

  const handleLinkOrEditSubmit = (data: IDataSourceBase[] | undefined) => {
    console.log('handleLinkOrEditSubmit', data);
    submit?.(data);
    setOpenLinkModal(false);
  };

  return (
    <div className="flex flex-col gap-2">
      <section className="flex flex-col">
        <div className="text-base font-medium text-text-primary">
          {t('knowledgeConfiguration.dataSource')}
        </div>
        {/* <div className="flex items-center gap-1 text-text-primary text-sm">
          {t('knowledgeConfiguration.dataSource')}
        </div> */}
        <div className="flex justify-between items-center">
          <div className="text-center text-xs text-text-secondary">
            {t('knowledgeConfiguration.linkSourceSetTip')}
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
              {t('knowledgeConfiguration.linkDataSource')}
            </span>
          </Button>
        </div>
      </section>
      <section className="flex flex-col gap-2">
        {pipelineNode.map(
          (item) =>
            item.id && (
              <DataSourceItem
                key={item.id}
                openLinkModalFunc={openLinkModalFunc}
                unbindFunc={unbindFunc}
                handleAutoParse={handleAutoParse}
                {...item}
              />
            ),
        )}
      </section>
      <LinkDataSourceModal
        selectedList={data as IConnector[]}
        open={openLinkModal}
        setOpen={(open: boolean) => {
          openLinkModalFunc(open);
        }}
        onSubmit={handleLinkOrEditSubmit}
      />
    </div>
  );
};
export default LinkDataSource;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataset/dataset-setting/components/link-data-source.tsx` is located in the `web/src/pages/dataset/dataset-setting/components` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to components.

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

- [added-source-card.tsx](added-source-card.tsx_docs.md)
- [link-data-pipeline.tsx](link-data-pipeline.tsx_docs.md)
- [link-data-pipline-modal.tsx](link-data-pipline-modal.tsx_docs.md)
- [link-data-source-modal.tsx](link-data-source-modal.tsx_docs.md)
- [tag-item.tsx](tag-item.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
