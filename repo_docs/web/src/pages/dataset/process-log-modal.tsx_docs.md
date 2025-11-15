# File Documentation: web/src/pages/dataset/process-log-modal.tsx

## File Metadata

- **Path**: `web/src/pages/dataset/process-log-modal.tsx`
- **Extension**: `.tsx`
- **Lines**: 156
- **Characters**: 4,374
- **Size**: 4,374 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import FileStatusBadge from '@/components/file-status-badge';
import { Button } from '@/components/ui/button';
import { Modal } from '@/components/ui/modal/modal';
import { RunningStatusMap } from '@/constants/knowledge';
import { useTranslate } from '@/hooks/common-hooks';
import React, { useMemo } from 'react';
import reactStringReplace from 'react-string-replace';
import { RunningStatus } from './dataset/constant';
export interface ILogInfo {
  fileType?: string;
  uploadedBy?: string;
  uploadDate?: string;
  processBeginAt?: string;
  chunkNumber?: number;

  taskId?: string;
  fileName: string;
  fileSize?: string;
  source?: string;
  task?: string;
  status?: RunningStatus;
  startTime?: string;
  endTime?: string;
  duration?: string;
  details: string;
}

interface ProcessLogModalProps {
  visible: boolean;
  onCancel: () => void;
  logInfo: ILogInfo;
  title: string;
}

const InfoItem: React.FC<{
  label: string;
  value: string | React.ReactNode;
  className?: string;
}> = ({ label, value, className = '' }) => {
  return (
    <div className={`flex flex-col mb-4 ${className}`}>
      <span className="text-text-secondary text-sm">{label}</span>
      <span className="text-text-primary mt-1">{value}</span>
    </div>
  );
};
export const replaceText = (text: string) => {
  // Remove duplicate \n
  const nextText = text.replace(/(\n)\1+/g, '$1');

  const replacedText = reactStringReplace(
    nextText,
    /(\[ERROR\].+\s)/g,
    (match, i) => {
      return (
        <span key={i} className={'text-red-600'}>
          {match}
        </span>
      );
    },
  );

  return replacedText;
};
const ProcessLogModal: React.FC<ProcessLogModalProps> = ({
  visible,
  onCancel,
  logInfo: initData,
  title,
}) => {
  const { t } = useTranslate('knowledgeDetails');
  const blackKeyList = [''];
  console.log('logInfo', initData);
  const logInfo = useMemo(() => {
    console.log('logInfo', initData);
    return initData;
  }, [initData]);

  return (
    <Modal
      title={title || 'log'}
      open={visible}
      onCancel={onCancel}
      footer={
        <div className="flex justify-end">
          <Button onClick={onCancel}>{t('close')}</Button>
        </div>
      }
      className="process-log-modal"
    >
      <div className=" rounded-lg">
        <div className="flex flex-wrap ">
          {Object?.keys(logInfo).map((key) => {
            if (
              blackKeyList.includes(key) ||
              !logInfo[key as keyof typeof logInfo]
            ) {
              return null;
            }
            if (key === 'details') {
              return (
                <div className="w-full  mt-2" key={key}>
                  <InfoItem
                    label={t(key)}
                    value={
                      <div className="w-full  whitespace-pre-line text-wrap bg-bg-card rounded-lg h-fit max-h-[350px] overflow-y-auto scrollbar-auto p-2.5">
                        {replaceText(logInfo.details)}
                      </div>
                    }
                  />
                </div>
              );
            }
            if (key === 'status') {
              return (
                <div className="flex flex-col w-1/2" key={key}>
                  <span className="text-text-secondary text-sm">
                    {t('status')}
                  </span>
                  <div className="mt-1">
                    <FileStatusBadge
                      status={logInfo.status as RunningStatus}
                      name={RunningStatusMap[logInfo.status as RunningStatus]}
                    />
                  </div>
                </div>
              );
            }
            return (
              <div className="w-1/2" key={key}>
                <InfoItem
                  label={t(key)}
                  value={logInfo[key as keyof typeof logInfo]}
                />
              </div>
            );
          })}
        </div>
        {/* <InfoItem label="Details" value={logInfo.details} /> */}
        {/* <div>
          <div>Details</div>
          <div>
            <ul className="space-y-2">
              <div className={'w-full  whitespace-pre-line text-wrap '}>
                {replaceText(logInfo.details)}
              </div>
            </ul>
          </div>
        </div> */}
      </div>
    </Modal>
  );
};

export default ProcessLogModal;

```

## High-Level Overview

  // Remove duplicate \n

## Detailed Walkthrough

### Exports (1)

- `replaceText`: Exported entity

### Functions (3)

- `replaceText()`: Function definition
- `replacedText()`: Function definition
- `logInfo()`: Function definition

### Imports (8)

- `import FileStatusBadge from '@/components/file-status-badge';`
- `import { Button } from '@/components/ui/button';`
- `import { Modal } from '@/components/ui/modal/modal';`
- `import { RunningStatusMap } from '@/constants/knowledge';`
- `import { useTranslate } from '@/hooks/common-hooks';`
- `import React, { useMemo } from 'react';`
- `import reactStringReplace from 'react-string-replace';`
- `import { RunningStatus } from './dataset/constant';`

## Code Structure Analysis

- Total lines: 156
- Blank lines: 8 (5.1%)
- Comment lines: ~1 (0.6%)
- Code lines: ~147


## Dependencies and Imports

- `@/components/file-status-badge`
- `@/components/ui/button`
- `@/components/ui/modal/modal`
- `@/constants/knowledge`
- `@/hooks/common-hooks`
- `react`
- `react-string-replace`
- `./dataset/constant`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataset`.

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

- Other files in `web/src/pages/dataset/` directory
- Potential test file: `test_process-log-modal.tsx`

## Keywords

./dataset/constant, @/components/file-status-badge, @/components/ui/button, @/components/ui/modal/modal, @/constants/knowledge, @/hooks/common-hooks, Button, Details, ERROR, FileStatusBadge, ILogInfo, InfoItem, Modal, Object, ProcessLogModal, ProcessLogModalProps, React, ReactNode, Remove, RunningStatus, RunningStatusMap, TypeScript, blackKeyList, logInfo, nextText, react, react-string-replace, replaceText, replacedText

---
*Generated by RAGFlow Repository Documentation Generator*
