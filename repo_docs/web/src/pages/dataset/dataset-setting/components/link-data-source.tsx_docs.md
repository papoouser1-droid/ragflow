# File Documentation: web/src/pages/dataset/dataset-setting/components/link-data-source.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset-setting/components/link-data-source.tsx`
- **Extension**: `.tsx`
- **Lines**: 224
- **Characters**: 7,079
- **Size**: 7,079 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset-setting/components/link-data-source.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 224 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `DataSourceItem()`: Function definition
- `toDetail()`: Function definition
- `LinkDataSource()`: Function definition
- `openLinkModalFunc()`: Function definition
- `handleLinkOrEditSubmit()`: Function definition

### Imports (14)

- `import { IconFontFill } from '@/components/icon-font';`
- `import { Button } from '@/components/ui/button';`
- `import { Switch } from '@/components/ui/switch';`
- `import {`
- `import { useNavigatePage } from '@/hooks/logic-hooks/navigate-hooks';`
- `import { IConnector } from '@/interfaces/database/knowledge';`
- `import { delSourceModal } from '@/pages/user-setting/data-source/component/delete-source-modal';`
- `import { DataSourceInfo } from '@/pages/user-setting/data-source/contant';`
- `import { useDataSourceRebuild } from '@/pages/user-setting/data-source/hooks';`
- `import { IDataSourceBase } from '@/pages/user-setting/data-source/interface';`

## Code Structure Analysis

- Total lines: 224
- Blank lines: 12 (5.4%)
- Comment lines: ~9 (4.0%)
- Code lines: ~203


## Dependencies and Imports

- `@/components/icon-font`
- `@/components/ui/button`
- `@/components/ui/switch`
- `@/hooks/logic-hooks/navigate-hooks`
- `@/interfaces/database/knowledge`
- `@/pages/user-setting/data-source/component/delete-source-modal`
- `@/pages/user-setting/data-source/contant`
- `@/pages/user-setting/data-source/hooks`
- `@/pages/user-setting/data-source/interface`
- `lucide-react`
- `react`
- `react-i18next`
- `./link-data-source-modal`

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
- Potential test file: `test_link-data-source.tsx`

## Keywords

./link-data-source-modal, @/components/icon-font, @/components/ui/button, @/components/ui/switch, @/hooks/logic-hooks/navigate-hooks, @/interfaces/database/knowledge, @/pages/user-setting/data-source/component/delete-source-modal, @/pages/user-setting/data-source/contant, @/pages/user-setting/data-source/hooks, @/pages/user-setting/data-source/interface, Button, DataSourceInfo, DataSourceItem, DataSourceItemProps, IConnector, IDataSourceBase, IDataSourceNodeProps, ILinkDataSourceProps, IconFontFill, Link, LinkDataSource, LinkDataSourceModal, React, ReactNode, Settings, Switch, Tooltip, TooltipContent, TooltipTrigger, TypeScript, Unlink, handleLinkOrEditSubmit, lucide-react, openLinkModalFunc, pipelineNode, react, react-i18next, toDetail

---
*Generated by RAGFlow Repository Documentation Generator*
