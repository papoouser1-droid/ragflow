# Documentation: web/src/pages/dataflow-result/components/chunk-card/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/chunk-card/index.tsx`
- **Size**: 3590 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/dataflow-result/components/chunk-card/index.tsx`.

## Original Source Code

```tsx
import Image from '@/components/image';
import { useTheme } from '@/components/theme-provider';
import { Card } from '@/components/ui/card';
import { Checkbox } from '@/components/ui/checkbox';
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import { Switch } from '@/components/ui/switch';
import { IChunk } from '@/interfaces/database/knowledge';
import { CheckedState } from '@radix-ui/react-checkbox';
import classNames from 'classnames';
import DOMPurify from 'dompurify';
import { useEffect, useState } from 'react';
import { ChunkTextMode } from '../../constant';
import styles from './index.less';

interface IProps {
  item: IChunk;
  checked: boolean;
  switchChunk: (available?: number, chunkIds?: string[]) => void;
  editChunk: (chunkId: string) => void;
  handleCheckboxClick: (chunkId: string, checked: boolean) => void;
  selected: boolean;
  clickChunkCard: (chunkId: string) => void;
  textMode: ChunkTextMode;
}

const ChunkCard = ({
  item,
  checked,
  handleCheckboxClick,
  editChunk,
  switchChunk,
  selected,
  clickChunkCard,
  textMode,
}: IProps) => {
  const available = Number(item.available_int);
  const [enabled, setEnabled] = useState(false);
  const { theme } = useTheme();

  const onChange = (checked: boolean) => {
    setEnabled(checked);
    switchChunk(available === 0 ? 1 : 0, [item.chunk_id]);
  };

  const handleCheck = (e: CheckedState) => {
    handleCheckboxClick(item.chunk_id, e === 'indeterminate' ? false : e);
  };

  const handleContentDoubleClick = () => {
    editChunk(item.chunk_id);
  };

  const handleContentClick = () => {
    clickChunkCard(item.chunk_id);
  };

  useEffect(() => {
    setEnabled(available === 1);
  }, [available]);
  const [open, setOpen] = useState<boolean>(false);
  return (
    <Card
      className={classNames('rounded-lg w-full py-3 px-3', {
        'bg-bg-title': selected,
        'bg-bg-input': !selected,
      })}
    >
      <div className="flex items-start justify-between gap-2">
        <Checkbox onCheckedChange={handleCheck} checked={checked}></Checkbox>
        {item.image_id && (
          <Popover open={open}>
            <PopoverTrigger
              asChild
              onMouseEnter={() => setOpen(true)}
              onMouseLeave={() => setOpen(false)}
            >
              <div>
                <Image id={item.image_id} className={styles.image}></Image>
              </div>
            </PopoverTrigger>
            <PopoverContent
              className="p-0"
              align={'start'}
              side={'right'}
              sideOffset={-20}
            >
              <div>
                <Image
                  id={item.image_id}
                  className={styles.imagePreview}
                ></Image>
              </div>
            </PopoverContent>
          </Popover>
        )}
        <section
          onDoubleClick={handleContentDoubleClick}
          onClick={handleContentClick}
          className={styles.content}
        >
          <div
            dangerouslySetInnerHTML={{
              __html: DOMPurify.sanitize(item.content_with_weight),
            }}
            className={classNames(styles.contentText, {
              [styles.contentEllipsis]: textMode === ChunkTextMode.Ellipse,
            })}
          ></div>
        </section>
        <div>
          <Switch
            checked={enabled}
            onCheckedChange={onChange}
            aria-readonly
            className="!m-0"
          />
        </div>
      </div>
    </Card>
  );
};

export default ChunkCard;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/dataflow-result/components/chunk-card/index.tsx` is located in the `web/src/pages/dataflow-result/components/chunk-card` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-card.

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

- [index.less](index.less_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
