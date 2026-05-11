---
date: 2026-05-11
slug: attribute-vs-gear-comparison
files_touched:
  - index.html
  - styles.css
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

In-game testing data for comparing 40.4 points of each attribute vs 1 max-roll gear stat at level 91. Plus stat-optimization recommendations per build type. Add as new card(s) on chiso-attrs page; update KNOWLEDGE.md.

Test data provided:
- 40.4 Body = +2908.8 HP > 2426 HP (1 max-roll gear) → Attribute > Gear
- 40.4 Power = +54.54 Max ATK < 63.8 Max ATK → Gear > Attribute
- 40.4 Mẫn = +36.36 Min ATK + 3.07% Crit (= 57% + 41% ≈ 98% of 1 max-roll gear slot) → ≈ Gear
- 40.4 Thế = +36.36 Max ATK + 1.54% Affinity (= 57% + 43% = 100% of 1 max-roll gear slot) → = Gear
- 40.4 Defense = irrelevant for all builds → Skip

## Plan / decisions

- **User-driven design choices** (collected via AskUserQuestion before implementing):
  - Translation for "passive requirement (200/225)" → **"ngưỡng kích hoạt thuộc tính thiên phú"** (user's preferred phrasing; literal: "talent passive attribute activation threshold").
  - Layout for the comparison: **single card with 5 compact rows** (over the alternative of 5 separate case-cards or inline rows in existing stat cards).
  - Defense card in the 5-stat list: **keep card, add 'bỏ qua' tag** (preserves per-point reference data; signals skip-tier visually).
- **Card placement**: New cards inserted *after* the existing "Kình lực vs Thế" card, since the new content (attribute-vs-gear efficiency + build recommendations) builds on the per-attribute info above. Order is now: per-point conversions → attribute-vs-attribute (Power vs Thế) → attribute-vs-gear → build recommendations.
- **Comparison row layout**: 3-column grid (name | content cells | verdict tag), where content has 2 sub-cells (attribute side + gear side). For multi-stat attributes (Mẫn, Thế), added a `ratio` line showing the percentage breakdown (57% + 41% = 98%, etc.). Defense row uses a single full-width skip message instead of 2 cells.
- **Verdict tag colors using rank-quality palette**:
  - `verdict-better` → jade (Attribute wins)
  - `verdict-worse` → cinnabar (Gear wins)
  - `verdict-equal` → gold (roughly equal)
  - `verdict-skip` → muted italic
  - This use of jade/cinnabar is appropriate per project rules — these are *rank quality* judgments, not stat-identity coloring.
- **Mobile responsive**: 3-column row grid collapses to single column under 720px; sub-cells also stack.
- **KNOWLEDGE.md**: Added "Attribute vs Gear Stat Efficiency" subsection under Section 4 with the test data table + "Build Optimization" subsection with the same 3-tier guidance (Body/Power/Agility builds → cap and switch to gear; Momentum builds → flexible; Defense → skip).

## Files changed

- `styles.css` (after the `#page-chiso-attrs .case-stats` rule):
  - Added `.attr-cmp-list` flex container.
  - Added `.attr-cmp-row` 3-column grid layout + `.attr-cmp-name`, `.attr-cmp-cells`, `.attr-cmp-cell` styles (with `.lbl`, `.val`, `.ratio` inner classes).
  - Added `.attr-cmp-skip-msg` for the full-width Defense message variant.
  - Added 4 verdict tag classes: `.verdict-better` (jade), `.verdict-worse` (cinnabar), `.verdict-equal` (gold), `.verdict-skip` (muted).
  - Added `@media (max-width: 720px)` stack rule for mobile.
- `index.html`:
  - Phòng thủ card in the 5-stat list (page-chiso-attrs): added `<span class="tag tag-worst">bỏ qua</span>` to the case-head.
  - After the "Kình lực vs Thế" card, inserted 2 new cards:
    1. **40,4 điểm thuộc tính vs 1 stat max-roll trang bị (cấp 91)** — 5-row comparison table with verdicts.
    2. **Khuyến nghị tối ưu stat theo build** — 3 case-cards: (a) Build Thể lực/Kình lực/Mẫn (cap at threshold, then gear stats); (b) Build Thế with `case-aff` color (flexible — attribute and gear are equal); (c) Phòng thủ with `tag-worst` "bỏ qua" tag.
- `KNOWLEDGE.md` (Section 4, after the "In-game verified" callout):
  - Added "Attribute vs Gear Stat Efficiency" subsection with the 5-row comparison table.
  - Added "Build Optimization" subsection with 3-tier build guidance.

## Open follow-ups

- **Per-Inner-Way threshold values**: The 200/225 threshold varies by Inner Way (公 pháp). KNOWLEDGE.md and the recommendation card state this range but don't list which Inner Ways use 200 vs 225. A future enhancement could add a per-Path threshold reference once verified.
- **Defense / Agility / Momentum per-point re-verification**: Power was off by 50% in aggregated sources. The remaining attributes (Defense +17 HP + 0.5 Def, Agility +0.9 Min + 0.076% Crit, Momentum +0.9 Max + 0.038% Aff) are still flagged "Aggregated" in KNOWLEDGE.md; should be re-verified in-game when convenient. The 40.4-point comparison numbers in the new card assume these aggregated values are correct.
- **Confirm 63.8 Max ATK / 7.4% Crit / 3.6% Affinity / 2426 HP max-roll values**: The new comparison card uses these as "1 max-roll gear stat" baselines. They match the chiso-damage page's "+63.8 max roll" reference but should be confirmed against in-game stat tooltips on actual level-91 max-roll gear.

## Cross-project lessons

- **AskUserQuestion for translation/design before implementing UI copy is worth the round-trip**: The user explicitly invited it, and the answer ("ngưỡng kích hoạt thuộc tính thiên phú" — a custom phrasing they preferred) wasn't among my 3 pre-canned options. Volunteering option lists is fine; making the user override is also fine; not asking and getting the term wrong would have required a follow-up correction.
- **Comparison tables benefit from verdict columns**: Showing the numbers and the bottom-line verdict in the same row makes scannable "should I bother?" decisions trivial. The verdict-better/worse/equal/skip color coding (using the rank-quality palette intentionally) gives an at-a-glance read without needing to do mental math on the values.
- **One CSS pattern per question being answered**: The new `.attr-cmp-*` classes answer "is attribute or gear better for this stat?" — distinct from the existing `.case-card` (which answers "what does each attribute do?"). Inventing a new pattern instead of overloading `.case-card` keeps both semantics clear.
