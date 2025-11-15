# File Documentation: web/src/locales/config.ts

## File Metadata

- **Path**: `web/src/locales/config.ts`
- **Extension**: `.ts`
- **Lines**: 84
- **Characters**: 2,472
- **Size**: 2,472 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```typescript
import i18n from 'i18next';
import LanguageDetector from 'i18next-browser-languagedetector';
import { initReactI18next } from 'react-i18next';

import { LanguageAbbreviation } from '@/constants/common';
import translation_de from './de';
import translation_en from './en';
import translation_es from './es';
import translation_fr from './fr';
import translation_id from './id';
import translation_ja from './ja';
import translation_pt_br from './pt-br';
import translation_ru from './ru';
import { createTranslationTable, flattenObject } from './until';
import translation_vi from './vi';
import translation_zh from './zh';
import translation_zh_traditional from './zh-traditional';

const resources = {
  [LanguageAbbreviation.En]: translation_en,
  [LanguageAbbreviation.Zh]: translation_zh,
  [LanguageAbbreviation.ZhTraditional]: translation_zh_traditional,
  [LanguageAbbreviation.Id]: translation_id,
  [LanguageAbbreviation.Ja]: translation_ja,
  [LanguageAbbreviation.Es]: translation_es,
  [LanguageAbbreviation.Vi]: translation_vi,
  [LanguageAbbreviation.Ru]: translation_ru,
  [LanguageAbbreviation.PtBr]: translation_pt_br,
  [LanguageAbbreviation.De]: translation_de,
  [LanguageAbbreviation.Fr]: translation_fr,
};
const enFlattened = flattenObject(translation_en);
const viFlattened = flattenObject(translation_vi);
const ruFlattened = flattenObject(translation_ru);
const esFlattened = flattenObject(translation_es);
const zhFlattened = flattenObject(translation_zh);
const jaFlattened = flattenObject(translation_ja);
const pt_brFlattened = flattenObject(translation_pt_br);
const zh_traditionalFlattened = flattenObject(translation_zh_traditional);
const deFlattened = flattenObject(translation_de);
const frFlattened = flattenObject(translation_fr);
export const translationTable = createTranslationTable(
  [
    enFlattened,
    viFlattened,
    ruFlattened,
    esFlattened,
    zhFlattened,
    zh_traditionalFlattened,
    jaFlattened,
    pt_brFlattened,
    deFlattened,
    frFlattened,
  ],
  [
    'English',
    'Vietnamese',
    'ru',
    'Spanish',
    'zh',
    'zh-TRADITIONAL',
    'ja',
    'pt-BR',
    'Deutsch',
    'French',
  ],
);
i18n
  .use(initReactI18next)
  .use(LanguageDetector)
  .init({
    detection: {
      lookupLocalStorage: 'lng',
    },
    supportedLngs: Object.values(LanguageAbbreviation),
    resources,
    fallbackLng: 'en',
    interpolation: {
      escapeValue: false,
    },
  });

export default i18n;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/locales/config.ts`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 84 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

### Exports (1)

- `translationTable`: Exported entity

### Imports (16)

- `import i18n from 'i18next';`
- `import LanguageDetector from 'i18next-browser-languagedetector';`
- `import { initReactI18next } from 'react-i18next';`
- `import { LanguageAbbreviation } from '@/constants/common';`
- `import translation_de from './de';`
- `import translation_en from './en';`
- `import translation_es from './es';`
- `import translation_fr from './fr';`
- `import translation_id from './id';`
- `import translation_ja from './ja';`

## Code Structure Analysis

- Total lines: 84
- Blank lines: 4 (4.8%)
- Comment lines: ~0 (0.0%)
- Code lines: ~80


## Dependencies and Imports

- `i18next`
- `i18next-browser-languagedetector`
- `react-i18next`
- `@/constants/common`
- `./de`
- `./en`
- `./es`
- `./fr`
- `./id`
- `./ja`
- `./pt-br`
- `./ru`
- `./until`
- `./vi`
- `./zh`
- `./zh-traditional`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/locales`.

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

- Other files in `web/src/locales/` directory
- Potential test file: `test_config.ts`

## Keywords

./de, ./en, ./es, ./fr, ./id, ./ja, ./pt-br, ./ru, ./until, ./vi, ./zh, ./zh-traditional, @/constants/common, Deutsch, English, French, LanguageAbbreviation, LanguageDetector, Object, PtBr, Spanish, TRADITIONAL, TypeScript, Vietnamese, ZhTraditional, deFlattened, enFlattened, esFlattened, frFlattened, i18next, i18next-browser-languagedetector, jaFlattened, pt_brFlattened, react-i18next, resources, ruFlattened, translationTable, viFlattened, zhFlattened, zh_traditionalFlattened

---
*Generated by RAGFlow Repository Documentation Generator*
