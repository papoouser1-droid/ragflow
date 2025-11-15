# Documentation: web/src/components/jsonjoy-builder/components/features/schema-inferencer.tsx

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/components/features/schema-inferencer.tsx`
- **Size**: 3573 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/components/features/schema-inferencer.tsx`.

## Original Source Code

```tsx
import { Button } from '@/components/ui/button';
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from '@/components/ui/dialog';
import Editor, { type BeforeMount, type OnMount } from '@monaco-editor/react';
import { Loader2 } from 'lucide-react';
import { useRef, useState } from 'react';
import { useMonacoTheme } from '../../hooks/use-monaco-theme';
import { useTranslation } from '../../hooks/use-translation';
import { createSchemaFromJson } from '../../lib/schema-inference';
import type { JSONSchema } from '../../types/json-schema';

/** @public */
export interface SchemaInferencerProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  onSchemaInferred: (schema: JSONSchema) => void;
}

/** @public */
export function SchemaInferencer({
  open,
  onOpenChange,
  onSchemaInferred,
}: SchemaInferencerProps) {
  const t = useTranslation();
  const [jsonInput, setJsonInput] = useState('');
  const [error, setError] = useState<string | null>(null);
  const editorRef = useRef<Parameters<OnMount>[0] | null>(null);
  const {
    currentTheme,
    defineMonacoThemes,
    configureJsonDefaults,
    defaultEditorOptions,
  } = useMonacoTheme();

  const handleBeforeMount: BeforeMount = (monaco) => {
    defineMonacoThemes(monaco);
    configureJsonDefaults(monaco);
  };

  const handleEditorDidMount: OnMount = (editor) => {
    editorRef.current = editor;
    editor.focus();
  };

  const handleEditorChange = (value: string | undefined) => {
    setJsonInput(value || '');
  };

  const inferSchemaFromJson = () => {
    try {
      const jsonObject = JSON.parse(jsonInput);
      setError(null);

      // Use the schema inference service to create a schema
      const inferredSchema = createSchemaFromJson(jsonObject);

      onSchemaInferred(inferredSchema);
      onOpenChange(false);
    } catch (error) {
      console.error('Invalid JSON input:', error);
      setError(t.inferrerErrorInvalidJson);
    }
  };

  const handleClose = () => {
    setJsonInput('');
    setError(null);
    onOpenChange(false);
  };

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="sm:max-w-4xl max-h-[90vh] flex flex-col jsonjoy">
        <DialogHeader>
          <DialogTitle>{t.inferrerTitle}</DialogTitle>
          <DialogDescription>{t.inferrerDescription}</DialogDescription>
        </DialogHeader>
        <div className="flex-1 min-h-0 py-4 flex flex-col">
          <div className="border rounded-md flex-1 overflow-hidden h-full">
            <Editor
              height="450px"
              defaultLanguage="json"
              value={jsonInput}
              onChange={handleEditorChange}
              beforeMount={handleBeforeMount}
              onMount={handleEditorDidMount}
              options={defaultEditorOptions}
              theme={currentTheme}
              loading={
                <div className="flex items-center justify-center h-full w-full bg-secondary/30">
                  <Loader2 className="h-6 w-6 animate-spin" />
                </div>
              }
            />
          </div>
          {error && <p className="text-sm text-destructive mt-2">{error}</p>}
        </div>
        <DialogFooter>
          <Button type="button" variant="outline" onClick={handleClose}>
            {t.inferrerCancel}
          </Button>
          <Button type="button" onClick={inferSchemaFromJson}>
            {t.inferrerGenerate}
          </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/components/features/schema-inferencer.tsx` is located in the `web/src/components/jsonjoy-builder/components/features` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to features.

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

- [json-validator.tsx](json-validator.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
