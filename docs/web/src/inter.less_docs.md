# Documentation: web/src/inter.less

## File Metadata

- **Path**: `web/src/inter.less`
- **Size**: 6996 bytes
- **Type**: .less
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/inter.less`.

## Original Source Code

```less
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

## Detailed Analysis

### File Role in Repository

The file `web/src/inter.less` is located in the `web/src` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to src.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [app.tsx](app.tsx_docs.md)
- [conf.json](conf.json_docs.md)
- [custom.d.ts](custom.d.ts_docs.md)
- [global.less](global.less_docs.md)
- [routes.ts](routes.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
