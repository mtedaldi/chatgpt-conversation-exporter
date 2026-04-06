# ChatGPT Conversation Exporter

A lightweight, browser-based tool to search, filter, and export individual conversations from a ChatGPT data export.

## Background

When you request a data export from ChatGPT, OpenAI delivers a ZIP archive containing (among other things) one or more `conversations-NNN.json` files and a `chat.html` viewer. The HTML viewer is handy for reading your history, but it offers no way to extract specific conversations. This tool fills that gap.

## Features

- **Multi-file support** — load the full ChatGPT export ZIP directly, or individual `conversations-*.json` files (drag & drop or file picker)
- **Full-text search** — searches both conversation titles and message content, with hit highlighting
- **Sorting** — by date (newest/oldest), alphabetically, or by message count
- **Selective export** — pick any subset of conversations
- **Two export formats**
  - **Markdown** — clean, readable `.md` files; multiple conversations are bundled as a ZIP
  - **JSON** — stripped-down, readable JSON without ChatGPT's internal mapping structure; multiple conversations are exported as a single array
- **100 % client-side** — no server, no uploads, no tracking; your data never leaves the browser

## Demo

👉 [Live version on GitHub Pages](https://mtedaldi.github.io/chatgpt-conversation-exporter/)

## Usage

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Drop your ChatGPT export ZIP onto the drop zone — or select individual `conversations-*.json` files
4. Search or sort to find the conversations you want
5. Check the ones you need and click **↓ Markdown** or **↓ JSON**

No installation required. The tool loads [JSZip](https://stuk.github.io/jszip/) from CDN for ZIP support, and Google Fonts for typography — both are purely optional; the tool works offline for JSON-only imports.

## Export formats

### Markdown
Each conversation becomes a `.md` file named after its title. Messages are rendered as:

```markdown
# Conversation title

Datum: 26.03.2026
Nachrichten: 12

---

**Du**

Your message here

---

**ChatGPT**

The response here

---
```

### JSON
Each conversation is exported as a clean object:

```json
{
  "id": "…",
  "title": "…",
  "create_time": 1700000000,
  "update_time": 1700000001,
  "messages": [
    { "role": "user", "text": "…", "time": 1700000000 },
    { "role": "assistant", "text": "…", "time": 1700000001 }
  ]
}
```

Multiple selected conversations are exported as a JSON array in a single file.

## Development

Developed with the assistance of [Claude](https://claude.ai) by Anthropic. The concept, requirements, and direction are by the author; the implementation was generated and iterated with AI assistance. The project is intentionally dependency-free — a single `index.html` with vanilla JS.

Contributions and issues welcome via GitHub.

## License

[MIT](LICENSE)
