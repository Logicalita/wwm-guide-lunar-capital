---
date: 2026-05-11
slug: precision-first-tree
files_touched:
  - index.html
bugs_recorded: []
---

## User request

After verifying the actual game UI, restructure the damage probability tree to be **Precision-first**: Chính xác → {Hiểu ý, Chí mạng, Thường}, Không chính xác → {Hiểu ý, Sát thương yếu}. Confirmed: on Precision, Affinity is checked first (mutually exclusive with Crit); on Non-Precision, the same Affinity stat y is used. If x+y>100 on Precision, Normal cannot trigger.

## Plan / decisions

- **Restructure SVG to Precision-first**: Tấn công splits into Chính xác (a%) and Không chính xác (1−a%). Each branch directly lists its possible outcomes (3 on Precision, 2 on Non-Precision). Hiểu ý appears as **two separate outcome nodes** — one per branch — both labeled with conditional probability `y` to make it visually clear the same stat operates on both sides.
- **Confirmed mutual exclusivity**: Affinity wins over Crit on Precision hits. If x+y ≥ 100% on Precision, Normal probability is 0 (since (1−y)(1−x) = 0 when y=1 or x=1).
- **Probability values unchanged**: Both worked examples (Ex1: 20/64/16/0; Ex2: 40/21.6/32.4/6) match the new formula expressions. Math: P(Hiểu ý) = a·y + (1−a)·y = y; P(Chí mạng) = a(1−y)x; P(Thường) = a(1−y)(1−x); P(Sát thương yếu) = (1−a)(1−y). Sum = 1.
- **Outcome labels use conditional probabilities** (within each branch) — matches existing convention where Chí mạng showed "x%" (conditional on Precision). New labels: Hiểu ý=y, Chí mạng=(1−y)×x, Thường=(1−y)×(1−x), Hiểu ý=y, Sát thương yếu=1−y.
- **Formula note rewritten** to show joint (overall) probabilities for the 4-color summary, with explicit "Hiểu ý từ cả hai nhánh đã gộp chung" so readers don't double-count the two Hiểu ý outcome nodes.
- **No changes to chiso-rates or chiso-attrs**: They reference rates/caps and the optimal-Affinity heuristic for Crit builds. The heuristic (Hiểu ý = 100% − Crit − Direct Crit) is a useful linear approximation that still applies under the new tree.
- **No changes to KNOWLEDGE.md**: Section 2's hit resolution tree was already in Precision-first form (the previous session had already fixed it). The SVG was the lagging piece.

## Files changed

- `index.html`:
  - Intro paragraph for page-chiso-damage SVG: rewritten — "Mỗi đòn đánh xét Chính xác trước. Đòn Chính xác có thể ra Hiểu ý (xét trước), Chí mạng, hoặc Thường. Đòn Không chính xác chỉ có thể ra Hiểu ý hoặc Sát thương yếu — không bao giờ ra Thường."
  - SVG (viewBox 840×510): full structural replacement. New layout — Tấn công at left (15,248); branch nodes Chính xác at (215,148) and Không chính xác at (215,380); 5 outcome nodes on the right at x=610 with y values 68/158/248/320/440. 7 new branch paths replacing the previous 8 paths. Two `dmg-flow-aff` outcome nodes (Hiểu ý from each branch).
  - Note below SVG: updated — explains the in-branch ordering (Hiểu ý first, then Chí mạng vs Thường on Precision; Hiểu ý first, else Sát thương yếu on Non-Precision) and clarifies y is shared across both branches.
  - Formula note in examples card: switched from conditional to joint probability formulation. P(Chí mạng) = a × (1−y) × x; P(Thường) = a × (1−y) × (1−x); P(Sát thương yếu) = (1−a) × (1−y).
  - Tổng kết card: rewritten — Precision 100% is now framed as "push everything into the Precision branch and eliminate Sát thương yếu entirely"; then minimize P(Thường) by capping y and x and using Direct stats to exceed caps.

## Open follow-ups

- **Affinity check ordering on Non-Precision**: Confirmed by user — same y stat applies. If future patches introduce a separate Non-Precision Affinity Rate, the SVG and formulas need updating.
- **Direct Crit Rate / Direct Affinity Rate exact mechanics**: Mentioned as uncapped stats that exceed the 40%/80% caps. Their exact rolling order vs. normal Crit/Affinity rates is not documented here. Worth a dedicated callout if the user wants more depth.
- **chiso-rates optimal-Affinity formula**: Currently uses linear approximation `y = 100% − x − Direct Crit`. The exact formula for P(Thường) = 0 on Precision is `(1−y)(1−x) = 0`, requiring y=100% or x=100%. The linear heuristic is fine for the page's audience but mathematically imprecise.

## Cross-project lessons

- **Game-state verification beats source synthesis**: After three rounds of probability-tree edits sourced from research notes, the definitive correction came from the user opening the game and reading the actual UI. For game-mechanic docs, prefer in-game verification over aggregate-source research when the two diverge.
- **Mental-model alignment matters for tree diagrams**: The previous "Affinity-first hoisted to top" tree was mathematically equivalent to the Precision-first tree but visually misled the user about the actual gameplay decision order. When the math allows multiple equivalent representations, pick the one that matches the user's mental model of the system.
