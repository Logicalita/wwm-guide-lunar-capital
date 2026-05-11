---
date: 2026-05-11
slug: compact-recommendation-rows
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

"For the chiso pages, compact what you think necessary." — open-ended invitation to apply the same compaction judgment used on the per-point card across all 3 chiso pages.

## Plan / decisions

- **Audit first, edit second**: Surveyed all 4 cards on each of the 3 chiso pages before touching anything. Identified one card worth compacting (Khuyến nghị tối ưu stat theo build on chiso-attrs); the rest were either already lean or would lose user-requested content.
- **Why only one card**:
  - **chiso-rates** is already compact — 4 cards (priority list 3 rows, optimal-affinity compare callout, 3-tile rate grid, formula key-card). Each card is intrinsically minimal.
  - **chiso-damage** has the SVG diagram (irreducible — it IS the content), a 2×2 formula tile grid (already grid-packed via `.dmg-formula-grid`), the worked Examples card (user-requested branch-breakdown format from session e014), and a single-callout Tổng kết. Compacting any of these would either lose information or undo prior work.
  - **chiso-attrs Khuyến nghị card** was the outlier: 3 case-cards stacked with case-head + multi-sentence case-desc, ~330px tall combined. Same kind of "tall card for short info" complaint that prompted the per-point compaction.
- **Pattern choice**: Created a new `.attr-rec-*` family parallel to `.attr-conv-*` from the previous session — same row-with-left-border style, but tuned for paragraph content rather than single-line stats (slightly wider col 2, line-height 1.6 for wrapping text).
- **Text trim**: The phrase "ngưỡng kích hoạt thuộc tính thiên phú" was repeated in all 3 case-descs; shortened to "ngưỡng thiên phú" on 2 of them since the comparison card above already establishes the full term. Other redundancies removed without losing info.

## Files changed

- `styles.css` (right after the `.attr-conv-*` block):
  - Added `.attr-rec-list` (flex column, gap 6px).
  - Added `.attr-rec-row` (2-col grid `minmax(190px, 0.7fr) 1.3fr`, padding 12px 16px, default muted border-left).
  - Added `.attr-rec-row.attr-rec-aff` (orange border-left for Thế build).
  - Added `.attr-rec-title` (serif, silk, inline with tag) and `.attr-rec-text` (small ivory, line-height 1.6, gold-bright strong).
  - Added mobile media query stacking the 2 cols.
- `index.html` (page-chiso-attrs, Khuyến nghị card):
  - Replaced the 3 `.case-card` blocks (case-head + paragraph case-desc) with 3 `.attr-rec-row` blocks.
  - Tightened wording in the descriptions; preserved all key recommendations (cap at threshold, switch to gear stats; Thế is flexible; Phòng thủ skip).

## Open follow-ups

- **Unused CSS dead code**: `#page-chiso-attrs .case-stats { grid-template-columns: repeat(2, 1fr); }` is no longer used (the Khuyến nghị card removed the last case-stats usage on chiso-attrs). Also `.case-card.case-aff` and `.case-card.case-crit` no longer used on this page (Khuyến nghị used to use case-aff). Both are harmless but worth a cleanup pass if visiting styles.css again. Not urgent.
- **chiso-damage Examples card**: Largest card by line count (~100 lines), but kept as-is because the user explicitly designed the branch-breakdown layout. If the user ever finds it too tall, possible compactions: drop the per-row calc text (= a × y = 0.9 × 0.4) since the joint formula note at the bottom captures it; or collapse the branch sub-headers.

## Cross-project lessons

- **Audit before editing for open-ended "compact" requests**: When the user says "compact what you think necessary," the right response isn't to compact everything — it's to survey and pick the highest-impact target with the least information loss. Reporting back which cards you considered and explicitly skipped (with reasons) makes the judgment visible to the user, so they can override if they disagree.
- **Parallel patterns beat overloading existing ones**: `.attr-conv-*` (single-line stat content) and `.attr-rec-*` (paragraph content) share design language but solve different layout problems. Creating a second pattern instead of trying to make one pattern handle both kinds of content kept each one simple. Same idea as `.attr-cmp-*` (comparison rows with verdicts) which is separate again.
