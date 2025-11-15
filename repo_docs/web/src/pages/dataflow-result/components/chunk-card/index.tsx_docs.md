# File Documentation: web/src/pages/dataflow-result/components/chunk-card/index.tsx

## File Metadata

- **Path**: `web/src/pages/dataflow-result/components/chunk-card/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 128
- **Characters**: 3,590
- **Size**: 3,590 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

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

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/dataflow-result/components/chunk-card/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 128 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (5)

- `ChunkCard()`: Function definition
- `onChange()`: Function definition
- `handleCheck()`: Function definition
- `handleContentDoubleClick()`: Function definition
- `handleContentClick()`: Function definition

### Imports (13)

- `import Image from '@/components/image';`
- `import { useTheme } from '@/components/theme-provider';`
- `import { Card } from '@/components/ui/card';`
- `import { Checkbox } from '@/components/ui/checkbox';`
- `import {`
- `import { Switch } from '@/components/ui/switch';`
- `import { IChunk } from '@/interfaces/database/knowledge';`
- `import { CheckedState } from '@radix-ui/react-checkbox';`
- `import classNames from 'classnames';`
- `import DOMPurify from 'dompurify';`

## Code Structure Analysis

- Total lines: 128
- Blank lines: 9 (7.0%)
- Comment lines: ~0 (0.0%)
- Code lines: ~119


## Dependencies and Imports

- `@/components/image`
- `@/components/theme-provider`
- `@/components/ui/card`
- `@/components/ui/checkbox`
- `@/components/ui/switch`
- `@/interfaces/database/knowledge`
- `@radix-ui/react-checkbox`
- `classnames`
- `dompurify`
- `react`
- `../../constant`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/dataflow-result/components/chunk-card`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/dataflow-result/components/chunk-card/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ./index.less, @/components/image, @/components/theme-provider, @/components/ui/card, @/components/ui/checkbox, @/components/ui/switch, @/interfaces/database/knowledge, @radix-ui/react-checkbox, Card, Checkbox, CheckedState, ChunkCard, ChunkTextMode, DOMPurify, Ellipse, IChunk, IProps, Image, Number, Popover, PopoverContent, PopoverTrigger, Switch, TypeScript, available, classnames, dompurify, handleCheck, handleContentClick, handleContentDoubleClick, onChange, radix, react

---
*Generated by RAGFlow Repository Documentation Generator*
