# MarkdownSite

A single-file markdown document browser. Drop your `.md` files alongside this viewer and browse them with working cross-document links, Obsidian wikilinks, callouts, and custom themes. No build step, no dependencies.

For full documentation, see the [README](README.md).

## Hash Routing

Documents load via the URL hash fragment. The hash is stripped, `.md` is appended, and the file is fetched relative to the viewer's directory.

| URL | Loads |
|---|---|
| `MarkdownSite/` | `WELCOME.md` (this page) |
| `MarkdownSite/#my-doc` | `my-doc.md` |
| `MarkdownSite/#notes/combat` | `notes/combat.md` |

If no hash is provided, the viewer defaults to `WELCOME.md`. If a document is not found, an error message is shown in the content area.

## Expected File Structure

Place your `.md` files in the same directory as `index.html`, or organize them into subdirectories. The viewer resolves all paths relative to its own location. Optional customization files control the header, footer, and theme.

```
MarkdownSite/
├── index.html          ← the viewer
├── WELCOME.md          ← default page (no hash)
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

## Cross-Document Links

Standard markdown links to `.md` files are automatically rewritten to use hash routing. A link like `[Combat Rules](rules/combat.md)` becomes a click that navigates to `#rules/combat` without a page reload.

Obsidian-style wikilinks are also supported. `[[rules/combat]]` and `[[rules/combat|Combat Rules]]` both work. Paths must be explicit - there is no vault-wide filename search, so `[[combat]]` only resolves relative to the current document's folder.

Cross-document anchors work too - `[link](other.md#section)` navigates to the document and scrolls to the heading.

## Images

Relative image paths resolve from the document's folder. External images (`https://...`) load directly. Images are styled to fit within the document width.

## Callouts

Supports 14 Obsidian-compatible callout types with optional collapsible variants:

> [!tip] Example Callout
> Content here with full markdown support - lists, tables, code blocks, etc.

Types: `note`, `abstract`/`tldr`, `summary`, `info`/`todo`, `tip`/`hint`/`important`, `success`/`check`/`done`, `question`/`help`/`faq`, `warning`/`caution`, `failure`/`fail`, `danger`/`error`, `bug`, `example`, `quote`/`cite`, `statblock` (D&D parchment theme)

Add a `-` after the type for collapsible callouts: `> [!warning]- Click to expand`

## Copy Buttons

Copy buttons appear on code blocks, images, and inline code - hover to reveal them.

## Toolbar

- **Rendered / Source** - toggle between rendered markdown and raw source
- **Thin / Normal / Wide** - document max-width: 600px / 800px / 1200px
- **Copy Link** - copies current URL (with hash) to clipboard

## Customization

The viewer is fully customizable without editing `index.html`:

- **HEADER.md** - If present, rendered as the page header above the toolbar. Delete it for no header.
- **FOOTER.md** - If present, rendered as the page footer below the content. Delete it for no footer.
- **STYLE.css** - If present, injected after the built-in styles. Override just the CSS variables you want to change - everything else keeps its defaults.

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

### Query Parameters

| Param | Effect |
|---|---|
| `?chrome=false` | Hides header and footer (toolbar stays) |
| `?style=NORD` | Loads `NORD.css` instead of `STYLE.css` |

Both can be combined: `?chrome=false&style=NORD#my-doc`. Query parameters persist across all internal navigation since hash changes don't affect the query string.

## Requirements

This viewer uses `fetch()` to load markdown files, so it **must be served over HTTP**. It will not work from `file://`. Any local HTTP server works - `python -m http.server`, VS Code Live Server, etc.

## Playground

See the [Playground](PLAYGROUND.md) for a full demo of every supported markdown feature - headings, text formatting, links, images, code blocks, lists, blockquotes, callouts, tables, and mixed content.

---

Built by [PromptFerret](https://promptferret.github.io). Part of the [PromptFerret Tools](https://promptferret.github.io/tools/) collection.
