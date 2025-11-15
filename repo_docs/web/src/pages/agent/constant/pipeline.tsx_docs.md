# File Documentation: web/src/pages/agent/constant/pipeline.tsx

## File Metadata

- **Path**: `web/src/pages/agent/constant/pipeline.tsx`
- **Extension**: `.tsx`
- **Lines**: 273
- **Characters**: 5,741
- **Size**: 5,741 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { ParseDocumentType } from '@/components/layout-recognize-form-field';
import {
  initialLlmBaseValues,
  DataflowOperator as Operator,
} from '@/constants/agent';

export enum FileType {
  PDF = 'pdf',
  Spreadsheet = 'spreadsheet',
  Image = 'image',
  Email = 'email',
  TextMarkdown = 'text&markdown',
  Docx = 'word',
  PowerPoint = 'slides',
  Video = 'video',
  Audio = 'audio',
}

export enum PdfOutputFormat {
  Json = 'json',
  Markdown = 'markdown',
}

export enum SpreadsheetOutputFormat {
  Json = 'json',
  Html = 'html',
}

export enum ImageOutputFormat {
  Text = 'text',
}

export enum EmailOutputFormat {
  Json = 'json',
  Text = 'text',
}

export enum TextMarkdownOutputFormat {
  Text = 'text',
}

export enum DocxOutputFormat {
  Markdown = 'markdown',
  Json = 'json',
}

export enum PptOutputFormat {
  Json = 'json',
}

export enum VideoOutputFormat {
  Text = 'text',
}

export enum AudioOutputFormat {
  Text = 'text',
}

export const OutputFormatMap = {
  [FileType.PDF]: PdfOutputFormat,
  [FileType.Spreadsheet]: SpreadsheetOutputFormat,
  [FileType.Image]: ImageOutputFormat,
  [FileType.Email]: EmailOutputFormat,
  [FileType.TextMarkdown]: TextMarkdownOutputFormat,
  [FileType.Docx]: DocxOutputFormat,
  [FileType.PowerPoint]: PptOutputFormat,
  [FileType.Video]: VideoOutputFormat,
  [FileType.Audio]: AudioOutputFormat,
};

export const InitialOutputFormatMap = {
  [FileType.PDF]: PdfOutputFormat.Json,
  [FileType.Spreadsheet]: SpreadsheetOutputFormat.Html,
  [FileType.Image]: ImageOutputFormat.Text,
  [FileType.Email]: EmailOutputFormat.Text,
  [FileType.TextMarkdown]: TextMarkdownOutputFormat.Text,
  [FileType.Docx]: DocxOutputFormat.Json,
  [FileType.PowerPoint]: PptOutputFormat.Json,
  [FileType.Video]: VideoOutputFormat.Text,
  [FileType.Audio]: AudioOutputFormat.Text,
};

export enum ContextGeneratorFieldName {
  Summary = 'summary',
  Keywords = 'keywords',
  Questions = 'questions',
  Metadata = 'metadata',
}

export const FileId = 'File'; // BeginId

export enum TokenizerSearchMethod {
  Embedding = 'embedding',
  FullText = 'full_text',
}

export enum ImageParseMethod {
  OCR = 'ocr',
}

export enum TokenizerFields {
  Text = 'text',
  Questions = 'questions',
  Summary = 'summary',
}

export enum ParserFields {
  From = 'from',
  To = 'to',
  Cc = 'cc',
  Bcc = 'bcc',
  Date = 'date',
  Subject = 'subject',
  Body = 'body',
  Attachments = 'attachments',
}

// initialBeginValues
export const initialFileValues = {
  outputs: {
    name: {
      type: 'string',
      value: '',
    },
    file: {
      type: 'Object',
      value: {},
    },
  },
};

export const initialTokenizerValues = {
  search_method: [
    TokenizerSearchMethod.Embedding,
    TokenizerSearchMethod.FullText,
  ],
  filename_embd_weight: 0.1,
  fields: TokenizerFields.Text,
  outputs: {},
};

export enum StringTransformMethod {
  Merge = 'merge',
  Split = 'split',
}

export enum StringTransformDelimiter {
  Comma = ',',
  Semicolon = ';',
  Period = '.',
  LineBreak = '\n',
  Tab = '\t',
  Space = ' ',
}

export const initialParserValues = {
  outputs: {
    markdown: { type: 'string', value: '' },
    text: { type: 'string', value: '' },
    html: { type: 'string', value: '' },
    json: { type: 'Array<object>', value: [] },
  },
  setups: [
    {
      fileFormat: FileType.PDF,
      output_format: PdfOutputFormat.Json,
      parse_method: ParseDocumentType.DeepDOC,
    },
    {
      fileFormat: FileType.Spreadsheet,
      output_format: SpreadsheetOutputFormat.Html,
    },
    {
      fileFormat: FileType.Image,
      output_format: ImageOutputFormat.Text,
      parse_method: ImageParseMethod.OCR,
      system_prompt: '',
    },
    {
      fileFormat: FileType.Email,
      fields: Object.values(ParserFields),
      output_format: EmailOutputFormat.Text,
    },
    {
      fileFormat: FileType.TextMarkdown,
      output_format: TextMarkdownOutputFormat.Text,
    },
    {
      fileFormat: FileType.Docx,
      output_format: DocxOutputFormat.Json,
    },
    {
      fileFormat: FileType.PowerPoint,
      output_format: PptOutputFormat.Json,
    },
  ],
};

export const initialSplitterValues = {
  outputs: {
    chunks: { type: 'Array<Object>', value: [] },
  },
  chunk_token_size: 512,
  overlapped_percent: 0,
  delimiters: [{ value: '\n' }],
};

export enum Hierarchy {
  H1 = '1',
  H2 = '2',
  H3 = '3',
  H4 = '4',
  H5 = '5',
}

export const initialHierarchicalMergerValues = {
  outputs: {
    chunks: { type: 'Array<Object>', value: [] },
  },
  hierarchy: Hierarchy.H3,
  levels: [
    { expressions: [{ expression: '^#[^#]' }] },
    { expressions: [{ expression: '^##[^#]' }] },
    { expressions: [{ expression: '^###[^#]' }] },
    { expressions: [{ expression: '^####[^#]' }] },
  ],
};

export const initialExtractorValues = {
  ...initialLlmBaseValues,
  field_name: ContextGeneratorFieldName.Summary,
  outputs: {
    chunks: { type: 'Array<Object>', value: [] },
  },
};

export const NoDebugOperatorsList = [Operator.Begin];

export const FileTypeSuffixMap = {
  [FileType.PDF]: ['pdf'],
  [FileType.Spreadsheet]: ['xls', 'xlsx', 'csv'],
  [FileType.Image]: ['jpg', 'jpeg', 'png', 'gif'],
  [FileType.Email]: ['eml', 'msg'],
  [FileType.TextMarkdown]: ['md', 'markdown', 'mdx', 'txt'],
  [FileType.Docx]: ['doc', 'docx'],
  [FileType.PowerPoint]: ['pptx'],
  [FileType.Video]: ['mp4', 'avi', 'mkv'],
  [FileType.Audio]: [
    'da',
    'wave',
    'wav',
    'mp3',
    'aac',
    'flac',
    'ogg',
    'aiff',
    'au',
    'midi',
    'wma',
    'realaudio',
    'vqf',
    'oggvorbis',
    'ape',
  ],
};

export const SingleOperators = [
  Operator.Tokenizer,
  Operator.Splitter,
  Operator.HierarchicalMerger,
  Operator.Parser,
];

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/agent/constant/pipeline.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 273 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (12)

- `OutputFormatMap`: Exported entity
- `InitialOutputFormatMap`: Exported entity
- `FileId`: Exported entity
- `initialFileValues`: Exported entity
- `initialTokenizerValues`: Exported entity
- `initialParserValues`: Exported entity
- `initialSplitterValues`: Exported entity
- `initialHierarchicalMergerValues`: Exported entity
- `initialExtractorValues`: Exported entity
- `NoDebugOperatorsList`: Exported entity
- `FileTypeSuffixMap`: Exported entity
- `SingleOperators`: Exported entity

### Imports (2)

- `import { ParseDocumentType } from '@/components/layout-recognize-form-field';`
- `import {`

## Code Structure Analysis

- Total lines: 273
- Blank lines: 31 (11.4%)
- Comment lines: ~1 (0.4%)
- Code lines: ~241


## Dependencies and Imports

- `@/components/layout-recognize-form-field`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/agent/constant`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/agent/constant/` directory
- Potential test file: `test_pipeline.tsx`

## Keywords

@/components/layout-recognize-form-field, Array, Attachments, Audio, AudioOutputFormat, Bcc, Begin, BeginId, Body, Comma, ContextGeneratorFieldName, DataflowOperator, Date, DeepDOC, Docx, DocxOutputFormat, Email, EmailOutputFormat, Embedding, File, FileId, FileType, FileTypeSuffixMap, From, FullText, HierarchicalMerger, Hierarchy, Html, Image, ImageOutputFormat, ImageParseMethod, InitialOutputFormatMap, Json, Keywords, LineBreak, Markdown, Merge, Metadata, NoDebugOperatorsList, OCR, Object, Operator, OutputFormatMap, PDF, ParseDocumentType, Parser, ParserFields, PdfOutputFormat, Period, PowerPoint...

---
*Generated by RAGFlow Repository Documentation Generator*
