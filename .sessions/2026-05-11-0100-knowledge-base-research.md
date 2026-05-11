---
date: 2026-05-11
slug: knowledge-base-research
files_touched:
  - KNOWLEDGE.md
bugs_recorded: []
---

## User request

Do a big research project for this website: read the Where Winds Meet wiki (Fextralife) and all other relevant sources about all combat-related game knowledge, and produce a knowledge file for the agent in the local project folder.

## Plan / decisions

- **New file `KNOWLEDGE.md`** — agent-facing reference, not user-facing HTML. Lives at project root, readable by any Claude Code session to inform content decisions without re-researching the game every time.
- **Fextralife blocks all direct WebFetch** — every attempt to `wherewindsmeet.wiki.fextralife.com` returned "socket connection closed unexpectedly." Workaround: use `WebSearch` to extract content from search result snippets, then directly WebFetch alternative sites (Game8, Keengamer, Metabattle, AllThings.how, Boarhat, community calculators, Chinese-language sources).
- **Parallel agent architecture** — spawned up to 4 background research agents simultaneously, each focused on a different topic cluster. Main conversation handled all WebFetch calls directly since agents had narrower permissions.
- **Chinese sources for accuracy validation** — 游民星空, 16yanyun.com, 网易云游戏 used to cross-check hit resolution order. Chinese community is more authoritative on internal mechanics than English wiki (which had affinity/crit order wrong in some articles).
- **Two content waves**: first wave covered all core mechanics (20 sections); second wave added full per-path build documentation for all 8 paths with tables, rotations, and gear.
- **Key corrections found during research**:
  - Chinese game title is 燕云十六声 (Yān Yún Shíliù Shēng) — 百面千寻 was only the development-era working title.
  - Affinity is checked **BEFORE** Crit on Precision hits — they are mutually exclusive per hit. Many English sources had this backwards.
  - Affinity damage bonus is +35%, not +20% as some English sources stated.
  - There are 8 official paths, not 7 — Bamboocut–Dust (v1.4) is separate from Bamboocut–Wind.
  - Stonesplit–Strength (Snowparting + Phalanxbane) is a separate named path, not a variant of Stonesplit–Might.

## Files changed

- `KNOWLEDGE.md` — created from scratch; ~591 lines (first wave) expanding to ~750 lines (after second wave). Sections:
  1. Combat Fundamentals (core loop, Qi resource, attack glow types, parry system)
  2. Hit Resolution Tree (Precision → Affinity FIRST → Crit → Abrasion; full decision diagram)
  3. Damage Calculation (full multiplicative formula, all zones)
  4. Core Attributes (5 combat stats with per-point conversion rates)
  5. Derived Combat Stats (all named stats with caps: Affinity 40%, Crit 80%)
  6. Weapon Types (7 families with named weapons)
  7. Paths — 8 official paths table with CN names, weapons, roles
  8. Skills System (Martial Arts / Mystic Skills / Inner Ways)
  9. Gear System (slots, rarity, upgrade/tuning/mastery)
  10. All Named Gear Sets (10 sets with 2-pc/4-pc bonuses)
  11. Stonesplit–Might full build reference
  12. All Named Builds (Fextralife reference list)
  13. Divinecraft Elemental System (Fire/Water/Poison)
  14. Boss System (38 bosses, 3 types, Boss Talents system)
  15. Enemy Types
  16. Affinity Deep Dive
  17. Melodies of Peace progression system
  18. Healer Build (Silkbind–Deluge)
  19. Vietnamese ↔ English ↔ Chinese terminology glossary
  20. Key External Resources (30+ URLs)
  21. All Paths Build Reference — full tables for all 8 paths
  22. Build Tier List (PvE + PvP, April 2026)
  23. Universal Inner Ways Tier List
  24. Complete Gear Sets Reference

## Open follow-ups

- Bamboocut–Dust section was a stub — deep mechanics needed.
- Bamboocut–Wind Flamelash/Wrathful Form mechanics not fully detailed.
- Bellstrike–Splendor Sword Morph QQQ shield and Vagrant Sword scaling not documented.
- "Stonesplit–Scale" naming error still present (should be Stonesplit–Strength).
- Section 7 still listed 7 paths with "Henglai" instead of Bamboocut–Dust.

## Cross-project lessons

- **Fextralife blocks direct crawling** — always use WebSearch for snippet extraction + fetch alternative sites (Game8, Keengamer, Metabattle) for Where Winds Meet research. Recorded in memory.
- Chinese-language sources (游民星空, 16yanyun.com, 网易云游戏) are more authoritative than English wikis for internal mechanics on this game. Cross-validate any counter-intuitive mechanic against CN sources before publishing.
