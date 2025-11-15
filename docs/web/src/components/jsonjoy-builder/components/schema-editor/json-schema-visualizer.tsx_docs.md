# Documentation: web/src/components/jsonjoy-builder/components/schema-editor/json-schema-visualizer.tsx

## File Metadata

- **Path**: `web/src/components/jsonjoy-builder/components/schema-editor/json-schema-visualizer.tsx`
- **Size**: 3463 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/components/jsonjoy-builder/components/schema-editor/json-schema-visualizer.tsx`.

## Original Source Code

```tsx
import Editor, { type BeforeMount, type OnMount } from '@monaco-editor/react';
import { Download, FileJson, Loader2 } from 'lucide-react';
import { useRef, type FC } from 'react';
import { useMonacoTheme } from '../../hooks/use-monaco-theme';
import { useTranslation } from '../../hooks/use-translation';
import { cn } from '../../lib/utils';
import type { JSONSchema } from '../../types/json-schema';

/** @public */
export interface JsonSchemaVisualizerProps {
  schema: JSONSchema;
  className?: string;
  onChange?: (schema: JSONSchema) => void;
  readOnly?: boolean;
  showHeader?: boolean;
}

/** @public */
const JsonSchemaVisualizer: FC<JsonSchemaVisualizerProps> = ({
  schema,
  className,
  onChange,
  readOnly = false,
  showHeader = true,
}) => {
  const editorRef = useRef<Parameters<OnMount>[0] | null>(null);
  const {
    currentTheme,
    defineMonacoThemes,
    configureJsonDefaults,
    defaultEditorOptions,
  } = useMonacoTheme();

  const t = useTranslation();

  const handleBeforeMount: BeforeMount = (monaco) => {
    defineMonacoThemes(monaco);
    configureJsonDefaults(monaco);
  };

  const handleEditorDidMount: OnMount = (editor) => {
    editorRef.current = editor;
    editor.focus();
  };

  const handleEditorChange = (value: string | undefined) => {
    if (!value) return;

    try {
      const parsedJson = JSON.parse(value);
      if (onChange) {
        onChange(parsedJson);
      }
    } catch (_error) {
      // Monaco will show the error inline, no need for additional error handling
    }
  };

  const handleDownload = () => {
    const content = JSON.stringify(schema, null, 2);
    const blob = new Blob([content], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = t.visualizerDownloadFileName;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  };

  return (
    <div
      className={cn(
        'relative overflow-hidden h-full flex flex-col',
        className,
        'jsonjoy',
      )}
    >
      {showHeader && (
        <div className="flex items-center justify-between bg-secondary/80 backdrop-blur-xs px-4 py-2 border-b shrink-0">
          <div className="flex items-center gap-2">
            <FileJson size={18} />
            <span className="font-medium text-sm">{t.visualizerSource}</span>
          </div>
          <button
            type="button"
            onClick={handleDownload}
            className="p-1.5 hover:bg-secondary rounded-md transition-colors"
            title={t.visualizerDownloadTitle}
          >
            <Download size={16} />
          </button>
        </div>
      )}
      <div className="grow flex min-h-0">
        <Editor
          height="100%"
          defaultLanguage="json"
          value={JSON.stringify(schema, null, 2)}
          onChange={handleEditorChange}
          beforeMount={handleBeforeMount}
          onMount={handleEditorDidMount}
          className="monaco-editor-container w-full h-full"
          loading={
            <div className="flex items-center justify-center h-full w-full bg-secondary/30">
              <Loader2 className="h-6 w-6 animate-spin" />
            </div>
          }
          options={{ ...defaultEditorOptions, readOnly }}
          theme={currentTheme}
        />
      </div>
    </div>
  );
};

export default JsonSchemaVisualizer;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/components/jsonjoy-builder/components/schema-editor/json-schema-visualizer.tsx` is located in the `web/src/components/jsonjoy-builder/components/schema-editor` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to schema-editor.

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

- [add-field-button.tsx](add-field-button.tsx_docs.md)
- [json-schema-editor.tsx](json-schema-editor.tsx_docs.md)
- [schema-field-list.tsx](schema-field-list.tsx_docs.md)
- [schema-field.tsx](schema-field.tsx_docs.md)
- [schema-property-editor.tsx](schema-property-editor.tsx_docs.md)
- [schema-type-selector.tsx](schema-type-selector.tsx_docs.md)
- [schema-visual-editor.tsx](schema-visual-editor.tsx_docs.md)
- [type-dropdown.tsx](type-dropdown.tsx_docs.md)
- [type-editor.tsx](type-editor.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
