# Toolbox

A collection of free, privacy-first developer tools that run entirely in the browser. No uploads, no accounts, no data ever leaves your device.

Live at **[lindetoolbox.com](https://lindetoolbox.com)**

---

## Tools

### Images & Documents

| Tool | Description |
|---|---|
| Image Compressor | Compress PNG, JPG, WebP and SVG files with live before/after size comparison |
| Image Resizer | Resize to exact pixel dimensions with contain, cover, stretch or pad fit modes |
| Image Converter | Convert between PNG, JPEG, WebP, AVIF, SVG, HEIC and BMP |
| ICO Favicon Generator | Generate a multi-size `.ico` favicon from any image format |
| PDF Compressor | Compress PDFs across four quality levels with live estimated size |
| File Diff | Compare any two text, code or PDF files with inline diff highlighting |

### Typography & Color

| Tool | Description |
|---|---|
| Font Format Converter | Convert between TTF, OTF, WOFF and WOFF2 — download all formats at once |
| Font Inspector | Inspect OpenType metadata: classification, weight, designer, license, glyph set |
| Font Pairing Explorer | Browse AI-suggested Google Fonts pairings with live preview |
| WCAG Contrast Checker | Check WCAG 2.1 AA/AAA contrast ratios with shade suggestions |
| Color Palette Generator | Generate harmonious palettes from a base color using HSL color theory |
| CSS Gradient Builder | Build linear and radial CSS gradients with a visual editor |
| Tints & Shades Generator | Generate tints and shades with CSS variable and design token export |

### Code & Web

| Tool | Description |
|---|---|
| Code Formatter | Beautify JavaScript, CSS and HTML with configurable options |
| Code Minifier | Minify JS, CSS and HTML with original vs minified byte counts |
| SCSS Compiler | Compile SCSS to CSS — supports variables, nesting, mixins and functions |
| Semantic HTML Converter | Replace div-soup with proper HTML5 semantic elements |
| Brand Assets Extractor | Extract color palette, typography, logos and metadata from any URL |
| XD → Figma Packager | Package XD artboard SVG exports into a numbered ZIP for Figma import |

---

## Stack

- **[Astro](https://astro.build)** — static site generator
- **[Tailwind CSS](https://tailwindcss.com)** — styling
- **[Alpine.js](https://alpinejs.dev)** — lightweight reactivity where needed
- **[Cloudflare Workers + Assets](https://workers.cloudflare.com)** — hosting and edge functions
- All tool logic runs client-side via vanilla TypeScript `<script>` blocks

---

## Development

```bash
npm install
npm run dev       # start dev server
npm run build     # build to dist/
npm run preview   # serve dist/ locally
npm run deploy    # build + deploy to Cloudflare
```

The Cloudflare Worker (`worker/counter.js`) handles two endpoints:

- `POST /fontpair` — AI font pairing suggestions (requires `ANTHROPIC_KEY` secret)
- `GET /POST /u?k=<key>` — usage counters via Durable Objects

---

## Project Structure

```
src/
  pages/
    index.astro           — Home page and tool directory
    tools/
      [slug].astro        — Fallback placeholder for unimplemented tools
      <slug>.astro        — One file per tool
  layouts/
    MainLayout.astro      — Shared header, sidebar and footer
  components/
    Breadcrumbs.astro
    FAQAccordion.astro
    QuickTips.astro
    ToolCard.astro
  data/
    tools.ts              — Master tool registry
    nav.ts                — Navigation structure
  lib/
    color.ts              — Color utility functions
    utils.ts              — Shared utilities
worker/
  counter.js              — Cloudflare Worker
```
