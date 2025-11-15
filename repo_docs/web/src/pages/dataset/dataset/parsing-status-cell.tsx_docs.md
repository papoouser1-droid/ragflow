# File Documentation: web/src/pages/dataset/dataset/parsing-status-cell.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/dataset/parsing-status-cell.tsx`
- **Extension**: `.tsx`
- **Lines**: 181
- **Characters**: 5,838
- **Size**: 5,838 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';
import { IconFontFill } from '@/components/icon-font';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import { Progress } from '@/components/ui/progress';
import { Separator } from '@/components/ui/separator';
import {
  Tooltip,
  TooltipContent,
  TooltipTrigger,
} from '@/components/ui/tooltip';
import { IDocumentInfo } from '@/interfaces/database/document';
import { CircleX } from 'lucide-react';
import { useCallback, useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { DocumentType, RunningStatus } from './constant';
import { ParsingCard } from './parsing-card';
import { UseChangeDocumentParserShowType } from './use-change-document-parser';
import { useHandleRunDocumentByIds } from './use-run-document';
import { UseSaveMetaShowType } from './use-save-meta';
import { isParserRunning } from './utils';
const IconMap = {
  [RunningStatus.UNSTART]: (
    <IconFontFill name="play" className="text-accent-primary" />
  ),
  [RunningStatus.RUNNING]: (
    <CircleX size={14} color="rgba(var(--state-error))" />
  ),
  [RunningStatus.CANCEL]: (
    <IconFontFill name="reparse" className="text-accent-primary" />
  ),
  [RunningStatus.DONE]: (
    <IconFontFill name="reparse" className="text-accent-primary" />
  ),
  [RunningStatus.FAIL]: (
    <IconFontFill name="reparse" className="text-accent-primary" />
  ),
};

export function ParsingStatusCell({
  record,
  showChangeParserModal,
  showSetMetaModal,
  showLog,
}: {
  record: IDocumentInfo;
  showLog: (record: IDocumentInfo) => void;
} & UseChangeDocumentParserShowType &
  UseSaveMetaShowType) {
  const { t } = useTranslation();
  const {
    run,
    parser_id,
    pipeline_id,
    pipeline_name,
    progress,
    chunk_num,
    id,
  } = record;
  const operationIcon = IconMap[run];
  const p = Number((progress * 100).toFixed(2));
  const { handleRunDocumentByIds } = useHandleRunDocumentByIds(id);
  const isRunning = isParserRunning(run);
  const isZeroChunk = chunk_num === 0;

  const handleOperationIconClick =
    (shouldDelete: boolean = false) =>
    () => {
      handleRunDocumentByIds(record.id, isRunning, shouldDelete);
    };

  const handleShowChangeParserModal = useCallback(() => {
    showChangeParserModal(record);
  }, [record, showChangeParserModal]);

  const handleShowSetMetaModal = useCallback(() => {
    showSetMetaModal(record);
  }, [record, showSetMetaModal]);

  const showParse = useMemo(() => {
    return record.type !== DocumentType.Virtual;
  }, [record]);

  const handleShowLog = (record: IDocumentInfo) => {
    showLog(record);
  };
  return (
    <section className="flex gap-8 items-center">
      <div className="text-ellipsis w-[100px] flex items-center justify-between">
        <DropdownMenu>
          <DropdownMenuTrigger>
            <Tooltip>
              <TooltipTrigger asChild>
                <div className="border-none truncate max-w-32 cursor-pointer px-2 py-1 rounded-sm hover:bg-bg-card">
                  {pipeline_id
                    ? pipeline_name || pipeline_id
                    : parser_id === 'naive'
                      ? 'general'
                      : parser_id}
                </div>
              </TooltipTrigger>
              <TooltipContent>
                <p>
                  {pipeline_id
                    ? pipeline_name || pipeline_id
                    : parser_id === 'naive'
                      ? 'general'
                      : parser_id}
                </p>
              </TooltipContent>
            </Tooltip>
          </DropdownMenuTrigger>
          <DropdownMenuContent>
            <DropdownMenuItem onClick={handleShowChangeParserModal}>
              {t('knowledgeDetails.dataPipeline')}
            </DropdownMenuItem>
            <DropdownMenuItem onClick={handleShowSetMetaModal}>
              {t('knowledgeDetails.setMetaData')}
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      </div>

      {showParse && (
        <div className="flex items-center gap-3">
          <Separator orientation="vertical" className="h-2.5" />
          {!isParserRunning(run) && (
            <ConfirmDeleteDialog
              title={t(`knowledgeDetails.redo`, { chunkNum: chunk_num })}
              hidden={isZeroChunk || isRunning}
              onOk={handleOperationIconClick(true)}
              onCancel={handleOperationIconClick(false)}
            >
              <div
                className="cursor-pointer flex items-center gap-3"
                onClick={
                  isZeroChunk || isRunning
                    ? handleOperationIconClick(false)
                    : () => {}
                }
              >
                {!isParserRunning(run) && operationIcon}
              </div>
            </ConfirmDeleteDialog>
          )}
          {isParserRunning(run) ? (
            <>
              <div
                className="flex items-center gap-1 cursor-pointer"
                onClick={() => handleShowLog(record)}
              >
                <Progress value={p} className="h-1 flex-1 min-w-10" />
                {p}%
              </div>
              <div
                className="cursor-pointer flex items-center gap-3"
                onClick={
                  isZeroChunk || isRunning
                    ? handleOperationIconClick(false)
                    : () => {}
                }
              >
                {operationIcon}
              </div>
            </>
          ) : (
            <ParsingCard
              record={record}
              handleShowLog={handleShowLog}
            ></ParsingCard>
          )}
        </div>
      )}
    </section>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataset/dataset/parsing-status-cell.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 181 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `ParsingStatusCell`: Exported entity

### Functions (6)

- `ParsingStatusCell()`: Function definition
- `handleOperationIconClick()`: Function definition
- `handleShowChangeParserModal()`: Function definition
- `handleShowSetMetaModal()`: Function definition
- `showParse()`: Function definition
- `handleShowLog()`: Function definition

### Imports (16)

- `import { ConfirmDeleteDialog } from '@/components/confirm-delete-dialog';`
- `import { IconFontFill } from '@/components/icon-font';`
- `import {`
- `import { Progress } from '@/components/ui/progress';`
- `import { Separator } from '@/components/ui/separator';`
- `import {`
- `import { IDocumentInfo } from '@/interfaces/database/document';`
- `import { CircleX } from 'lucide-react';`
- `import { useCallback, useMemo } from 'react';`
- `import { useTranslation } from 'react-i18next';`

## Code Structure Analysis

- Total lines: 181
- Blank lines: 8 (4.4%)
- Comment lines: ~0 (0.0%)
- Code lines: ~173


## Dependencies and Imports

- `@/components/confirm-delete-dialog`
- `@/components/icon-font`
- `@/components/ui/progress`
- `@/components/ui/separator`
- `@/interfaces/database/document`
- `lucide-react`
- `react`
- `react-i18next`
- `./constant`
- `./parsing-card`
- `./use-change-document-parser`
- `./use-run-document`
- `./use-save-meta`
- `./utils`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset/dataset`.

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

- Other files in `web/src/pages/dataset/dataset/` directory
- Potential test file: `test_parsing-status-cell.tsx`

## Keywords

./constant, ./parsing-card, ./use-change-document-parser, ./use-run-document, ./use-save-meta, ./utils, @/components/confirm-delete-dialog, @/components/icon-font, @/components/ui/progress, @/components/ui/separator, @/interfaces/database/document, CANCEL, CircleX, ConfirmDeleteDialog, DONE, DocumentType, DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuTrigger, FAIL, IDocumentInfo, IconFontFill, IconMap, Number, ParsingCard, ParsingStatusCell, Progress, RUNNING, RunningStatus, Separator, Tooltip, TooltipContent, TooltipTrigger, TypeScript, UNSTART, UseChangeDocumentParserShowType, UseSaveMetaShowType, Virtual, handleOperationIconClick, handleShowChangeParserModal, handleShowLog, handleShowSetMetaModal, isRunning, isZeroChunk, lucide-react, operationIcon, p, react, react-i18next...

---
*Generated by RAGFlow Repository Documentation Generator*
