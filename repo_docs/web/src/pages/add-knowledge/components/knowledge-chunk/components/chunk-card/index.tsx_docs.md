# File Documentation: web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-card/index.tsx

## File Metadata

- **Path**: `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-card/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 102
- **Characters**: 2,770
- **Size**: 2,770 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import Image from '@/components/image';
import { IChunk } from '@/interfaces/database/knowledge';
import { Card, Checkbox, CheckboxProps, Flex, Popover, Switch } from 'antd';
import classNames from 'classnames';
import DOMPurify from 'dompurify';
import { useEffect, useState } from 'react';

import { useTheme } from '@/components/theme-provider';
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

  const handleCheck: CheckboxProps['onChange'] = (e) => {
    handleCheckboxClick(item.chunk_id, e.target.checked);
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

  return (
    <Card
      className={classNames(styles.chunkCard, {
        [`${theme === 'dark' ? styles.cardSelectedDark : styles.cardSelected}`]:
          selected,
      })}
    >
      <Flex gap={'middle'} justify={'space-between'}>
        <Checkbox onChange={handleCheck} checked={checked}></Checkbox>
        {item.image_id && (
          <Popover
            placement="right"
            content={
              <Image id={item.image_id} className={styles.imagePreview}></Image>
            }
          >
            <Image id={item.image_id} className={styles.image}></Image>
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
          <Switch checked={enabled} onChange={onChange} />
        </div>
      </Flex>
    </Card>
  );
};

export default ChunkCard;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-card/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 102 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (4)

- `ChunkCard()`: Function definition
- `onChange()`: Function definition
- `handleContentDoubleClick()`: Function definition
- `handleContentClick()`: Function definition

### Imports (9)

- `import Image from '@/components/image';`
- `import { IChunk } from '@/interfaces/database/knowledge';`
- `import { Card, Checkbox, CheckboxProps, Flex, Popover, Switch } from 'antd';`
- `import classNames from 'classnames';`
- `import DOMPurify from 'dompurify';`
- `import { useEffect, useState } from 'react';`
- `import { useTheme } from '@/components/theme-provider';`
- `import { ChunkTextMode } from '../../constant';`
- `import styles from './index.less';`

## Code Structure Analysis

- Total lines: 102
- Blank lines: 13 (12.7%)
- Comment lines: ~0 (0.0%)
- Code lines: ~89


## Dependencies and Imports

- `@/components/image`
- `@/interfaces/database/knowledge`
- `antd`
- `classnames`
- `dompurify`
- `react`
- `@/components/theme-provider`
- `../../constant`
- `./index.less`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-card`.

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

- Other files in `web/src/pages/add-knowledge/components/knowledge-chunk/components/chunk-card/` directory
- Potential test file: `test_index.tsx`

## Keywords

../../constant, ./index.less, @/components/image, @/components/theme-provider, @/interfaces/database/knowledge, Card, Checkbox, CheckboxProps, ChunkCard, ChunkTextMode, DOMPurify, Ellipse, Flex, IChunk, IProps, Image, Number, Popover, Switch, TypeScript, antd, available, classnames, dompurify, handleCheck, handleContentClick, handleContentDoubleClick, onChange, react

---
*Generated by RAGFlow Repository Documentation Generator*
