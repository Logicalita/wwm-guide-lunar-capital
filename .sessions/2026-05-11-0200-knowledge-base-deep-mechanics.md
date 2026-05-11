---
date: 2026-05-11
slug: knowledge-base-deep-mechanics
files_touched:
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

Continue research on all paths' builds — complete the deep mechanics documentation left as stubs/pending from the previous session.

## Plan / decisions

- **All changes confined to `KNOWLEDGE.md`** — no HTML or CSS touched. This session is pure knowledge-base completion.
- **Priority order**: (1) fix naming inconsistencies that affect every lookup (Scale → Strength, Henglai → Dust, 7 → 8 paths), (2) replace the Bamboocut–Dust stub (most incomplete section), (3) add deep mechanics to Bamboocut–Wind and Bellstrike–Splendor.
- **Bamboocut–Dust replacement**: full rewrite from one-paragraph stub to complete section. Content sourced from prior research agent reports. Key mechanic is the Perfect Catch rhythm (throw → catch timing → enhanced throw → repeat) and Candlelight 5-stack mass Immobilize for AoE clearing. T0 AoE designation preserved with caveat that overall tier lists measure single-target boss DPS.
- **Bamboocut–Wind Flamelash**: the non-obvious insight is the Agility double-dip — Min Physical ATK scales with Agility normally, AND during Flamelash, Critical Damage Bonus also scales with Min ATK, making Agility doubly effective in rage state. This is the key stat priority decision for this path.
- **Bellstrike–Splendor Vagrant Sword**: +1% damage per Endurance point is easy to miss because Endurance is not listed as a primary combat stat — it's an exploration/career attribute in most UI views. Important to note for gear secondary selection.
- **Tier list correction**: Bamboocut–Dust noted as "B/S" (B overall, T0 AoE) rather than a single letter — avoids misleading players who use the path specifically for mob-clearing content.
- **PvP Bamboocut–Dust** left at B — the path's AoE strength doesn't translate well to 1v1 or small-group PvP where single-target burst matters more.

## Files changed

- `KNOWLEDGE.md` — 6 batches of edits, total ~250 lines added/replaced:
  1. **Section 7 rewrite**: heading updated "7 Paths" → "8 Paths"; table updated — Henglai row replaced with proper Bamboocut–Dust row (破竹尘, Everspring + Rope Dart, Ranged AoE, T0 AoE clearing), Stonesplit–Strength row added (裂石力, Snowparting + Phalanxbane, Close-Range DPS); note updated explaining Bamboocut Wind/Dust and Stonesplit Might/Strength are distinct pairs.
  2. **Global rename**: all "Stonesplit–Scale" → "Stonesplit–Strength" across sections 21/22/23/24 (was wrong English community name circulating in some sources).
  3. **Section 21 path table**: Stonesplit–Strength corrected; tier table note updated with Bamboocut–Dust T0 AoE caveat.
  4. **Bamboocut–Dust section**: full replacement. New content: Fading Crimson resource (stops regenerating during Flower Burial stance), Scarlet Spin throw-and-return (hits twice per throw), Perfect Catch mechanic (timing window on return), Candlelight 5-stack Immobilize (2+ simultaneous hits required), Unfettered Rope Dart burst role, Stars Align BiS gear, Inner Ways (Phantom Rally / Towline Sweep / Song of Tang / Light Anew), full standard rotation (solo vs. mob variants).
  5. **Bamboocut–Wind deep mechanics**: added sub-sections — Flamelash/Wrathful Form (Fortitude, Celestial Fury 5-hit, HP drain, +10% Crit Rate, +20% Crit DMG), Agility double-dip explanation, Sin→Karma debuff via Echoes of Oblivion (−10% Phys Def + −10% Bamboocut Resistance), Rodent Rampage Token of Gratitude/Vendetta Token economy loop.
  6. **Bellstrike–Splendor deep mechanics**: added sub-section — QQQ shield rotation via Sword Morph T2/T3, Vagrant Sword +1% damage per Endurance point, Nameless Art Tier 4 generating 3 Sword Energy charges per cast (triple Sword Horizon frequency), Nameless Spear as utility-only in PvE (swap away immediately after buff/CC application).
  7. **Tier list**: Bamboocut–Dust PvE row updated to "B/S — B overall; T0 AoE mob clearing via Candlelight mass-immobilize".

Final KNOWLEDGE.md: 1006 lines, 24 sections, all 8 paths fully documented.

## Open follow-ups

- None critical. KNOWLEDGE.md is considered complete as of this session for the current content scope.
- If v1.5+ patches change mechanics (especially Bamboocut–Dust as a new path), revisit the tier list and rotation sections.
- Bamboocut–Wind PvE tier: Game8 April 2026 lists B; some community sources (Metabattle) rate it higher. Current file keeps B with the understanding that PvP is S. Revisit if consensus shifts.

## Cross-project lessons

- **Tier lists must be qualified by content type** — a path can be T0 for one content category (AoE mob clearing) and B for another (single-target boss). A single-letter rating misleads. Always annotate when a build has strong role-specific performance outside its overall tier.
- **Agility double-dip in Flamelash** — worth remembering for any future build that has a scaling mechanic inside a buff window: the multiplier may scale off a stat twice via different formulas simultaneously. Always check if Crit DMG Bonus has an unusual scaling source during rage/empowered states.
