---
date: 2026-05-11
slug: tldr-reorder-and-verify
files_touched:
  - index.html
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

Two things:
1. The other 3 stat per-point rates (Defense, Agility, Momentum) were verified in-game and match the prior aggregated values — update KNOWLEDGE.md to drop the "pending verification" caveat.
2. Reorder the chiso-rates and chiso-attrs pages so the actionable cards lead, with reference data below (per my own recommendation in the previous turn).

## Plan / decisions

- **Verified-stat update**: Removed the "Source" column from the Stat Conversion Rates table since all 5 rates are now in-game verified — column became uniform. Replaced the long callout (which described Defense/Agility/Momentum as pending) with a short ✅ confirmation noting all 5 verified, with the Power +0.45 nuance preserved.
- **Reorder rationale**: Both pages had actionable / recommendation cards at the bottom after several reference cards. Gamer audience (per user preference "doesn't like to read much") benefits from leading with the TL;DR — recommendation visible immediately on page load, reference data accessible below for verification.
- **chiso-rates new order**: Thứ tự ưu tiên → Tối ưu Hiểu ý → Ba tỷ lệ và giới hạn → Công thức Chính xác. The two action cards lead; reference data follows.
- **chiso-attrs new order**: Khuyến nghị tối ưu stat theo build → Tỷ lệ chuyển đổi mỗi điểm → Kình lực vs Thế → 40,4 điểm vs gear. The single recommendation card leads.
- **Reorder technique**: Two-edit pattern per page — Edit 1 inserts the target card(s) at the top (creates temporary duplicate); Edit 2 deletes the originals from the bottom, using trailing section-close context (`</section>\n\n    <!-- comment -->\n    <section ...>`) as a unique anchor that only matches the bottom occurrence. No accidental duplicate/loss.

## Files changed

- `KNOWLEDGE.md` (Section 4):
  - Stat Conversion Rates table: dropped the Source column (now uniform — all 5 in-game verified).
  - Replaced the multi-line "in-game verified overrides prior research" callout with a single ✅ line confirming all 5 verified, noting Body was undocumented and Power was wrong by 50% in aggregated sources.
- `index.html`:
  - **page-chiso-rates**: Moved cards 3 (Thứ tự ưu tiên) and 4 (Tối ưu Hiểu ý) from the bottom to immediately after the hero. Reference cards (Ba tỷ lệ và giới hạn, Công thức Chính xác hiệu quả) now occupy positions 3-4.
  - **page-chiso-attrs**: Moved card 4 (Khuyến nghị tối ưu stat theo build) from the bottom to immediately after the hero. Reference cards (5-stat list, Kình lực vs Thế, 40,4 vs gear) now occupy positions 2-4.

## Open follow-ups

- **Audit other pages for the same pattern**: chiso-damage already leads with the SVG tree (which is data, not action) and ends with Tổng kết (action). Could potentially reorder, but the SVG IS the central content of that page, not really comparable. Leaving as-is.
- **Path build pages (page-tank, page-bambooc, etc.)**: Each has its own internal card order; haven't audited for action-vs-reference ordering. Worth a pass later if user wants consistency across the whole site.

## Cross-project lessons

- **Lead with the TL;DR works especially well for guide content with mixed depth**: When a page has actionable advice + dense reference data, putting the action first respects readers who want a quick answer and supports readers who want to verify the reasoning underneath. No content cost — just ordering.
- **Two-edit move pattern with trailing-context anchors**: When moving content within a file via Edit operations, use trailing context that only matches the original (source) position to disambiguate after the new (target) copy is in place. For HTML this is often the section/document boundary right after the source. Avoids the "first match" pitfall of Edit's old_string matching.
