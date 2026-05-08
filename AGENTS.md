# wwm-guide-lunar-capital — Agent Guide

Static Vietnamese-language build guide for the game **Where Winds Meet** (Mạc Đao / Stonesplit-Might tank focus). Single-page app, no build tools.

## Stack
- Vanilla HTML + CSS + inline JS (no framework, no bundler, no package.json).
- External: Tabler Icons webfont (CDN), Google Fonts (Noto Serif Display, Be Vietnam Pro), `bg.png` local.
- Page navigation = `data-page` buttons toggling `.page.active`. Mobile menu via `.hamburger` + `.scrim`.

## Conventions
- All user-facing copy is **Vietnamese**. Keep new strings in Vietnamese; do not auto-translate to English.
- Game-term glossary: *chí mạng* = crit, *hiểu ý* = affinity, *trắng* = raw stat on gear, *vàng* = effective stat after resist, *min/max thuộc tính khác hệ* = off-element min/max attribute roll.
- Color palette is theme-defined in `:root` of `styles.css` (gold / jade / cinnabar on inked background). Reuse the CSS variables — don't hardcode hex.
- Numbers (56%, 24%, 80%, etc.) come from in-game testing; treat as content data — do not "round" or "fix" them without the user confirming.

## Workflow
- Edit `index.html` and `styles.css` directly. There is no preview server beyond opening the file in a browser.
- Follow the global `<HOME>/ai-optimizer/AGENTS.md` loop: plan → research → build → record. Query `memory/index.json` first.
