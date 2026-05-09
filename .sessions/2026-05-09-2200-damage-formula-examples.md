---
date: 2026-05-09
slug: damage-formula-examples
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

Add two worked examples of the damage-output formula on the Sát thương đầu ra page (user provided `a, x, y` for each, asked me to derive the rest), plus a clarifying note that the values plugged into the formula are post-resist (the gold-bracketed effective values), not raw gear stats.

## Plan / decisions

- **Formula interpretation** — the SVG tree shows conditional probability within the Prec branch. Reading the math:
  - z is derived: `z = 100 − (x + y)` (within-Prec normal share)
  - Global probability of each leaf = product along the path:
    - `P(Chí mạng) = a × x / 100`
    - `P(Hiểu ý) = a × y / 100`
    - `P(Thường) = a × z / 100`
    - `P(Sát thương yếu) = 100 − a` (Non prec collapses to a single leaf)
  - The "Abrasion: 100% − y" label in the original source diagram is **not** a probability formula — taking it literally produces nonsense (e.g. for y=20 it'd give 80% abrasion alongside 80% crit). Treating Sát thương yếu as `100 − a` is the only reading that makes the four leaves sum to 100%. Worth flagging if the user has a different mental model.
- **Computed examples**:
  - Example 1 (`a=100, x=80, y=20, z=0`) → 80% / 20% / 0% / 0%.
  - Example 2 (`a=90, x=40, y=40, z=20`) → 36% / 36% / 18% / 10%.
  Both verified to sum to 100%.
- **Component reuse vs new** — wrapped each example in a new `.dmg-example` card-within-card and reused the existing `.dmg-r` damage-bar grid for the four outcome rows. One CSS override on `.dmg-example .dmg-r` widens the label column from 100px → 130px so "Sát thương yếu" (14 chars) doesn't truncate. Bar width = absolute probability (so the bar visually represents the % directly), which differs from the recommend-page `.dmg-r` usage (relative-to-max). Same component, different semantic — acceptable since the readout next to the bar (`80%` etc.) makes intent clear.
- **Bar gradients** reuse the stat-identity colors set in the prior session: yellow for chí mạng, orange for hiểu ý. Thường gets the existing muted warm grey (matches "Đòn thường" elsewhere). Sát thương yếu gets a darker drabber gradient (`#2a2218 → #5e564a`) to differentiate it from Thường visually — both are "non-stat-identity" outcomes but Sát thương yếu reads as the worst path.
- **User-supplied note** rendered as a `.warn-card` placed *between* the existing tree card and the new examples card. Rationale: it's context for reading the examples ("the values plugged into the formula are gold-bracketed effective stats, not raw"), so it sets up the reader before they hit the worked numbers. Used `ti-info-circle` icon — informational, not alarmist. Wrapped "(chỉ số trong ngoặc vàng)" in `<em>` for a soft visual aside.
- **Footer note** below the two examples spells out all four formulas in one line for quick reference and confirms the sum is 100% (since the "100% − y" label on the diagram itself doesn't read as a probability; this footer makes the actual math explicit so a careful reader can reconcile).

## Files changed

- `index.html` — extended the `#page-chiso-damage` section with three new blocks (warn-card + examples card with two `.dmg-example` blocks + footer formula note) inserted between the existing tree card and the section's closing `</div></section>`.
- `styles.css` — added a `.dmg-example` block beneath the damage-flow infographic styles. New rules: `.dmg-example` (container card-in-card), `.dmg-example-head` (number + input chips with dashed underline), `.dmg-example-num` (gold-bordered numeric pill, mirrors `case-num`), `.dmg-example-inputs` and `.dmg-example-inputs strong` (variable name styled in italic gold-serif to look like math notation), `.dmg-example-derived strong` (the derived `z` value tinted silk to mark it as "computed, not given"), and the `.dmg-example .dmg-r` grid override.

## Open follow-ups

- The probability formula `P(Sát thương yếu) = 100 − a` was inferred from the structure (only valid sum-to-100% reading). If the WWM math is actually different (e.g. abrasion damage carries a `(100% − y)` damage multiplier, not a probability), the bottom-row formula in the footer note needs to change. User should sanity-check against the in-game numbers.
- If a future session adds a third example with different `a/x/y`, just copy the second `.dmg-example` block and recompute.

## Cross-project lessons

None recorded — math interpretation specific to this game's mechanics; not a portable pattern. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
