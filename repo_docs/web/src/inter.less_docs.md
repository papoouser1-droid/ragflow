# File Documentation: web/src/inter.less

## File Metadata

- **Path**: `web/src/inter.less`
- **Extension**: `.less`
- **Lines**: 274
- **Characters**: 6,996
- **Size**: 6,996 bytes
- **Purpose**: General file in the repository

## Original Source

```
/* Variable fonts usage:
:root { font-family: "Inter", sans-serif; }
@supports (font-variation-settings: normal) {
  :root { font-family: "InterVariable", sans-serif; font-optical-sizing: auto; }
} */
@font-face {
  font-family: InterVariable;
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
  src: url('@/assets/inter/InterVariable.woff2') format('woff2');
}
@font-face {
  font-family: InterVariable;
  font-style: italic;
  font-weight: 100 900;
  font-display: swap;
  src: url('@/assets/inter/InterVariable-Italic.woff2') format('woff2');
}

/* static fonts */
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 100;
  font-display: swap;
  src: url('@/assets/inter/Inter-Thin.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 100;
  font-display: swap;
  src: url('@/assets/inter/Inter-ThinItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 200;
  font-display: swap;
  src: url('@/assets/inter/Inter-ExtraLight.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 200;
  font-display: swap;
  src: url('@/assets/inter/Inter-ExtraLightItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url('@/assets/inter/Inter-Light.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url('@/assets/inter/Inter-LightItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url('@/assets/inter/Inter-Regular.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url('@/assets/inter/Inter-Italic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 500;
  font-display: swap;
  src: url('@/assets/inter/Inter-Medium.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 500;
  font-display: swap;
  src: url('@/assets/inter/Inter-MediumItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url('@/assets/inter/Inter-SemiBold.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url('@/assets/inter/Inter-SemiBoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 700;
  font-display: swap;
  src: url('@/assets/inter/Inter-Bold.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 700;
  font-display: swap;
  src: url('@/assets/inter/Inter-BoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 800;
  font-display: swap;
  src: url('@/assets/inter/Inter-ExtraBold.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 800;
  font-display: swap;
  src: url('@/assets/inter/Inter-ExtraBoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 900;
  font-display: swap;
  src: url('@/assets/inter/Inter-Black.woff2') format('woff2');
}
@font-face {
  font-family: 'Inter';
  font-style: italic;
  font-weight: 900;
  font-display: swap;
  src: url('@/assets/inter/Inter-BlackItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 100;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Thin.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 100;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-ThinItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 200;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-ExtraLight.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 200;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-ExtraLightItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Light.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-LightItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Regular.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Italic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 500;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Medium.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 500;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-MediumItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-SemiBold.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-SemiBoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 700;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Bold.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 700;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-BoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 800;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-ExtraBold.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 800;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-ExtraBoldItalic.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: normal;
  font-weight: 900;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-Black.woff2') format('woff2');
}
@font-face {
  font-family: 'InterDisplay';
  font-style: italic;
  font-weight: 900;
  font-display: swap;
  src: url('@/assets/inter/InterDisplay-BlackItalic.woff2') format('woff2');
}

```

## High-Level Overview

/* Variable fonts usage:

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 274
- Blank lines: 2 (0.7%)
- Comment lines: ~2 (0.7%)
- Code lines: ~270


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src`.

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

- Other files in `web/src/` directory
- Potential test file: `test_inter.less`

## Keywords

Black, BlackItalic, Bold, BoldItalic, ExtraBold, ExtraBoldItalic, ExtraLight, ExtraLightItalic, Inter, InterDisplay, InterVariable, Italic, Light, LightItalic, Medium, MediumItalic, Regular, SemiBold, SemiBoldItalic, Thin, ThinItalic, Variable, font, supports

---
*Generated by RAGFlow Repository Documentation Generator*
