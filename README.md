# MarkdownSite

A single-file markdown document browser. Drop it alongside a folder of `.md` files and browse them with working cross-document links, Obsidian wikilinks, callouts, and custom themes. No build step, no dependencies.

**Live:** [promptferret.github.io/MarkdownSite](https://promptferret.github.io/MarkdownSite/)
**Discord:** [Join our Discord](https://discord.gg/xed4bCazym) for support, discussion, and additional resources.

## What It Does

- Renders markdown files loaded via URL hash routing
- Supports standard markdown links, Obsidian `[[wikilinks]]`, and cross-document anchors
- 14 callout types (with collapsible variants) including a D&D statblock callout
- Customizable header, footer, and theme - all without editing `index.html`
- Copy buttons on code blocks, images, and inline code

## Quick Start

```bash
git clone https://github.com/PromptFerret/MarkdownSite.git
cd MarkdownSite

# Start any local HTTP server:
python3 -m http.server 8000
# or
npx serve .
```

Open `http://localhost:8000` in your browser.

**Important:** MarkdownSite uses `fetch()` to load `.md` files, so it **must be served over HTTP**. It will not work from `file://`.

## File Structure

Place your `.md` files alongside `index.html`:

```
your-site/
├── index.html          ← the viewer (copy from this repo)
├── WELCOME.md          ← default page (loaded when no hash)
├── HEADER.md           ← optional: rendered above toolbar
├── FOOTER.md           ← optional: rendered below content
├── STYLE.css           ← optional: CSS theme override
├── my-notes.md
├── campaign/
│   ├── session-01.md
│   └── session-02.md
└── rules/
    ├── combat.md
    └── spells.md
```

## How Routing Works

Documents are loaded via the URL hash fragment:

| URL | Loads |
|---|---|
| `your-site/` | `WELCOME.md` |
| `your-site/#my-notes` | `my-notes.md` |
| `your-site/#campaign/session-01` | `campaign/session-01.md` |

No page reloads - navigation is handled entirely client-side.

## Links and Wikilinks

Standard markdown links to `.md` files are automatically rewritten to use hash routing:

```markdown
[Combat Rules](rules/combat.md)     → navigates to #rules/combat
[[rules/combat]]                     → same, Obsidian wikilink syntax
[[rules/combat|Combat Rules]]        → with display text
[Jump to section](#my-heading)       → in-page anchor scroll
[link](other.md#section)             → cross-doc with anchor
```

Wikilink paths must be explicit - there is no vault-wide filename search.

## Customization

All customization works by adding files alongside `index.html` - no need to edit the HTML:

| File | Purpose |
|---|---|
| `HEADER.md` | Rendered as the page header (delete for no header) |
| `FOOTER.md` | Rendered as the page footer (delete for no footer) |
| `STYLE.css` | CSS overrides injected after built-in styles |

### Query Parameters

| Param | Effect |
|---|---|
| `?chrome=false` | Hides header and footer |
| `?style=NORD` | Loads `NORD.css` instead of `STYLE.css` |

Both can be combined: `?chrome=false&style=NORD#my-doc`

### Minimal Theme Override

A custom theme only needs to redefine the CSS variables it wants to change:

```css
:root {
  --bg: #2e3440;
  --surface: #3b4252;
  --accent: #88c0d0;
  --text: #eceff4;
}
```

## Callout Types

Supports 14 Obsidian-compatible callout types:

```markdown
> [!tip] My Tip
> Content here with full markdown support - lists, tables, code blocks, etc.

> [!warning]- Collapsible Warning
> Starts collapsed. Click header to expand.
```

Types: `note`, `abstract`/`tldr`, `summary`, `info`/`todo`, `tip`/`hint`/`important`, `success`/`check`/`done`, `question`/`help`/`faq`, `warning`/`caution`, `failure`/`fail`, `danger`/`error`, `bug`, `example`, `quote`/`cite`, `statblock` (D&D parchment theme)

## SEO Note

MarkdownSite uses hash routing (`#path/to/doc`), which means:
- Search engines only index the base page, not individual documents
- Meta tags (title, description, og:tags) are static - they describe the viewer, not individual documents
- Link previews will show the viewer's metadata, not the linked document's content

This makes MarkdownSite ideal for **internal docs, knowledge bases, campaign notes, and documentation** where search engine discoverability isn't needed. If you need SEO, use a static site generator instead.

If you host your own instance, update the `<title>` and meta tags in `index.html` to describe your content.

## Toolbar

- **Rendered / Source** - toggle between rendered markdown and raw source
- **Thin / Normal / Wide** - document max-width: 600px / 800px / 1200px
- **Copy Link** - copies current URL (with hash) to clipboard

## Contributing

Single HTML file - no build step, no package manager. Edit `index.html` and refresh.

1. Fork the repo
2. Make your changes to `index.html`
3. Test with a local HTTP server
4. Submit a pull request

Read `CLAUDE.md` for detailed architecture docs (markdown parser internals, link rewriting rules, callout system, copy button enhancement, customization file loading).

Use `PLAYGROUND.md` to test all supported markdown features.

## License

[MIT License](LICENSE) - Copyright (c) 2026 PromptFerret

## Links

- [All PromptFerret Tools](https://promptferret.github.io/tools/)
- [PromptFerret](https://promptferret.github.io)
