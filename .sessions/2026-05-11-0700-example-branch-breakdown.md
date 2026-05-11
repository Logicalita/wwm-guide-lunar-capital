---
date: 2026-05-11
slug: example-branch-breakdown
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

After the SVG was restructured to a Precision-first tree, the worked examples still showed a flat 4-bar summary that didn't reflect the new branch structure. Fix the examples to match.

## Plan / decisions

- **Restructure each example as branch breakdown**: Sub-header for Nhánh Chính xác (a%), then 3 outcome rows (Hiểu ý / Chí mạng / Thường); sub-header for Nhánh Không chính xác (1−a%), then 2 outcome rows (Hiểu ý / Sát thương yếu); footer showing the combined Hiểu ý total + the 100% sum sanity check.
- **Show explicit calculation under each bar**: Each `.dmg-r` row gets a small `.dmg-calc` follow-up text showing the joint formula plugged with values. E.g. for Ex2 Chí mạng: `= a × (1−y) × x = 0.9 × 0.6 × 0.4`. The result `21.6%` stays on the right of the bar.
- **Bar widths use joint probability over ALL hits** (not within-branch conditional). This way all bars across both branches still visually sum to 100% on the page.
- **Empty branch handling for Example 1**: Since a=100%, the Non-Precision branch has 0 hits. Rather than show empty bars, render a one-line italic note: "Không có đòn nào ở nhánh này — Chính xác đạt 100%, Sát thương yếu bị loại bỏ hoàn toàn."
- **CSS additions** scoped to `.dmg-example` to avoid affecting other `.dmg-r` usage elsewhere: `.dmg-branch-head` (gold-soft uppercase divider), `.dmg-calc` (small muted text under the bar, aligned with bar's left edge at 142px = 130px label + 12px gap), `.dmg-branch-empty` (italic note for empty branches), `.dmg-branch-total` (gold-tinted callout for the summary row at the bottom of each example).
- **Intro paragraph for examples**: rewritten to make explicit that values are joint (already multiplied by branch probability), not conditional within-branch.

## Files changed

- `styles.css` (after `.dmg-example .dmg-r:last-child`): added 6 new selectors scoped to `.dmg-example` — `.dmg-branch-head`, `.dmg-branch-head:first-of-type`, `.dmg-branch-head strong`, `.dmg-calc`, `.dmg-branch-empty`, `.dmg-branch-total`, `.dmg-branch-total strong`. All use existing color variables (`--text-muted`, `--text-ivory`, `--gold`, `--gold-bright`, `--ink-border`); no hardcoded hex.
- `index.html` (page-chiso-damage, examples card): full rewrite of both `.dmg-example` blocks. Each example now has 2 `.dmg-branch-head` sections, per-outcome rows with `.dmg-calc` math, and a `.dmg-branch-total` footer. Example 1 includes a `.dmg-branch-empty` note for its (1−a)=0% Non-Precision branch.
- `index.html` (examples card intro paragraph): updated to mention the branch breakdown and joint-probability convention.

## Open follow-ups

- **Mobile layout for `.dmg-calc`**: The 142px margin-left aligns under the bar on desktop (130px label + 12px gap). On very narrow screens (<400px), the bar column shrinks but the label stays 130px, so 142px still places the calc text correctly. Verified visually on mobile mock at 375px — readable but tight.
- **Example with Direct Crit/Affinity Rate**: Not currently shown. If readers want to see how the optimal-Affinity formula on chiso-rates plays out in practice (y reaches 40% cap, then Direct Affinity Rate fills the remaining gap), a third example could be added later.

## Cross-project lessons

- **When restructuring a tree visualization, restructure the worked examples too**: The SVG and the examples are two views of the same probability model. Updating only the SVG leaves examples that still describe the old model (even if the final numeric outputs are mathematically identical).
- **Per-branch decomposition is more pedagogical than a flat outcome summary**: Showing how each value derives from `(branch probability) × (within-branch probability)` reinforces the multi-step decision tree. The flat 4-bar view loses this structure even though it has the right numbers.
