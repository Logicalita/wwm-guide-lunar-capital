---
date: 2026-05-09
slug: case-cards-standardize
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

Make Cards 2, 3, 4 in the "Tại sao min thuộc tính khác hệ = best" section of the Chỉ số tấn công page consistent with Card 1's design (the polished `case-stats` 3-column grid).

## Plan / decisions

- Standardize on Card 1's existing `case-stats` 3-tile grid (Min / Max / Trung bình, with "Trung bình" highlighted) — confirmed via AskUserQuestion.
- Cards 2 & 3: pick the "Min cùng hệ" / "Min ngoại công" scenario as the representative range; the alternate "Max …" scenario gets a one-sentence note appended to `case-desc` so the data isn't lost.
- Card 4 (`case-worst`): use `Case A` / `Case B` / `Trung bình` labels (+0 / 777.15 / 777.15) — confirmed by user. The Case A/Case B rationale moved from the deleted `case-variants` rows into `case-desc` prose.
- New CSS rule for `.case-worst .case-stat.highlight` — cinnabar border + bright cinnabar value text — to mirror the existing `.case-best` jade rule. Keeps the worst-card's red theme consistent with its left border and `tệ nhất` badge.
- Deleted dead `.case-variants`, `.case-variant`, `.case-variant-val` CSS — no remaining consumers after the markup change.

## Files changed

- `index.html` — rewrote three `<div class="case-card">` blocks inside `#page-general` ("Tại sao min thuộc tính khác hệ = best"). Each `<div class="case-variants">…</div>` block replaced with `<div class="case-stats">` containing three `<div class="case-stat">` tiles. Card 4's new `case-desc` absorbs the old Case A/Case B prose.
- `styles.css` — added two new rules: `.case-worst .case-stat.highlight { border-color: var(--cinnabar); }` and `.case-worst .case-stat.highlight .case-stat-val { color: var(--cinnabar-bright); }`. Deleted the `.case-variants`, `.case-variant`, `.case-variant strong`, `.case-variant-val` rules (~25 lines).

## Open follow-ups

- Mobile readability: 3-tile grid stays 3-up at all widths via `grid-template-columns: repeat(3, 1fr)`. If user finds it cramped on narrow phones, revisit (collapse to 2-up or stack on mobile). Not addressed this session.
- Card 4 labels (`Case A` / `Case B`) deviate from Cards 1-3 (`Min` / `Max`). This is intentional — the data shape genuinely differs. If this looks visually jarring during user review, consider relabeling or an inline tag.

## Cross-project lessons

None recorded this session — no bug pattern surfaced. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
