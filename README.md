# ChatGPT Conversation Exporter

A fully client-side browser tool to search, preview, and export your ChatGPT conversations from a ChatGPT data export (ZIP or JSON).

**Live:** [mtedaldi.github.io/chatgpt-conversation-exporter](https://mtedaldi.github.io/chatgpt-conversation-exporter/)

---

## Features

- **Import** ChatGPT data export (ZIP) or a single `conversations.json`
- **Search** across all conversation titles and message content
- **Sort** by date (newest/oldest), title (A–Z), or message count
- **Filter** to conversations containing images (ZIP only)
- **Preview** conversations inline with chat bubbles and embedded images
- **Export** selected conversations in multiple formats:

| Format | Images | Description |
|--------|--------|-------------|
| Markdown | — | One `.md` per conversation; multiple → ZIP |
| JSON | — | Clean JSON without internal metadata |
| HTML | ✓ | Self-contained file with Base64-embedded images |
| Markdown + ZIP | ✓ | `.md` files + `images/` folder in a ZIP |
| JSON + ZIP | ✓ | `conversations.json` + `images/` folder in a ZIP |

> **"Mit Bildern" exports** are only available when a ZIP file is loaded, since images are only present in the full data export.

---

## Privacy

Everything runs locally in your browser. No data is uploaded anywhere.

---

## Build & Deploy

The source `index.html` references JSZip via CDN. The CI build:

1. Downloads JSZip from cdnjs
2. Applies several sanitisation passes to make it safe for inline embedding:
   - Simplifies the UMD wrapper to always assign `window.JSZip`
   - Escapes raw newline bytes (`0x0a`) inside string literals (ZIP magic bytes)
   - Escapes malformed `\x` sequences (JSZip's hex-escape builder)
   - Escapes `</script>` occurrences to prevent premature script-block termination
3. Embeds Google Fonts as Base64 (via [embed-google-fonts](https://github.com/mtedaldi/embed-google-fonts))
4. Deploys to GitHub Pages

---

## Changelog

### v1.0.2
- Fix `SyntaxError: malformed hexadecimal character escape sequence` in inlined JSZip (pretty() function)
- Fix broken JSZip magic bytes: inlining must use binary-safe (latin-1) processing, not UTF-8, to preserve raw bytes
- Restore CDN `<script src>` in source `index.html`; CI workflow does all sanitisation on raw bytes
- `deploy.yml`: all four JSZip fixes now operate byte-level before any text decoding

### v1.0.1
- Fix `SyntaxError: string literal contains an unescaped line break` in inlined JSZip (ZIP magic bytes)
- Fix JSZip UMD wrapper to always assign to `window` regardless of environment
- Hide "Mit Bildern" export section until a ZIP file is loaded
- Fix hardcoded error state in initial HTML

### v1.0.0
- Initial release: import, search, filter, preview, export in 5 formats

---

## Development note

This project was developed with AI assistance (Claude by Anthropic). All code has been reviewed and tested by the author.

## License

MIT
