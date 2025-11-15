# File Documentation: web/src/constants/common.ts

## File Metadata

- **Path**: `web/src/constants/common.ts`
- **Extension**: `.ts`
- **Lines**: 168
- **Characters**: 3,493
- **Size**: 3,565 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
export const fileIconMap = {
  aep: 'aep.svg',
  ai: 'ai.svg',
  avi: 'avi.svg',
  css: 'css.svg',
  csv: 'csv.svg',
  dmg: 'dmg.svg',
  doc: 'doc.svg',
  docx: 'docx.svg',
  eps: 'eps.svg',
  exe: 'exe.svg',
  fig: 'fig.svg',
  gif: 'gif.svg',
  html: 'html.svg',
  indd: 'indd.svg',
  java: 'java.svg',
  jpeg: 'jpeg.svg',
  jpg: 'jpg.svg',
  js: 'js.svg',
  json: 'json.svg',
  mkv: 'mkv.svg',
  mp3: 'mp3.svg',
  mp4: 'mp4.svg',
  mpeg: 'mpeg.svg',
  pdf: 'pdf.svg',
  png: 'png.svg',
  ppt: 'ppt.svg',
  pptx: 'pptx.svg',
  psd: 'psd.svg',
  rss: 'rss.svg',
  sql: 'sql.svg',
  svg: 'svg.svg',
  tiff: 'tiff.svg',
  txt: 'txt.svg',
  wav: 'wav.svg',
  webp: 'webp.svg',
  xls: 'xls.svg',
  xlsx: 'xlsx.svg',
  xml: 'xml.svg',
};

export const LanguageList = [
  'English',
  'Chinese',
  'Traditional Chinese',
  'Russian',
  'Indonesia',
  'Spanish',
  'Vietnamese',
  'Japanese',
  'Portuguese BR',
  'German',
  'French',
];
export const LanguageMap = {
  English: 'English',
  Chinese: '简体中文',
  'Traditional Chinese': '繁體中文',
  Russian: 'Русский',
  Indonesia: 'Indonesia',
  Spanish: 'Español',
  Vietnamese: 'Tiếng việt',
  Japanese: '日本語',
  'Portuguese BR': 'Português BR',
  German: 'German',
  French: 'Français',
};

export enum LanguageAbbreviation {
  En = 'en',
  Zh = 'zh',
  ZhTraditional = 'zh-TRADITIONAL',
  Ru = 'ru',
  Id = 'id',
  Ja = 'ja',
  Es = 'es',
  Vi = 'vi',
  PtBr = 'pt-BR',
  De = 'de',
  Fr = 'fr',
}

export const LanguageAbbreviationMap = {
  [LanguageAbbreviation.En]: 'English',
  [LanguageAbbreviation.Zh]: '简体中文',
  [LanguageAbbreviation.ZhTraditional]: '繁體中文',
  [LanguageAbbreviation.Ru]: 'Русский',
  [LanguageAbbreviation.Id]: 'Indonesia',
  [LanguageAbbreviation.Es]: 'Español',
  [LanguageAbbreviation.Vi]: 'Tiếng việt',
  [LanguageAbbreviation.Ja]: '日本語',
  [LanguageAbbreviation.PtBr]: 'Português BR',
  [LanguageAbbreviation.De]: 'Deutsch',
  [LanguageAbbreviation.Fr]: 'Français',
};

export const LanguageTranslationMap = {
  English: 'en',
  Chinese: 'zh',
  'Traditional Chinese': 'zh-TRADITIONAL',
  Russian: 'ru',
  Indonesia: 'id',
  Spanish: 'es',
  Vietnamese: 'vi',
  Japanese: 'ja',
  'Portuguese BR': 'pt-br',
  German: 'de',
  French: 'fr',
};

export enum FileMimeType {
  Bmp = 'image/bmp',
  Csv = 'text/csv',
  Odt = 'application/vnd.oasis.opendocument.text',
  Doc = 'application/msword',
  Docx = 'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  Gif = 'image/gif',
  Htm = 'text/htm',
  Html = 'text/html',
  Jpg = 'image/jpg',
  Jpeg = 'image/jpeg',
  Pdf = 'application/pdf',
  Png = 'image/png',
  Ppt = 'application/vnd.ms-powerpoint',
  Pptx = 'application/vnd.openxmlformats-officedocument.presentationml.presentation',
  Tiff = 'image/tiff',
  Txt = 'text/plain',
  Xls = 'application/vnd.ms-excel',
  Xlsx = 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
  Mp4 = 'video/mp4',
  Json = 'application/json',
}

export const Domain = 'demo.ragflow.io';

//#region file preview
export const Images = [
  'jpg',
  'jpeg',
  'png',
  'gif',
  'bmp',
  'tif',
  'tiff',
  'webp',
  // 'svg',
  'ico',
];

// Without FileViewer
export const ExceptiveType = ['xlsx', 'xls', 'pdf', 'docx', ...Images];

export const SupportedPreviewDocumentTypes = [...ExceptiveType];
//#endregion

export enum Platform {
  RAGFlow = 'RAGFlow',
  Dify = 'Dify',
  FastGPT = 'FastGPT',
  Coze = 'Coze',
}

export enum ThemeEnum {
  Dark = 'dark',
  Light = 'light',
  System = 'system',
}

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/constants/common.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 168 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (9)

- `fileIconMap`: Exported entity
- `LanguageList`: Exported entity
- `LanguageMap`: Exported entity
- `LanguageAbbreviationMap`: Exported entity
- `LanguageTranslationMap`: Exported entity
- `Domain`: Exported entity
- `Images`: Exported entity
- `ExceptiveType`: Exported entity
- `SupportedPreviewDocumentTypes`: Exported entity

## Code Structure Analysis

- Total lines: 168
- Blank lines: 12 (7.1%)
- Comment lines: ~4 (2.4%)
- Code lines: ~152


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/constants`.

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

- Other files in `web/src/constants/` directory
- Potential test file: `test_common.ts`

## Keywords

Bmp, Chinese, Coze, Csv, Dark, Deutsch, Dify, Doc, Docx, Domain, English, ExceptiveType, FastGPT, FileMimeType, FileViewer, French, German, Gif, Htm, Html, Images, Indonesia, Japanese, Jpeg, Jpg, Json, LanguageAbbreviation, LanguageAbbreviationMap, LanguageList, LanguageMap, LanguageTranslationMap, Light, Mp4, Odt, Pdf, Platform, Png, Portuguese, Ppt, Pptx, PtBr, RAGFlow, Russian, Spanish, SupportedPreviewDocumentTypes, System, TRADITIONAL, ThemeEnum, Tiff, Traditional...

---
*Generated by RAGFlow Repository Documentation Generator*
