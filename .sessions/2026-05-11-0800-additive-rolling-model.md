---
date: 2026-05-11
slug: additive-rolling-model
files_touched:
  - index.html
  - KNOWLEDGE.md
bugs_recorded:
  - id: sequential-vs-additive-rolling
    summary: Used sequential-rolling model (P(Crit) = (1-y)x) instead of additive (P(Crit) = x). Affected SVG labels, example values, formula notes, KNOWLEDGE.md, and chiso-rates callout.
---

## User request

The model was wrong. Chí mạng = x (NOT (1−y)×x). If x+y=100, Normal damage cannot appear. The (1−y)×x formula was causing cascading errors across the SVG, examples, formula note, KNOWLEDGE.md key rules, and chiso-rates callout.

## Plan / decisions

- **Root cause identified**: Treated Crit and Affinity on a Precision hit as **sequential rolls** (Affinity first, Crit on remaining 1−y) — KNOWLEDGE.md's "Affinity is checked BEFORE Crit" was misinterpreted. The actual mechanic is an **additive partition of a single 0–100 roll**: Affinity occupies [0, y), Crit occupies [y, y+x), Normal is the leftover [y+x, 100]. Affinity "wins" only in the sense that it occupies the lower band — not in a separate roll.
- **Corrected formulas**:
  - P(Hiểu ý | Precision) = y
  - P(Chí mạng | Precision) = x (not (1−y)x)
  - P(Thường | Precision) = max(0, 1 − x − y)
  - P(Hiểu ý | Non-Precision) = y
  - P(Sát thương yếu | Non-Precision) = 1 − y
- **Joint probabilities updated**: P(Hiểu ý) = y, P(Chí mạng) = a × x, P(Thường) = a × (1 − x − y), P(STY) = (1 − a) × (1 − y).
- **Example values recalculated**:
  - Ex1 (a=100, x=80, y=20): Hiểu ý 20%, Chí mạng **80%** (was 64%), Thường **0%** (was 16%), STY 0%. x+y=100 → Thường vanishes — exactly the user's example.
  - Ex2 (a=90, x=40, y=40): Hiểu ý from Precision 36%, Chí mạng **36%** (was 21.6%), Thường **18%** (was 32.4%), Hiểu ý from Non-Precision 4%, STY 6%. Total Hiểu ý = 40%.
- **Strategic implication flipped**: Under additive model, hard caps (y=40 + x=80 = 120 ≥ 100) automatically eliminate Normal hits on a maxed Crit build. The earlier claim "Affinity crowds out Crit chance" was wrong — Affinity does NOT reduce Crit chance under additive rolling; they're independent bands.
- **chiso-rates "Tối ưu Hiểu ý" callout**: The formula `y = 100% − x − Direct Crit` was already correct under additive (it expresses x + y + d ≥ 100). Only the explanatory text needed rewording ("xét trước" → "dải xác suất độc lập").
- **KNOWLEDGE.md hit resolution tree**: Rewrote the Precision branch as three additive bands of a single roll. Replaced "Affinity FAIL → Roll Crit" with "Roll in [y, y+x)" notation. Added explicit warning that the sequential model is wrong even though some Chinese sources phrase it ambiguously.

## Files changed

- `index.html`:
  - SVG outcome formula text: Chí mạng "(1−y) × x" → **"x"**; Thường "(1−y) × (1−x)" → **"1 − x − y"**.
  - SVG comment on Hiểu ý outcome: dropped "xét trước Chí mạng" wording.
  - Intro paragraph (page-chiso-damage): rewritten to describe the single-roll additive partition explicitly. Highlights the "x + y ≥ 100% → Thường = 0" property.
  - Note below SVG: rewritten — "một roll duy nhất phân thành 3 nhánh độc lập, không xét tuần tự."
  - Example 1: Chí mạng 64→**80%**, Thường 16→**0%**; calc texts updated to `= a × x` and `= a × (1−x−y)`. Bar widths updated.
  - Example 2: Chí mạng 21.6→**36%**, Thường 32.4→**18%**; calc texts updated; bar widths updated. Branch-total sum now 40+36+18+4+6 = 100%.
  - Formula note: joint formulas updated. P(Chí mạng) = a × x; P(Thường) = a × max(0, 1−x−y).
  - Tổng kết card: rewritten to frame goal as "x + y ≥ 100% to eliminate Thường" — natural caps (y=40 + x=80 = 120) already satisfy this.
  - page-chiso-rates "Tối ưu Hiểu ý" callout: reworded "Hiểu ý xét trước Chí mạng" → "một roll duy nhất chia thành 3 dải xác suất độc lập"; formula `y = 100% − x − Direct Crit` retained (correct under additive); added clarifier that with Crit=80%, Hiểu ý=20% suffices to remove Normal — don't need to push Hiểu ý to 40% cap.
- `KNOWLEDGE.md`:
  - Hit resolution tree (lines 53–65): Precision branch rewritten as three additive bands of a single roll, not sequential rolls. Notation `Roll in [0, y) → AFFINITY` / `Roll in [y, y+x) → CRIT` / `Roll in [y+x, 100] → NORMAL`.
  - Key Rules (lines 70–76): rewritten to lead with the additive model and explicitly call out that "True Crit Chance = Precision × Crit Rate" (NOT × (1 − Affinity Rate)).
  - Added warning that some English sources show a sequential probability tree — do not use that model.
  - Hit Type Quick Reference table: Critical row note changed from "Only on Precision Hits, after Affinity fails" → "P = x (not (1−y)x)"; Crit damage updated to "×1.5 (+50% damage)" — was inconsistent across the file.
  - Strategic Implications: corrected the "Affinity crowds out Crit" claim (called out explicitly as a sequential-model artifact that does not apply); "Optimal stat target" rewritten — natural caps satisfy x+y ≥ 100 automatically.

## Open follow-ups

- **Verify in-game with extreme values**: If possible, test a build with x=80 + y=20 vs. one with x=80 + y=40. Under additive model, both should have P(Normal) = 0 on Precision; under sequential, the y=40 one would have lower Crit rate. This would empirically confirm additive.
- **Direct Crit Rate / Direct Affinity Rate rolling order**: Documented as uncapped stats that exceed soft caps. Under additive, they presumably add to the same partition (extending Crit band or Affinity band). Not verified in-game.
- **Crit Damage multiplier "1.5×"**: Updated in KNOWLEDGE.md table for consistency with the index.html page (was previously "× Crit multiplier"). User confirmed earlier that ×150% / +50% is correct. Worth confirming the same is true for healing crits.

## Cross-project lessons

- **"Mutually exclusive" can mean two different probability models**: Sequential ((1−y)×x) and additive (x) both produce mutually exclusive outcomes per hit. Reading "Affinity is checked first" in a Chinese source meant additive priority within a single roll — not a separate Affinity roll preceding the Crit roll. When the math implications diverge, verify the underlying mechanism (one die or two?) before picking a formula.
- **Trace cascades when a formula is wrong**: A wrong probability formula propagated into 5+ surfaces (SVG labels, two example sets, formula note, Tổng kết card, chiso-rates callout, KNOWLEDGE.md key rules, KNOWLEDGE.md strategic implications, and the tree diagram). Fixing the formula in one place isn't enough — every place that quotes or computes from it needs updating.
- **User flagged the contradiction cleanly with a thought experiment**: "If x+y=100, normal damage would not appear." That's a one-line litmus test for the additive model. Under sequential, x+y=100 (e.g., x=80, y=20) gives P(Normal) = (1−0.2)(1−0.8) = 16% ≠ 0. The litmus test instantly disqualifies sequential.
