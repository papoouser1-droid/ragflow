# File Documentation: web/src/pages/profile-setting/model/model-card.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/model/model-card.tsx`
- **Extension**: `.tsx`
- **Lines**: 137
- **Characters**: 3,990
- **Size**: 3,990 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import { Key, MoreVertical, Plus, Trash2 } from 'lucide-react';
import { PropsWithChildren } from 'react';

const settings = [
  {
    title: 'GPT Model',
    description:
      'The default chat LLM all the newly created knowledgebase will use.',
    model: 'DeepseekChat',
  },
  {
    title: 'Embedding Model',
    description:
      'The default embedding model all the newly created knowledgebase will use.',
    model: 'DeepseekChat',
  },
  {
    title: 'Image Model',
    description:
      'The default multi-capable model all the newly created knowledgebase will use. It can generate a picture or video.',
    model: 'DeepseekChat',
  },
  {
    title: 'Speech2TXT Model',
    description:
      'The default ASR model all the newly created knowledgebase will use. Use this model to translate voices to text something text.',
    model: 'DeepseekChat',
  },
  {
    title: 'TTS Model',
    description:
      'The default text to speech model all the newly created knowledgebase will use.',
    model: 'DeepseekChat',
  },
];

function Title({ children }: PropsWithChildren) {
  return <span className="font-bold text-xl">{children}</span>;
}

export function SystemModelSetting() {
  return (
    <Card>
      <CardContent className="p-4 space-y-6">
        {settings.map((x, idx) => (
          <div key={idx} className="flex items-center">
            <div className="flex-1 flex flex-col">
              <span className="font-semibold text-base">{x.title}</span>
              <span className="text-colors-text-neutral-standard">
                {x.description}
              </span>
            </div>
            <div className="flex-1">
              <Select defaultValue="english">
                <SelectTrigger>
                  <SelectValue />
                </SelectTrigger>
                <SelectContent>
                  <SelectItem value="english">English</SelectItem>
                </SelectContent>
              </Select>
            </div>
          </div>
        ))}
      </CardContent>
    </Card>
  );
}

export function AddModelCard() {
  return (
    <Card className="pt-4">
      <CardContent className="space-y-4">
        <div className="flex justify-between space-y-4">
          <Avatar>
            <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
            <AvatarFallback>CN</AvatarFallback>
          </Avatar>
          <Button variant={'outline'}>Sub models</Button>
        </div>
        <Title>Deep seek</Title>
        <p>LLM,TEXT EMBEDDING, SPEECH2TEXT, MODERATION</p>
        <Card>
          <CardContent className="p-3 flex gap-2">
            <Button variant={'secondary'}>
              deepseek-chat <Trash2 />
            </Button>
            <Button variant={'secondary'}>
              deepseek-code <Trash2 />
            </Button>
          </CardContent>
        </Card>
        <div className="flex justify-end gap-2">
          <Button variant="secondary" size="icon">
            <MoreVertical className="h-4 w-4" />
          </Button>
          <Button>
            <Key /> API
          </Button>
        </div>
      </CardContent>
    </Card>
  );
}

export function ModelLibraryCard() {
  return (
    <Card className="pt-4">
      <CardContent className="space-y-4">
        <Avatar className="mb-4">
          <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
          <AvatarFallback>CN</AvatarFallback>
        </Avatar>

        <Title>Deep seek</Title>
        <p>LLM,TEXT EMBEDDING, SPEECH2TEXT, MODERATION</p>

        <div className="text-right">
          <Button>
            <Plus /> Add
          </Button>
        </div>
      </CardContent>
    </Card>
  );
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/profile-setting/model/model-card.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 137 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (3)

- `SystemModelSetting`: Exported entity
- `AddModelCard`: Exported entity
- `ModelLibraryCard`: Exported entity

### Functions (4)

- `Title()`: Function definition
- `SystemModelSetting()`: Function definition
- `AddModelCard()`: Function definition
- `ModelLibraryCard()`: Function definition

### Imports (6)

- `import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';`
- `import { Button } from '@/components/ui/button';`
- `import { Card, CardContent } from '@/components/ui/card';`
- `import {`
- `import { Key, MoreVertical, Plus, Trash2 } from 'lucide-react';`
- `import { PropsWithChildren } from 'react';`

## Code Structure Analysis

- Total lines: 137
- Blank lines: 8 (5.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~129


## Dependencies and Imports

- `@/components/ui/avatar`
- `@/components/ui/button`
- `@/components/ui/card`
- `lucide-react`
- `react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/profile-setting/model`.

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

- Other files in `web/src/pages/profile-setting/model/` directory
- Potential test file: `test_model-card.tsx`

## Keywords

@/components/ui/avatar, @/components/ui/button, @/components/ui/card, API, ASR, Add, AddModelCard, Avatar, AvatarFallback, AvatarImage, Button, Card, CardContent, Deep, DeepseekChat, EMBEDDING, Embedding, English, GPT, Image, Key, LLM, MODERATION, Model, ModelLibraryCard, MoreVertical, Plus, PropsWithChildren, SPEECH2TEXT, Select, SelectContent, SelectItem, SelectTrigger, SelectValue, Speech2TXT, Sub, SystemModelSetting, TEXT, TTS, The, Title, Trash2, TypeScript, Use, lucide-react, react, settings, shadcn

---
*Generated by RAGFlow Repository Documentation Generator*
