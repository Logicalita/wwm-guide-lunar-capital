---
date: 2026-05-09
slug: damage-page-summary-card
files_touched:
  - index.html
  - AGENTS.md
bugs_recorded: []
---

## User request

Add a "Tổng kết" (summary) card at the end of the Sát thương đầu ra page with the user-supplied takeaway: "Khi tỷ lệ chí mạng và hiểu ý vượt ngưỡng 100% cùng với độ chính xác 100%, đòn thường và đòn xám sẽ không xuất hiện, tối ưu hóa triệt để sát thương."

## Plan / decisions

- **Component choice**: a regular `.card` with an h2 "Tổng kết" + the punchline wrapped in the existing `.compare` callout (the same gold-tinted box the recommend page uses for the "chí mạng > Hiểu ý" takeaway). This reuses an established "key insight" idiom rather than inventing a new component, and the .compare visual weight is right for "this is the page's conclusion".
- **Icons**: `ti-trophy` on the h2 (mirrors the icon already inside the recommend page's .compare for a similar "win condition" feel), and `ti-target-arrow` inside the .compare to suggest "the goal you're aiming for".
- **Emphasis**: only the three stat names (`chí mạng`, `hiểu ý`, `độ chính xác`) wrapped in `<strong>`. Avoided bolding "100%" or "đòn thường / đòn xám" so the eye lands on the stat-identity terms; over-bolding flattens the hierarchy. Kept the user's comma-then-em-dash rhythm rather than restructuring the sentence.
- **New term "đòn xám"**: the user introduced this as a colloquial alias for "sát thương yếu" (abrasion). Same color family — both bone/grey-tinted — and "xám" literally means "grey" so the metaphor is direct. Preserved the user's word verbatim in the summary instead of normalizing to "sát thương yếu", because the rhetorical pairing of "đòn thường ↔ đòn xám" (both colloquial "đòn X" formulations) reads cleaner than mixing register with "đòn thường ↔ sát thương yếu".
- **Glossary update** in project AGENTS.md: extended the existing entry to `sát thương yếu (colloquially also đòn xám) = abrasion` so future sessions know the two terms refer to the same outcome and don't accidentally re-translate.

## Files changed

- `index.html` — appended a new `.card` immediately after the existing examples-card footer note (so it's the last block in `#page-chiso-damage` before the section closes). Card contains an h2 "Tổng kết" with `ti-trophy` icon, then a `.compare` block with `ti-target-arrow` icon and the user-supplied summary text with the three stat names emphasized.
- `AGENTS.md` (project) — extended the `sát thương yếu` glossary entry with a parenthetical noting the colloquial synonym `đòn xám`.

## Open follow-ups

- The summary asserts crit + affinity exceeding 100% combined eliminates Thường and Sát thương yếu. That's mathematically consistent with the formula in the same page (when `x + y ≥ 100`, then `z = 0`; when `a = 100`, `P(Sát thương yếu) = 0`), so no contradiction. If the user later refines the threshold (e.g. caps and overflow behavior in WWM), the summary copy may need to be tightened.

## Cross-project lessons

None recorded — this is project-local copy + glossary maintenance, not a portable lesson. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
