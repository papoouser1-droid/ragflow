# Documentation: web/src/constants/common.ts

## File Metadata

- **Path**: `web/src/constants/common.ts`
- **Size**: 3565 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/constants/common.ts`.

## Original Source Code

```ts
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

## Detailed Analysis

### File Role in Repository

The file `web/src/constants/common.ts` is located in the `web/src/constants` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to constants.

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

- [agent.tsx](agent.tsx_docs.md)
- [authorization.ts](authorization.ts_docs.md)
- [chat.ts](chat.ts_docs.md)
- [file.ts](file.ts_docs.md)
- [form.ts](form.ts_docs.md)
- [knowledge.ts](knowledge.ts_docs.md)
- [llm.ts](llm.ts_docs.md)
- [permission.ts](permission.ts_docs.md)
- [setting.ts](setting.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
