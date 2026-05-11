---
date: 2026-05-11
slug: chiso-pages-fix
files_touched:
  - index.html
  - styles.css
  - KNOWLEDGE.md
  - .claude/CLAUDE.md
bugs_recorded: []
---

## User request

Read the Chỉ số pages, fix what is wrong using the knowledge base, and add more concise/infographic stat subpages for gamers who prefer visual reference over dense text.

## Plan / decisions

- **page-chiso-damage overhaul**: The SVG damage flow tree was mechanically wrong — it showed Hiểu ý as a sub-branch of Chính xác only. KNOWLEDGE.md confirms Hiểu ý fires on ALL hits (Precision and Non-Precision) and is checked BEFORE Chí mạng. Rewrote the entire SVG (viewBox 840×510) with correct tree: Attack → Hiểu ý roll (top branch) OR fail → Precision split → Chí mạng/Thường or Không chính xác → Thường✓/Sát thương yếu.
- **Probability formulas corrected**: Old formula assumed Affinity only on Precision hits. Correct: P(Hiểu ý)=y; P(Chí mạng)=(1−y)×a×x; P(Thường)=(1−y)×[a×(1−x)+(1−a)×conv]; P(STY)=(1−y)×(1−a)×(1−conv). Both examples recalculated with corrected values.
- **Crit multiplier corrected**: Guide showed ×150%; KNOWLEDGE.md says +35% damage = ×135%. Corrected in formula tile and explanatory note.
- **Two new subpages added**:
  - `page-chiso-rates` — "Ba tỷ lệ chiến đấu": 3-tile infographic (Chính xác/Chí mạng/Hiểu ý with their caps), Precision formula card, priority list, optimal Affinity formula for Crit builds.
  - `page-chiso-attrs` — "5 Thuộc tính gốc": 5 case-cards with per-point conversions, Kình lực vs Thế comparison.
- **User feedback during plan review**:
  - Precision formula: `65% + gear_prec ÷ (1 + level_resistance)`; example at lvl 91 ≈ 99.48%.
  - Hiểu ý 40% is a hard cap; optimal for Crit builds = `100% − Chí mạng Rate − Direct Chí mạng Rate`. Exceeding the cap requires Direct Affinity Rate (separate stat, uncapped).
  - Attribute Vietnamese names corrected: Kình lực (Power), Mẫn (Agility), Thế (Momentum).
  - Thể lực HP/point: unknown — shown as "+? / điểm, đang nghiên cứu".
- **CSS min-width updated**: SVG viewBox widened from 800→840; updated `.dmg-flow { min-width }` from 580px → 620px.
- **KNOWLEDGE.md updated**: Fixed line 92 optimal stat target (added hard cap note + Direct Affinity Rate); fixed line 117 Affinity +~20% → +35%.

## Files changed

- `index.html` — 13 edit batches:
  1. Nav: 2 new nav-child buttons (chiso-rates, chiso-attrs)
  2. page-chiso-damage intro paragraph: corrected to explain Hiểu ý fires first on all hits
  3. SVG: full replacement (viewBox 840×510, 5-outcome correct tree)
  4. SVG note: fixed — Hiểu ý independent of Chính xác
  5. Crit multiplier: ×150% → ×135%
  6. Crit note: updated to 1.35× / +35% damage
  7. Example intro: z → conv variable rename
  8. Example 1: rows reordered (Hiểu ý first), values corrected (Chí mạng 80%→64%, Thường 0%→16%)
  9. Example 2: rows reordered, values corrected (Hiểu ý 36%→40%, Chí mạng 36%→21.6%, Thường 18%→32.4%, STY 10%→6%)
  10. Formula note: corrected to full 4-branch probability notation
  11. Tổng kết card: updated to reflect correct P(Thường)≈0 goal
  12. NEW `page-chiso-rates`: 3-tile grid + precision formula card + priority list + optimal affinity callout
  13. NEW `page-chiso-attrs`: 5 case-card attribute blocks + Kình lực vs Thế comparison card
- `styles.css` — `.dmg-flow { min-width }`: 580px → 620px
- `KNOWLEDGE.md` — line 92: optimal stat target note rewritten with hard cap + Direct Affinity Rate; line 117: Affinity +~20% corrected to +35%
- `.claude/CLAUDE.md` — (previous session) added session-start step 6 (read last 3 `.sessions/` files) and session-end step 1 (write session file)

## Open follow-ups

- **Thể lực HP/point**: No source found. Shown as unknown. Revisit if community documents this stat.
- **Crit ×135% in-game verification**: Changed from ×150% based on KNOWLEDGE.md research. If in-game testing shows ×150%, revert the formula tile in page-chiso-damage.
- **page-chiso-rates — Direct Affinity Rate**: The callout mentions Direct Affinity Rate exists as an uncapped separate stat. No full explanation page yet. Consider a follow-up page or tooltip.

## Cross-project lessons

- **Always cross-check damage probability against Chinese sources before publishing.** English guides had the Hiểu ý/Chí mạng order inverted; KNOWLEDGE.md from CN sources was authoritative.
- **Infographic-first design works well for stat pages**: 3-tile grids + formula cards + `.pri` priority lists communicate the same content as 3 paragraphs in a fraction of the space. Use this pattern for any future stat reference pages.
- **Hard caps require special treatment in UI**: Saying "stop at 40%" is misleading if a separate Direct stat bypasses the cap. Always clarify which stat is capped and which is uncapped when both exist.
