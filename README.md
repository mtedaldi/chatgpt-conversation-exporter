# ChatGPT Conversation Exporter

A lightweight, browser-based tool to search, browse, and export individual conversations from a ChatGPT data export — including images.

## Background

When you request a data export from ChatGPT, OpenAI delivers a ZIP archive containing one or more `conversations-NNN.json` files and a `chat.html` viewer. The HTML viewer lets you read your history but offers no way to extract specific conversations. This tool fills that gap.

## Features

- **Direct ZIP import** — drop the full ChatGPT export ZIP without unpacking it first
- **Full-text search** — searches both conversation titles and message content, with hit highlighting
- **Sorting** — by date (newest/oldest), alphabetically, or by message count
- **Image filter** — show only conversations that contain images
- **Live preview** — click any conversation to read it directly in the tool, including embedded images
- **Selective export** — pick any subset of conversations
- **Five export formats:**

  | Format | Images | Output |
  |--------|--------|--------|
  | Markdown | — | `.md` file(s), multiple as ZIP |
  | JSON | — | Clean JSON without ChatGPT's internal mapping structure |
  | HTML | ✓ | Self-contained `.html` with all images embedded as Base64 |
  | Markdown + ZIP | ✓ | `.md` with relative image links + `images/` folder |
  | JSON + ZIP | ✓ | `conversations.json` + `images/` folder |

- **Image support** — user-uploaded photos and DALL·E generated images are extracted from the ZIP and matched to their conversation messages
- **Export progress indicator** — visible progress overlay when exporting large selections
- **100% client-side** — no server, no uploads, no tracking; your data never leaves the browser

## Demo

👉 [Live version on GitHub Pages](https://mtedaldi.github.io/chatgpt-conversation-exporter/)

## Usage

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Drop your ChatGPT export ZIP onto the drop zone — or select individual `conversations-*.json` files
4. Use the search box, sort order, and image filter to find conversations
5. Click a conversation title to preview it in the side panel
6. Check the conversations you want to export and click the appropriate export button

No installation required. The tool loads [JSZip](https://stuk.github.io/jszip/) from CDN for ZIP support and Google Fonts for typography — both optional; the tool works offline for JSON-only imports. The [GitHub Pages build](https://mtedaldi.github.io/chatgpt-conversation-exporter/) embeds both automatically so no external requests are made at all.

## Export formats

### Markdown
Each conversation becomes a `.md` file named after its title. Messages are rendered as `**Du**` / `**ChatGPT**` sections separated by horizontal rules. Multiple selections are bundled as a ZIP.

### JSON
Each conversation is exported as a clean object with `id`, `title`, `create_time`, `update_time`, and `messages` (containing `role`, `text`, `time`, and optionally `images`). Multiple selections are exported as a JSON array.

### HTML (with images)
A self-contained `.html` file with all images embedded as Base64 data URIs. Opens in any browser, fully offline, no external dependencies.

### Markdown + ZIP (with images)
A ZIP containing one subfolder per conversation: a `.md` file with relative `![alt](images/filename)` image links, and an `images/` subfolder with the actual image files.

### JSON + ZIP (with images)
A ZIP containing `conversations.json` and an `images/` folder with all referenced image files.

## Image matching

ChatGPT exports use two different internal asset pointer schemes:

- `file-service://file-<base58id>` — older uploaded files
- `sediment://file_<hexprefix>` — newer uploads and DALL·E generated images

The tool handles both automatically. DALL·E images are stored in a `user-{id}/` subfolder inside the ZIP and are detected by reading `user.json`.

## Development

Developed with the assistance of [Claude](https://claude.ai) by Anthropic. The concept, requirements, and direction are by the author; implementation was generated and iterated with AI assistance. Intentionally dependency-free for end users — a single `index.html` with vanilla JS.

The published version on GitHub Pages is automatically built by a GitHub Actions workflow that inlines JSZip and embeds Google Fonts as Base64 using [embed-google-fonts](https://github.com/mtedaldi/embed-google-fonts), ensuring the deployed version makes zero external requests.

Contributions and issues welcome via GitHub.

## License

[MIT](LICENSE)
