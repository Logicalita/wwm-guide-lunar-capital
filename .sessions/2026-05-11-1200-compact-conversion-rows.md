---
date: 2026-05-11
slug: compact-conversion-rows
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

The "Tỷ lệ chuyển đổi mỗi điểm" card on the chiso-attrs page took too much vertical scroll for the small amount of info it actually contains (5 per-point conversion rates). Compact it.

## Plan / decisions

- **New compact pattern**: One row per attribute, 2 columns — `name + EN translation + optional tag` | `per-point stat(s)`. Each row ~40-45px tall.
- **Dropped content**: The per-card case-roll labels ("Body — sinh tồn thuần túy") and case-desc paragraphs ("Nâng Max Physical ATK mạnh nhất — +1.35/điểm, hơn 50% so với Thế..."). Reason: the build-context info they contained is now redundant with the Khuyến nghị card (at the top of the page) and the Kình lực vs Thế / 40,4 vs gear comparison cards below. The per-point rate is the only message left here.
- **Kept content**: All per-point values (Body +72, Power +1.35, Defense +17/+0.5, Agility +0.9/0.076%, Momentum +0.9/0.038%); the tags (`bỏ qua` for Phòng thủ, `Crit build` for Mẫn, `Affinity build` for Thế); the stat-identity colors (Mẫn yellow border-left + Chí mạng yellow text on the rate; Thế orange border-left + Hiểu ý orange text on the rate); the English attribute names as a small italic muted "(Body)" alongside the Vietnamese name.
- **New CSS pattern** `.attr-conv-*` (distinct from the existing `.attr-cmp-*` comparison rows): single-line rows with border-left color theming, scoped media query to stack on narrow screens. Doesn't touch the existing `.case-card` styles — those remain valid for the Khuyến nghị card and chiso-damage page.
- **Decimal separator**: Switched from `.` to `,` to match the Vietnamese decimal convention already in use elsewhere on the page (e.g., the 40,4 / 2908,8 / 21,6 numbers in the comparison cards).

## Files changed

- `styles.css` (right after the `.attr-cmp-*` block):
  - Added `.attr-conv-list` (flex column container, gap 4px).
  - Added `.attr-conv-row` (2-col grid: `minmax(190px, 0.9fr) 1.1fr`, gap 16px, padding 10px 14px, border-left default muted).
  - Added `.attr-conv-row.attr-conv-crit` (yellow border-left) and `.attr-conv-row.attr-conv-aff` (orange border-left).
  - Added `.attr-conv-name` (serif, silk color, flex inline with tag/EN) and `.attr-conv-name .en` (small muted italic).
  - Added `.attr-conv-stats` (gold-bright serif).
  - Added mobile media query stacking the 2 cols.
- `index.html` (page-chiso-attrs, "Tỷ lệ chuyển đổi mỗi điểm" card):
  - Replaced the 5 `.case-card` blocks (each with `.case-head`, `.case-roll`, `.case-stats`, `.case-desc`) with 5 `.attr-conv-row` blocks.
  - Vietnamese decimal separator used: 1,35 / 0,076% / 0,038%.

## Open follow-ups

- **Unused CSS scoped rule**: `#page-chiso-attrs .case-stats { grid-template-columns: repeat(2, 1fr); }` is no longer used on the chiso-attrs page (the Khuyến nghị card uses case-cards without case-stats). It's harmless dead code; could be removed in a future cleanup pass but not urgent.
- **`.case-card.case-crit` / `.case-card.case-aff` CSS variants**: still used by the Khuyến nghị card's "Build Thế" card. Don't remove.
- **Consistency check**: If the user likes this compact-rows pattern, the chiso-damage page's case-card examples (the Min/Max/Trung bình build comparison cards) could potentially use a similar pattern. But those cards have 3 stat tiles per card with numeric values, which justifies a larger format. Leave as-is unless user requests.

## Cross-project lessons

- **Deduplication justifies compaction**: When the per-row descriptions are redundant with other cards on the same page, removing them removes vertical noise without removing information. The page becomes scannable as "what does each attribute do?" reference, with the explanatory cards (above and below) providing the deeper context.
- **Border-left as the identity carrier**: Stat-identity colors (crit-yellow, aff-orange) don't need a full theme override of the row — a 3px left border is enough to signal "this row is Mẫn/Crit-themed" while keeping the rest of the row visually consistent. Less paint, same readability.
