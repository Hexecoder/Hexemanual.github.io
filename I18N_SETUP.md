# Internationalization (i18n) Setup Guide

This document explains how the internationalization (i18n) system works in Hexemanual using Docsify.

## Overview

Hexemanual supports multiple languages through a directory-based approach where each language has its own folder containing translated content.

## Directory Structure

```
/
├── index.html              # Main Docsify configuration
├── README.md               # English (default) homepage
├── _sidebar.md             # English navigation sidebar
├── _navbar.md              # English navigation bar
├── _404.md                 # English 404 page
├── /es/                    # Spanish content
│   ├── README.md
│   ├── _sidebar.md
│   ├── _navbar.md
│   └── _404.md
├── /fr/                    # French content
│   ├── README.md
│   ├── _sidebar.md
│   ├── _navbar.md
│   └── _404.md
└── /de/                    # German content
    ├── README.md
    ├── _sidebar.md
    ├── _navbar.md
    └── _404.md
```

## Supported Languages

- 🇺🇸 **English** (`/`) - Default language
- 🇪🇸 **Spanish** (`/es/`) - Español
- 🇫🇷 **French** (`/fr/`) - Français
- 🇩🇪 **German** (`/de/`) - Deutsch

## How It Works

### 1. Language Detection
- Default language (English) uses the root directory (`/`)
- Other languages use subdirectories (`/es/`, `/fr/`, `/de/`)
- URLs follow the pattern: `#/[language]/[page]`

### 2. Language Selector
- Located in the top-right corner of every page
- Automatically detects the current language
- Preserves the current page when switching languages
- Implemented with JavaScript in `index.html`

### 3. Search Localization
Search functionality is localized with language-specific:
- Placeholder text
- "No results" messages
- Search depth and indexing

### 4. Fallback System
- `fallbackLanguages: ['en']` ensures English content loads if translated content is missing
- 404 pages are localized for each language

## Adding a New Language

To add a new language (e.g., Italian):

1. **Create language directory**:
   ```bash
   mkdir it
   ```

2. **Copy and translate core files**:
   ```bash
   cp README.md it/README.md
   cp _sidebar.md it/_sidebar.md
   cp _navbar.md it/_navbar.md
   cp _404.md it/_404.md
   ```

3. **Update index.html**:
   - Add the new language option to the language selector
   - Add search placeholder and noData messages
   - Add notFoundPage configuration

4. **Translate content**:
   - Translate all markdown files to the new language
   - Update navigation links to use the new language prefix
   - Ensure all internal links use the correct language path

## Configuration Details

### Docsify Configuration
Key i18n-related settings in `window.$docsify`:

```javascript
search: {
  placeholder: {
    '/es/': 'Buscar',
    '/fr/': 'Rechercher', 
    '/de/': 'Suchen',
    '/': 'Search'
  },
  noData: {
    '/es/': '¡No hay resultados!',
    '/fr/': 'Aucun résultat !',
    '/de/': 'Keine Ergebnisse!',
    '/': 'No results!'
  }
},
fallbackLanguages: ['en'],
notFoundPage: {
  '/es/': '/es/_404.md',
  '/fr/': '/fr/_404.md', 
  '/de/': '/de/_404.md',
  '/': '/_404.md'
}
```

### Language Selector JavaScript
The language selector:
- Detects current language from URL path
- Maintains current page context when switching
- Handles route construction for different languages

## Best Practices

1. **Consistent Structure**: Maintain the same file structure across all languages
2. **Link Management**: Always use language-prefixed paths for internal links
3. **Content Sync**: Keep content structure consistent across languages
4. **Testing**: Test language switching and navigation thoroughly
5. **Fallbacks**: Always provide English fallback content

## Troubleshooting

### Common Issues

1. **Broken Links**: Ensure all internal links include the language prefix
2. **Missing Content**: Verify all required files exist in each language directory
3. **Search Issues**: Check search configuration includes all supported languages
4. **Navigation Problems**: Ensure sidebar and navbar files are properly translated

### Testing Checklist

- [ ] Language selector works in all languages
- [ ] Internal links work correctly in each language
- [ ] Search works and shows localized messages
- [ ] 404 pages display in the correct language
- [ ] Navigation is properly translated
- [ ] Content loads correctly for each language

## Performance Considerations

- Content is loaded on-demand per language
- Only the active language content is loaded
- Search indexes are built per language
- Minimal JavaScript overhead for language switching