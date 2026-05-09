---
date: 2026-05-09
slug: stat-color-identity-yellow-orange
files_touched:
  - styles.css
  - index.html
  - AGENTS.md
bugs_recorded: []
---

## User request

Change all color usage of Critical (chí mạng) → **yellow**, and Affinity (hiểu ý) → **orange**. Note the terminology + understanding to a markdown file so future sessions don't regress.

## Plan / decisions

- **Two roles, two palettes** — the cinnabar/jade colors were doing double duty: stat identity (crit/aff) AND rank quality (best/worst). The fix isn't a global red→yellow / green→orange replace; it's separating those two roles cleanly:
  - **Stat identity** (the new rule): `--crit*` (yellow) for chí mạng, `--aff*` (orange) for hiểu ý.
  - **Rank quality** (kept as-is): `--jade*` for "best", `--cinnabar*` for "worst" — `case-best`, `case-worst`, `tag-best`, `tag-worst`, `case-stat.highlight` inside best/worst cards, the rank #1 and rank #4 bars in `summary-bars`.
- **New CSS tokens** added to `:root` with a comment explaining the role separation:
  - `--crit: #d4ad2e` / `--crit-bright: #f0c83a` / `--crit-deep: #3e3010`
  - `--aff: #c46d2e` / `--aff-bright: #e88e4e` / `--aff-deep: #3e1e10`
  - Earthy yellow/orange tones picked to harmonize with existing gold/cinnabar/jade rather than a saturated primary yellow/orange (which would clash with the inked-warm palette).
- **Sites of stat identity that got changed** (this is the full inventory — verified by grep):
  - `.tg-crit` (chí mạng stat tile in "Ba chỉ số mục tiêu")
  - `.tg-aff` (hiểu ý stat tile)
  - `.key-card` (the big "56% effective chí mạng" card — content is explicitly crit-themed, so identity color follows)
  - `.row-total .row-val` (the +109.4% crit damage total in "Vì sao crit Mạc Đao mạnh gấp đôi")
  - `.dmg-flow-crit` and `.dmg-flow-aff` (Critical/Affinity nodes in the new damage-tree SVG)
  - Inline gradient on the `chí mạng` damage bar in the recommend page
  - Inline gradient on the `Hiểu ý` damage bar
- **Sites deliberately NOT changed** (rank quality, kept jade/cinnabar): `case-best` border + `case-stat.highlight`, `case-worst` border + `case-stat.highlight`, `tag-best` and `tag-worst` chips, the rank #1 (jade) and rank #4 (cinnabar) `sb-fill` gradients in the bottom comparison bars on the chỉ số tấn công page.

## Files changed

- `styles.css`:
  - `:root` — added `--crit`, `--crit-bright`, `--crit-deep`, `--aff`, `--aff-bright`, `--aff-deep` with an explanatory comment block right above them.
  - `.tg-crit` — background gradient + border + label + raw-text colors all switched from cinnabar family to crit-yellow family.
  - `.tg-aff` — same swap from jade to aff-orange.
  - `.key-card` — base background switched from `--cinnabar-deep` to `--crit-deep`; gradient overlay and box-shadow rgba's swapped from cinnabar to crit-yellow.
  - `.row-total .row-val` — color switched from `--cinnabar-bright` to `--crit-bright`.
  - `.dmg-flow-crit .dmg-flow-title-text` and `.dmg-flow-aff .dmg-flow-title-text` — fills switched to `--crit-bright` / `--aff-bright`.
- `index.html` — two inline `linear-gradient(...)` styles on `.dmg-f` updated: chí mạng row → yellow gradient `(#3e3010 → #f0c83a)`, Hiểu ý row → orange gradient `(#3e1e10 → #e88e4e)`. The two cinnabar/jade inline gradients in `summary-bars` (rank #1 and rank #4) are deliberately kept since they represent rank quality, not stat identity.
- `AGENTS.md` (project) — extended the "Color palette" bullet under Conventions with a new **Stat identity colors** sub-section listing the chí mạng → yellow, hiểu ý → orange, độ chính xác → gold-on-cream mapping, plus an explicit "do not reuse cinnabar/jade for stats" rule that names every rank-quality consumer that owns those palettes. The terminology is captured next to the existing game-term glossary so it loads on every session in this project.

## Open follow-ups

- Visual verification needed: open the recommend page (Chỉ số recommend), the chỉ số tấn công page, and the new Sát thương đầu ra page in a browser. Confirm crit reads as yellow, hiểu ý reads as orange, and the case-best/worst cards still read green/red.
- The `--gold-faint` token (#4a3e22) is now visually close to `--crit-deep` (#3e3010). If a future design overlays them, may need to bump one apart.
- If a future stat needs its own identity color, follow the same pattern: add a `--<stat>` family in `:root`, add a doc bullet under "Stat identity colors" in `AGENTS.md`. Don't recycle existing tokens.

## Cross-project lessons

None recorded — this is a project-specific design decision, not a reusable bug pattern. The rule lives in project AGENTS.md, not in `<HOME>/ai-optimizer/memory/bugs/`.
