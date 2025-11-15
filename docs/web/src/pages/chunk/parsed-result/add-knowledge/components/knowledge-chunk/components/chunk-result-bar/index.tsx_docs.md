# Documentation: web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-result-bar/index.tsx

## File Metadata

- **Path**: `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-result-bar/index.tsx`
- **Size**: 3450 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-result-bar/index.tsx`.

## Original Source Code

```tsx
import { Input } from '@/components/originui/input';
import { Button } from '@/components/ui/button';
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import { Radio } from '@/components/ui/radio';
import { useTranslate } from '@/hooks/common-hooks';
import { cn } from '@/lib/utils';
import { SearchOutlined } from '@ant-design/icons';
import { ListFilter, Plus } from 'lucide-react';
import { useState } from 'react';
import { ChunkTextMode } from '../../constant';
interface ChunkResultBarProps {
  changeChunkTextMode: React.Dispatch<React.SetStateAction<string | number>>;
  available: number | undefined;
  selectAllChunk: (value: boolean) => void;
  handleSetAvailable: (value: number | undefined) => void;
  createChunk: () => void;
  handleInputChange: (e: React.ChangeEvent<HTMLInputElement>) => void;
  searchString: string;
}
export default ({
  changeChunkTextMode,
  available,
  selectAllChunk,
  handleSetAvailable,
  createChunk,
  handleInputChange,
  searchString,
}: ChunkResultBarProps) => {
  const { t } = useTranslate('chunk');
  const [textSelectValue, setTextSelectValue] = useState<string | number>(
    ChunkTextMode.Full,
  );
  const handleFilterChange = (e: string | number) => {
    const value = e === -1 ? undefined : (e as number);
    selectAllChunk(false);
    handleSetAvailable(value);
  };
  const filterContent = (
    <div className="w-[200px]">
      <Radio.Group onChange={handleFilterChange} value={available}>
        <div className="flex flex-col gap-2 p-4">
          <Radio value={-1}>{t('all')}</Radio>
          <Radio value={1}>{t('enabled')}</Radio>
          <Radio value={0}>{t('disabled')}</Radio>
        </div>
      </Radio.Group>
    </div>
  );
  const textSelectOptions = [
    { label: t(ChunkTextMode.Full), value: ChunkTextMode.Full },
    { label: t(ChunkTextMode.Ellipse), value: ChunkTextMode.Ellipse },
  ];

  const changeTextSelectValue = (value: string | number) => {
    setTextSelectValue(value);
    changeChunkTextMode(value);
  };
  return (
    <div className="flex pr-[25px]">
      <div className="flex items-center gap-4 bg-bg-card text-muted-foreground w-fit h-[35px] rounded-md px-4 py-2">
        {textSelectOptions.map((option) => (
          <div
            key={option.value}
            className={cn('flex items-center cursor-pointer', {
              'text-primary': option.value === textSelectValue,
            })}
            onClick={() => changeTextSelectValue(option.value)}
          >
            {option.label}
          </div>
        ))}
      </div>
      <div className="ml-auto"></div>
      <Input
        className="bg-bg-card text-muted-foreground"
        style={{ width: 200 }}
        placeholder={t('search')}
        icon={<SearchOutlined />}
        onChange={handleInputChange}
        value={searchString}
      />
      <div className="w-[20px]"></div>
      <Popover>
        <PopoverTrigger asChild>
          <Button className="bg-bg-card text-muted-foreground hover:bg-card">
            <ListFilter />
          </Button>
        </PopoverTrigger>
        <PopoverContent className="p-0 w-[200px]">
          {filterContent}
        </PopoverContent>
      </Popover>
      <div className="w-[20px]"></div>
      <Button
        onClick={() => createChunk()}
        className="bg-bg-card text-primary hover:bg-card"
      >
        <Plus size={44} />
      </Button>
    </div>
  );
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-result-bar/index.tsx` is located in the `web/src/pages/chunk/parsed-result/add-knowledge/components/knowledge-chunk/components/chunk-result-bar` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to chunk-result-bar.

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

- [checkbox-sets.tsx](checkbox-sets.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
