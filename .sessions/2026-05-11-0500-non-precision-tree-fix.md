---
date: 2026-05-11
slug: non-precision-tree-fix
files_touched:
  - index.html
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

The damage probability tree was still wrong: a Non-Precision attack can only produce Affinity damage or Abrasion damage — never Normal. The "Thường ✓ (conv%) rescue" branch needs to be removed. Recalculate the formulas.

## Plan / decisions

- **Root error**: I had carried over the "Abrasion Conversion Rate rescues to Normal" claim from KNOWLEDGE.md (which sourced it from Fextralife). User confirmed this is incorrect — Non-Precision resolves to AFFINITY or ABRASION only.
- **SVG fix**: Remove the "Thường ✓" outcome node and its incoming path. Não chính xác now has a single output curve to "Sát thương yếu" — no branching.
- **Probability values unchanged**: Both worked examples already used conv=0, so the displayed percentages were already correct. Only the tree shape and formula notes (which referenced `conv`) needed updating.
- **Formula expression simplifies**: P(Sát thương yếu) = (1−y) × (1−a) — no more `× (1−conv)`. Net 4-branch sum still = 100%.
- **Sát thương yếu node positioned at y=405** (was y=448 with Thường ✓ at y=352 above). Centered on the Không chính xác output line for visual balance.
- **KNOWLEDGE.md hit resolution tree**: Non-Precision branch simplified — `Affinity FAIL → ABRASION HIT` direct, no Abrasion Conversion step. Added a ⚠️ correction callout explicitly flagging the Fextralife "rescue to Normal" claim as incorrect.
- **Abrasion Conversion Rate stat**: kept as an entry but marked exact mechanic as uncertain (pending in-game verification). The stat exists; the "rescue to Normal" interpretation was wrong. It likely affects Abrasion damage magnitude.
- **Strategic implications updated**: Push Precision to 100% becomes the only reliable way to eliminate the Abrasion floor (since Non-Precision is now binary: Affinity or Abrasion).

## Files changed

- `index.html`:
  - SVG paths: removed "Không chính xác → Thường (phục hồi)" path; replaced "Không chính xác → Sát thương yếu" with a single direct curve `M 524 397 C 552 397, 552 405, 582 405`.
  - SVG nodes: removed the "Thường ✓ (conv%)" `<g>` block entirely. Repositioned "Sát thương yếu" to (x=590, y=405) with formula "100%" (sole outcome of the branch).
  - SVG note: removed `conv` from variable list; added clarification that Không chính xác without Hiểu ý always becomes Sát thương yếu (never Thường).
  - Example intro: removed the "conv bỏ qua (≈ 0)" disclaimer.
  - Formula note: dropped the "(conv ≈ 0)" qualifier; appended clarification that Thường only comes from the Chính xác branch.
- `KNOWLEDGE.md`:
  - Section 2 hit resolution tree (lines 62–66): collapsed Non-Precision branch — `Affinity FAIL → ABRASION HIT` direct, no Conversion sub-branch.
  - Added ⚠️ correction callout below the tree explicitly debunking the Fextralife "rescue to Normal" claim.
  - Line 74 (Abrasion Conversion Rate rule): rewrote — mechanic uncertain; previous "rescue to Normal" claim was wrong; likely a damage-magnitude modifier; pending in-game verification.
  - Hit Type Quick Reference table (Abrasion row): note changed from "rescued by Abrasion Conversion Rate" to "only result of Non-Precision when Affinity fails".
  - Strategic Implications: "Abrasion Conversion Rate matters most for..." rewritten — Push Precision to 100% is the only reliable Abrasion mitigation; the stat itself is unverified.
  - Derived Combat Stats table (Abrasion Conversion Rate row): description updated to flag the Fextralife claim as wrong and note the actual mechanic is unverified.

## Open follow-ups

- **What does Abrasion Conversion Rate actually do?** The stat appears on gear but its actual effect is unknown. Three hypotheses: (a) modifies Abrasion damage magnitude (e.g., closer to Min instead of Min/2), (b) affects something else entirely, (c) deprecated/vestigial stat. Needs in-game testing or Chinese-source confirmation.
- **page-chiso-rates Direct Affinity Rate callout**: unchanged in this session. The optimal-Affinity-for-Crit-build formula is still valid under the new tree (and arguably more important, since Non-Precision can no longer be rescued to Normal).

## Cross-project lessons

- **Don't trust derived knowledge files end-to-end**: When the user previously flagged the Crit damage value as wrong (e011), I should have re-checked the whole hit-resolution tree at that point — not just the multiplier. Fextralife-sourced content is unreliable across the board, not just on one specific value.
- **For probability-tree UIs, the "what cannot happen" is as important as "what can"**: The "Thường ✓ rescue" path was a phantom outcome that misled readers about what stat to invest in. When documenting a decision tree, explicitly state when a branch terminates with a single outcome rather than implying additional sub-rolls exist.
