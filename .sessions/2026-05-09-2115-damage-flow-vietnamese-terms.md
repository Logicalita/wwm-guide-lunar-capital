---
date: 2026-05-09
slug: damage-flow-vietnamese-terms
files_touched:
  - index.html
  - AGENTS.md
bugs_recorded: []
---

## User request

Translate the English terminology in the Sát thương đầu ra infographic to Vietnamese; ask before guessing on uncertain terms.

## Plan / decisions

- Confident-and-applied translations: Attack → Tấn công, Critical → Chí mạng, Affinity → Hiểu ý, Normal → Thường (matches the existing `Đòn thường` damage row in the recommend page; chose the shorter form for the diagram).
- **Asked the user** about two genuinely uncertain pairs (Prec/Non prec, and Abrasion) since multiple Vietnamese game-translation conventions exist. User picks: **Chính xác / Không chính xác** for Prec/Non prec, **Sát thương yếu** for Abrasion.
- Updated everywhere those English terms appeared in the section, not just the SVG nodes:
  - SVG title nodes (7 labels)
  - Hero `.sub` paragraph
  - Card intro paragraph
  - Footer note (also dropped the parenthetical "(Prec)" annotation since the standalone Vietnamese is now the primary)
- Did NOT touch CSS class names like `dmg-flow-crit` / `dmg-flow-aff` / `dmg-flow-abrasion` — those are internal hooks and translating would force broader refactor for zero user-visible benefit.
- Did NOT keep English in parentheses anywhere in the diagram. The project AGENTS.md glossary already provides the EN↔VN mapping for cross-reference; duplicating it in the diagram clutters the visual.
- Extended the AGENTS.md "Game-term glossary" line so the three new mappings (chính xác = precision, thường = normal, sát thương yếu = abrasion) survive across sessions and future agents pick the same Vietnamese spellings.

## Files changed

- `index.html` — three Edit blocks inside the `#page-chiso-damage` section:
  1. Hero `.sub` and the card intro `<p class="dmg-flow-intro">` — replaced "Prec / Critical / Affinity / Abrasion" mentions with their Vietnamese forms.
  2. SVG `<text>` nodes — seven labels translated (Tấn công, Chính xác, Không chính xác, Thường, Chí mạng, Hiểu ý, Sát thương yếu). Formula text (`a%`, `100% − a`, `z% (= 100% − (x+y))`, `x%`, `y%`, `100% − y`) left untouched — these are math, not terminology.
  3. Footer `.note` — variable-key sentence and the trigger-condition sentence rewritten to use Vietnamese term names.
- `AGENTS.md` (project) — extended the "Game-term glossary" bullet with three new mappings: *chính xác* = precision (and its opposite *không chính xác*), *thường* = normal hit, *sát thương yếu* = abrasion. Loaded automatically on every session in this project, so future agents won't re-pick alternate translations like "mài mòn" or "chuẩn xác".

## Open follow-ups

- Visual check on narrow widths: "Không chính xác" (15 chars at 24px serif) is now the longest middle-column label. The SVG `min-width: 580px` on `.dmg-flow` should still fit it inside the existing branch-end x-coordinate, but worth eyeballing on the live page.
- If a future stat page adds a non-Mạc Đao class that uses different in-game term spellings (e.g. cross-class glossary divergence), revisit the AGENTS.md glossary structure — it currently assumes one canonical Vietnamese spelling per game term.

## Cross-project lessons

None recorded — this is project-specific terminology, captured in project AGENTS.md as the single source of truth. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
