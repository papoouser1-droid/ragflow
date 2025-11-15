# Documentation: web/tailwind.config.js

## File Metadata

- **Path**: `web/tailwind.config.js`
- **Size**: 8336 bytes
- **Type**: .js
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/tailwind.config.js`.

## Original Source Code

```js
const { fontFamily } = require('tailwindcss/defaultTheme');

/** @type {import('tailwindcss').Config} */

module.exports = {
  darkMode: ['selector'],
  content: [
    './src/pages/**/*.tsx',
    './src/components/**/*.tsx',
    './src/layouts/**/*.tsx',
  ],
  theme: {
    container: {
      center: true,
      padding: '2rem',
      screens: {
        '2xl': '1536px',
      },
    },
    screens: {
      sm: '640px',
      md: '768px',
      lg: '1024px',
      xl: '1280px',
      '2xl': '1536px',
      '3xl': '1780px',
      '4xl': '1980px',
    },
    extend: {
      borderWidth: {
        0.5: '0.5px',
      },
      colors: {
        border: 'var(--border-default)',
        input: 'hsl(var(--input))',
        ring: 'hsl(var(--ring))',
        background: 'var(--background)',
        foreground: 'var(--colors-text-neutral-strong)',
        buttonBlueText: 'var(--button-blue-text)',

        'colors-outline-sentiment-primary':
          'var(--colors-outline-sentiment-primary)',
        'colors-outline-neutral-strong': 'var(--colors-outline-neutral-strong)',
        'colors-outline-neutral-standard':
          'var(--colors-outline-neutral-standard)',

        'colors-text-core-standard': 'var(--colors-text-core-standard)',
        'colors-text-neutral-strong': 'var(--colors-text-neutral-strong)',
        'colors-text-neutral-standard': 'var(--colors-text-neutral-standard)',
        'colors-text-neutral-weak': 'var(--colors-text-neutral-weak)',
        'colors-text-functional-danger': 'var(--colors-text-functional-danger)',
        'colors-text-inverse-strong': 'var(--colors-text-inverse-strong)',
        'colors-text-persist-light': 'var(--colors-text-persist-light)',
        'colors-text-inverse-weak': 'var(--colors-text-inverse-weak)',

        'background-badge': 'var(--background-badge)',
        'text-badge': 'var(--text-badge)',
        'text-title': 'var(--text-title)',
        'text-sub-title': 'var(--text-sub-title)',
        'text-sub-title-invert': 'var(--text-sub-title-invert)',
        'text-title-invert': 'var(--text-title-invert)',
        'background-header-bar': 'var(--background-header-bar)',
        'background-card': 'var(--background-card)',
        'background-note': 'var(--background-note)',
        'background-highlight': 'var(--background-highlight)',

        'input-border': 'var(--input-border)',

        /* design colors */
        'bg-title': 'var(--bg-title)',
        'bg-base': 'var(--bg-base)',
        'bg-card': 'var(--bg-card)',
        'bg-component': 'var(--bg-component)',
        'bg-input': 'var(--bg-input)',
        'bg-canvas': {
          DEFAULT: 'rgb(var(--bg-canvas) / <alpha-value>)',
        },
        'bg-list': {
          DEFAULT: 'rgb(var(--bg-list) / <alpha-value>)',
        },
        'text-primary': {
          DEFAULT: 'rgb(var(--text-primary) / <alpha-value>)',
        },
        'text-primary-inverse': {
          DEFAULT: 'rgb(var(--text-primary-inverse) / <alpha-value>)',
        },
        'text-secondary': {
          DEFAULT: 'rgb(var(--text-secondary) / <alpha-value>)',
        },
        'text-secondary-inverse': {
          DEFAULT: 'rgb(var(--text-secondary-inverse) / <alpha-value>)',
        },
        'text-disabled': 'var(--text-disabled)',
        'text-input-tip': 'var(--text-input-tip)',
        'border-default': 'var(--border-default)',
        'border-accent': 'var(--border-accent)',
        'border-button': 'var(--border-button)',
        'accent-primary': {
          DEFAULT: 'rgb(var(--accent-primary) / <alpha-value>)',
          5: 'rgba(var(--accent-primary) / 0.05)', // 5%
        },
        'bg-accent': 'var(--bg-accent)',
        'state-success': {
          DEFAULT: 'rgb(var(--state-success) / <alpha-value>)',
          5: 'rgba(var(--state-success) / 0.05)', // 5%
        },
        'state-warning': {
          DEFAULT: 'rgb(var(--state-warning) / <alpha-value>)',
          5: 'rgba(var(--state-warning) / 0.05)', // 5%
        },
        'state-error': {
          DEFAULT: 'rgb(var(--state-error) / <alpha-value>)',
          5: 'rgba(var(--state-error) / 0.05)', // 5%
        },
        'team-group': 'var(--team-group)',
        'team-member': 'var(--team-member)',
        'team-department': 'var(--team-department)',
        'bg-group': 'var(--bg-group)',
        'bg-member': 'var(--bg-member)',
        'bg-department': 'var(--bg-department)',

        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'var(--background-inverse-strong)',
          foreground: 'var(--background-inverse-strong-foreground)',
        },
        destructive: {
          DEFAULT: 'hsl(var(--destructive))',
          foreground: 'hsl(var(--destructive-foreground))',
        },
        muted: {
          DEFAULT: 'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
        popover: {
          DEFAULT: 'hsl(var(--popover))',
          foreground: 'hsl(var(--popover-foreground))',
        },
        card: {
          DEFAULT: 'var(--background-inverse-standard)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        backgroundCoreWeak: {
          DEFAULT: 'var(--background-core-weak)',
          foreground: 'var(--background-core-weak-foreground)',
        },
        'colors-background-inverse-standard': {
          DEFAULT: 'var(--colors-background-inverse-standard)',
          foreground: 'var(--colors-background-inverse-standard-foreground)',
        },
        'colors-background-inverse-standard': {
          DEFAULT: 'var(--colors-background-inverse-standard)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        'colors-background-inverse-strong': {
          DEFAULT: 'var(--colors-background-inverse-strong)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        'colors-background-neutral-standard': {
          DEFAULT: 'var(--colors-background-neutral-standard)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        'colors-background-neutral-strong': {
          DEFAULT: 'var(--colors-background-neutral-strong)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        'colors-background-neutral-weak': {
          DEFAULT: 'var(--colors-background-neutral-weak)',
          foreground: 'var(--background-inverse-standard-foreground)',
        },
        sidebar: {
          DEFAULT: 'hsl(var(--sidebar-background))',
          foreground: 'hsl(var(--sidebar-foreground))',
          primary: 'hsl(var(--sidebar-primary))',
          'primary-foreground': 'hsl(var(--sidebar-primary-foreground))',
          accent: 'hsl(var(--sidebar-accent))',
          'accent-foreground': 'hsl(var(--sidebar-accent-foreground))',
          border: 'hsl(var(--sidebar-border))',
          ring: 'hsl(var(--sidebar-ring))',
        },
      },
      backgroundImage: {
        'metallic-gradient':
          'linear-gradient(104deg, rgb(var(--text-primary)) 30%, var(--metallic) 50%, rgb(var(--text-primary)) 70%)',
      },
      borderRadius: {
        lg: `var(--radius)`,
        md: `calc(var(--radius) - 2px)`,
        sm: 'calc(var(--radius) - 4px)',
      },
      fontFamily: {
        sans: ['var(--font-sans)', ...fontFamily.sans],
      },
      keyframes: {
        'accordion-down': {
          from: { height: '0' },
          to: { height: 'var(--radix-accordion-content-height)' },
        },
        'accordion-up': {
          from: { height: 'var(--radix-accordion-content-height)' },
          to: { height: '0' },
        },
        'caret-blink': {
          '0%,70%,100%': { opacity: '1' },
          '20%,50%': { opacity: '0' },
        },
      },
      animation: {
        'accordion-down': 'accordion-down 0.2s ease-out',
        'accordion-up': 'accordion-up 0.2s ease-out',
        'caret-blink': 'caret-blink 1.25s ease-out infinite',
      },
    },
  },
  plugins: [
    require('tailwindcss-animate'),
    require('@tailwindcss/line-clamp'),
    require('tailwind-scrollbar'),
  ],
};

```

## Detailed Analysis

### File Role in Repository

The file `web/tailwind.config.js` is located in the `web` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to web.

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

- [README.md](README.md_docs.md)
- [externals.d.ts](externals.d.ts_docs.md)
- [jest-setup.ts](jest-setup.ts_docs.md)
- [jest.config.ts](jest.config.ts_docs.md)
- [package-lock.json](package-lock.json_docs.md)
- [package.json](package.json_docs.md)
- [postcss.config.js](postcss.config.js_docs.md)
- [tailwind.css](tailwind.css_docs.md)
- [tsconfig.json](tsconfig.json_docs.md)
- [typings.d.ts](typings.d.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
