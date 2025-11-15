# Documentation: web/src/pages/profile-setting/model/model-card.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/model/model-card.tsx`
- **Size**: 3990 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/profile-setting/model/model-card.tsx`.

## Original Source Code

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

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/profile-setting/model/model-card.tsx` is located in the `web/src/pages/profile-setting/model` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to model.

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

- [index.tsx](index.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
