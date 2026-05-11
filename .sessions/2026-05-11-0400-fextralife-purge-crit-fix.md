---
date: 2026-05-11
slug: fextralife-purge-crit-fix
files_touched:
  - index.html
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

Fextralife wiki has wrongful/old information. Correct values: Crit hits deal +50% damage; Affinity hits deal +35% based on max attack. Remove Fextralife sources from the project and fix the Chỉ số pages/subpages.

## Plan / decisions

- **Reverting prior session's "fix"**: Session 0300 had changed Crit multiplier from ×150% → ×135% based on what KNOWLEDGE.md said (sourced from Fextralife at the time). The KNOWLEDGE.md value was wrong. Reverting Crit to ×150% / +50%.
- **Affinity stays at +35%** (Max attack × 1.35) — this value was correct in both the original guide and KNOWLEDGE.md.
- **Fextralife is now flagged as untrusted**: Added a warning in KNOWLEDGE.md header. All Fextralife URLs removed from the External Resources table. Section 12 heading "All Named Builds (Fextralife Reference)" simplified to "All Named Builds" (build names retained — those are valid even if the source is unreliable for mechanics).
- **page-chiso-rates and page-chiso-attrs**: No edits needed — neither page references the Crit damage multiplier, only rates/caps which are unaffected by this correction.
- **Probability examples in page-chiso-damage**: No edits needed — example probabilities (Hiểu ý 20%, Chí mạng 64%, etc.) are independent of damage multipliers.

## Files changed

- `index.html`:
  - Line 834 (Chí mạng formula tile): `× 135%` → `× 150%`
  - Line 850 (note under Sát thương theo từng nhánh): Rewrote — now states Hiểu ý uses max attack + 35% deterministic; Chí mạng = 1.5× a random Min→Max roll (+50% damage, applies to both damage and heals).
- `KNOWLEDGE.md`:
  - Line 3 (sources preamble): Removed Fextralife from the trusted-sources list; added a ⚠️ warning that Fextralife is not trusted for this game (outdated/incorrect mechanics observed, including wrong Crit damage values).
  - Line 116: Crit Damage = +50% (was +35%); applies to both damage and healing.
  - Line 156 (Derived Combat Stats table — Critical Rate row): "+35% dmg" → "+50% dmg".
  - Line 393: Section heading "12. All Named Builds (Fextralife Reference)" → "12. All Named Builds".
  - Section 20 (External Resources): Removed 5 Fextralife URLs (Fextralife Wiki master, Raging Mountain, Healing Rain, Spring Umbrella, Infernal Spring).

## Open follow-ups

- **Other unverified KNOWLEDGE.md content**: Some sections may still contain Fextralife-derived facts that survived the trust filter (e.g. gear set bonuses, build descriptions in Section 12). If any future content edit references KNOWLEDGE.md, cross-validate against Chinese community sources or in-game first.
- **Heal Crit multiplier**: Was previously documented as "+50% to healing values" separately from the +35% damage crit. Now that damage crit is also +50%, both are unified — but it's worth confirming in-game whether heal crit is exactly +50% or some other value.

## Cross-project lessons

- **Fextralife is unreliable for Where Winds Meet** — documented in KNOWLEDGE.md header. Any future research for this game should skip Fextralife and prefer Chinese community sources (游民星空, 16yanyun.com, 网易云游戏) + in-game verification.
- **Don't trust agent-built knowledge files as canonical**: KNOWLEDGE.md is research notes, not ground truth. When the user reports a guide-content error, check the in-game value and the user's correction before assuming KNOWLEDGE.md is right.
