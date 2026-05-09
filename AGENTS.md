# wwm-guide-lunar-capital — Agent Guide

Static Vietnamese-language build guide for the game **Where Winds Meet** (Mạc Đao / Stonesplit-Might tank focus). Single-page app, no build tools.

## Stack
- Vanilla HTML + CSS + inline JS (no framework, no bundler, no package.json).
- External: Tabler Icons webfont (CDN), Google Fonts (Noto Serif Display, Be Vietnam Pro), `bg.png` local.
- Page navigation = `data-page` buttons toggling `.page.active`. Mobile menu via `.hamburger` + `.scrim`.

## Conventions
- All user-facing copy is **Vietnamese**. Keep new strings in Vietnamese; do not auto-translate to English.
- Game-term glossary: *chí mạng* = crit, *hiểu ý* = affinity, *chính xác* = precision (the "prec" damage branch — opposite is *không chính xác*), *thường* = normal hit, *sát thương yếu* (colloquially also *đòn xám*) = abrasion (non-prec damage outcome), *trắng* = raw stat on gear, *vàng* = effective stat after resist, *min/max thuộc tính khác hệ* = off-element min/max attribute roll.
- Color palette is theme-defined in `:root` of `styles.css` (gold / jade / cinnabar on inked background). Reuse the CSS variables — don't hardcode hex.
- **Stat identity colors** (use these for anything visually representing a specific stat):
  - *chí mạng* / Critical → **yellow** (`--crit`, `--crit-bright`, `--crit-deep`).
  - *hiểu ý* / Affinity → **orange** (`--aff`, `--aff-bright`, `--aff-deep`).
  - *độ chính xác* / Precision → **gold-on-cream** (existing `tg-prc` palette).
  - Do **not** reuse `--cinnabar` (red) or `--jade` (green) for these stats — those two palettes are reserved for **rank quality** cues only: `case-best` / `case-worst`, `tag-best` / `tag-worst`, the rank-#1 / rank-#4 bars in `summary-bars`, and `.case-stat.highlight` inside best/worst cards. Mixing these two roles dilutes both signals.
- Numbers (56%, 24%, 80%, etc.) come from in-game testing; treat as content data — do not "round" or "fix" them without the user confirming.
- **Placeholder for new pages**: any new page or subpage created without real content yet must show a single `.warn-card` with `<i class="ti ti-tools"></i>` and the copy: *"Đang cập nhật: Phần này sẽ được bổ sung sau. Anh chị em quay lại sau nhé."* See the existing `Công pháp` / `Tâm pháp` / `Trang bị` placeholder pages for the exact markup to copy.

## Workflow
- Edit `index.html` and `styles.css` directly. There is no preview server beyond opening the file in a browser.
- Follow the global `<HOME>/ai-optimizer/AGENTS.md` loop: plan → research → build → record. Query `memory/index.json` first.
