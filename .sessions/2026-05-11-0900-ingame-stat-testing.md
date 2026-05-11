---
date: 2026-05-11
slug: ingame-stat-testing
files_touched:
  - index.html
  - styles.css
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

In-game testing produced canonical per-point values: Body = +72 HP/point, Power = +1.35 Max Physical Attack/point. Also for the 5 chỉ số chung (chiso-attrs page): remove "Vai trò" tile, rename "trần/sàn sát thương" → "max/min attack", change Crit-build / Affinity-build card colors to match Chí mạng (yellow) / Hiểu ý (orange), and update the Power vs Momentum comparison. Update KNOWLEDGE.md with new values and overwrite older aggregated research.

## Plan / decisions

- **In-game values override prior research**: Body HP/point was previously undocumented in every source (Keengamer, 16yanyun.com, etc.); Power had been listed at +0.9 Max ATK across all aggregated sources. Both are now verified in-game and marked with ✅ + 2026-05-11 timestamp in KNOWLEDGE.md. Defense/Agility/Momentum rates not yet re-tested — marked as "Aggregated" pending verification.
- **Power vs Thế is no longer a wash**: With Power at +1.35 and Thế at +0.9, Power gives **+0.45 more Max ATK/point** with no Affinity. Thế trades that 0.45 ATK for +0.038% Affinity Rate. The old "equal Max ATK, Thế adds Affinity" framing was wrong because the +0.9 Power value was wrong. New optimization rule for Affinity builds: stack Thế up to Affinity cap (~1053 points to hit 40%), then switch to Power for raw ATK.
- **Stat-identity card colors**: The Mẫn and Thế cards were using `.case-best`/`.tag-best` (jade green) for Crit-build/Affinity-build labels. This violates project rule — `--jade` is reserved for rank quality, not stats. Added new CSS variants `.case-card.case-crit` (Chí mạng yellow `--crit-bright`) and `.case-card.case-aff` (Hiểu ý orange `--aff-bright`) + `.tag-crit` and `.tag-aff` to match. The `.case-best`/`.case-worst`/`.tag-best`/`.tag-worst` classes remain for legitimate rank-quality contexts (used on chiso-damage page for "tốt nhất"/"tệ nhất" cases).
- **2-column grid for chiso-attrs case-stats**: Removed the "Vai trò" tile from all 5 cards, leaving 1–2 highlights per card. Scoped `#page-chiso-attrs .case-stats { grid-template-columns: repeat(2, 1fr); }` so the layout adapts without breaking the 3-column grid on chiso-damage's case-cards.
- **Vietnamese naming corrected in KNOWLEDGE.md**: Earlier session updated the index.html with user-confirmed VN names (Kình lực, Mẫn, Thế) but KNOWLEDGE.md still had stale entries (Lực, Nhanh, Động lực, just "Phòng"). Fixed now.

## Files changed

- `styles.css`:
  - Added `.tag-crit` and `.tag-aff` (crit-yellow / aff-orange tag variants).
  - Added `.case-card.case-crit` and `.case-card.case-aff` border-left color overrides.
  - Added `.case-crit .case-stat.highlight` and `.case-aff .case-stat.highlight` (with `-val` color overrides) so highlight tiles pick up the stat-identity color.
  - Added `#page-chiso-attrs .case-stats { grid-template-columns: repeat(2, 1fr); }` for the 2-stat layout.
- `index.html` (page-chiso-attrs):
  - Hero sub: dropped "và vai trò" — now just "Tỷ lệ chuyển đổi mỗi điểm trong chiến đấu."
  - Body card: Max HP "+? / điểm" → **"+72 / điểm"**; description rewritten — "Chỉ nâng Max HP, không cộng giáp. Nếu muốn vừa HP vừa giáp, dùng Phòng thủ."; removed Vai trò tile.
  - Kình lực card: case-roll "trần sát thương" → "max attack"; Max ATK "+0.9" → **"+1.35"**; description rewritten to reflect dominance over Thế (+50% more Max ATK); removed Vai trò tile.
  - Phòng thủ card: removed Vai trò tile only.
  - Mẫn card: classes `case-best` → `case-crit`, `tag-best` → `tag-crit`; case-roll "sàn sát thương" → "min attack"; description simplified; removed Vai trò tile.
  - Thế card: classes `case-best` → `case-aff`, `tag-best` → `tag-aff`; case-roll "trần sát thương" → "max attack"; description rewritten — "chấp nhận ít Max ATK hơn Kình lực (0.45/điểm) để đổi lấy Hiểu ý"; removed Vai trò tile.
  - Kình lực vs Thế comparison card: rewritten with new 1.35 vs 0.9 numbers — recommends Thế for Affinity builds until cap, then Power; recommends Power for Crit / raw damage builds.
- `KNOWLEDGE.md`:
  - Core Attributes table (Section 4): Vietnamese names corrected — Lực → Kình lực, Nhanh → Mẫn, Động lực → Thế, Phòng → Phòng thủ, Body now has Thể lực.
  - Stat Conversion Rates table: added Source column. Body row: **+72 Max HP** (in-game ✅). Power row split from Momentum: **+1.35 Max Physical ATK** (in-game ✅). Momentum row: +0.9 Max ATK + 0.038% Affinity (aggregated, pending re-verification). Defense and Agility unchanged with "Aggregated" source.
  - Added ⚠️ callout: in-game verified values for Body and Power override prior research; other rates pending verification.
  - Strategic Implications: rewrote the "Affinity builds" item with the new Power vs Thế trade-off — stack Thế to Affinity cap (~1053 points), then switch to Power.

## Open follow-ups

- **Re-verify Defense, Agility, Momentum per-point rates in-game**: KNOWLEDGE.md still marks these as "Aggregated" — if Power was wrong by +50%, the others may also be off. The previously assumed Agility (+0.9 Min ATK + 0.076% Crit) and Momentum (+0.9 Max ATK + 0.038% Affinity) values are now suspect.
- **Re-check Affinity Rate cap point cost**: With Momentum at +0.038% Affinity per point, hitting the 40% cap requires ~1053 points. This calculation depends on the 0.038% rate being correct — if Momentum is also under-reported (like Power was), the cap point cost changes.
- **Inverse-style ratio comparison**: A future enhancement could add a "ATK per Affinity %" or "raw ATK ratio" callout on the chiso-attrs page to make the trade-off mathematically explicit.

## Cross-project lessons

- **Aggregated sources can be wrong by significant margins, not just edge cases**: Power was +1.35 in-game but +0.9 in 2+ aggregated sources (Keengamer, 16yanyun.com). That's a 50% under-report — not a rounding error. If the user does in-game testing, treat their observed values as canonical and overwrite everything derived from research with a verified-source tag.
- **Source column in stat tables is worth the row width**: Adding a "Source" column to the stat conversion table immediately makes it obvious which values are in-game verified vs. inherited from research. This is cheap and pays for itself the next time we revisit the data.
- **Stat-identity colors should be enforced through CSS class names, not ad-hoc styling**: The Mẫn / Thế cards inherited rank-quality jade because they used `.case-best`. Creating `.case-crit` / `.case-aff` modifier classes makes the design intent explicit and prevents accidental jade reuse for stat-coded contexts. The same pattern would help for any future stat-themed UI (status bars, build-tier badges, etc.).
