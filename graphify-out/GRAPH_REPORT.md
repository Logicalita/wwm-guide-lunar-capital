# Graph Report - wwm-guide-lunar-capital  (2026-05-28)

## Corpus Check
- Large corpus: 58 files · ~1,273,642 words. Semantic extraction will be expensive (many Claude tokens). Consider running on a subfolder.

## Summary
- 154 nodes · 260 edges · 8 communities (6 shown, 2 thin omitted)
- Extraction: 80% EXTRACTED · 20% INFERRED · 0% AMBIGUOUS · INFERRED: 52 edges (avg confidence: 0.85)
- Token cost: 278,791 input · 12,000 output

## Community Hubs (Navigation)
- [[_COMMUNITY_Guide Pages & Core Stats|Guide Pages & Core Stats]]
- [[_COMMUNITY_UI Conventions & Early Sessions|UI Conventions & Early Sessions]]
- [[_COMMUNITY_Combat Math & Path Knowledge|Combat Math & Path Knowledge]]
- [[_COMMUNITY_Knowledge-Base Research & Sources|Knowledge-Base Research & Sources]]
- [[_COMMUNITY_Project Rules & v1.7 Patch|Project Rules & v1.7 Patch]]
- [[_COMMUNITY_Stat Testing & Site Polish|Stat Testing & Site Polish]]
- [[_COMMUNITY_Local Settings Permissions|Local Settings Permissions]]
- [[_COMMUNITY_Project Settings Permissions|Project Settings Permissions]]

## God Nodes (most connected - your core abstractions)
1. `Session: Guild War + Tank Tổng quan + Source Audit` - 11 edges
2. `Section I — Path Optimization (8 sub-paths)` - 9 edges
3. `AGENTS.md — Agent/Stack Conventions` - 9 edges
4. `Stat-Identity Colors (crit-yellow / aff-orange)` - 9 edges
5. `KNOWLEDGE.md — Combat Knowledge Base` - 8 edges
6. `Session: Chỉ số Group and Damage Flow` - 8 edges
7. `Top DPS Tank Guide (Stonesplit-Might / Mạc Đao)` - 7 edges
8. `Tank · Chỉ số recommend` - 7 edges
9. `Chí mạng (Critical Rate stat)` - 7 edges
10. `8 Martial Arts Paths reference table` - 7 edges

## Surprising Connections (you probably didn't know these)
- `Game-term glossary (chi mang/hieu y/chinh xac/etc.)` --semantically_similar_to--> `Vietnamese-English-Chinese terminology table`  [INFERRED] [semantically similar]
  AGENTS.md → KNOWLEDGE.md
- `Khien Ti — Ngoc (Silkbind-Jade) changes` --conceptually_related_to--> `8 Martial Arts Paths reference table`  [INFERRED]
  capnhat-v17.html → KNOWLEDGE.md
- `Liet Thach — Quan (Stonesplit-Strength) changes` --conceptually_related_to--> `Stonesplit-Might (Mac Dao tank) full reference`  [INFERRED]
  capnhat-v17.html → KNOWLEDGE.md
- `Pha Truc — Phong (Bamboocut-Wind) changes` --conceptually_related_to--> `8 Martial Arts Paths reference table`  [INFERRED]
  capnhat-v17.html → KNOWLEDGE.md
- `Pha Truc — Tran (Bamboocut-Dust) changes` --conceptually_related_to--> `8 Martial Arts Paths reference table`  [INFERRED]
  capnhat-v17.html → KNOWLEDGE.md

## Hyperedges (group relationships)
- **Three combat rates govern the damage probability tree** —  [INFERRED 0.90]
- **Tank crit build relies on Mạc Đao passive, Thundercry martial art, and Rainwhispers set** —  [INFERRED 0.85]
- **Color rule chain governing stat visualization** —  [INFERRED]
- **Source-priority audit trail** —  [INFERRED]
- **Sát thương đầu ra damage-page feature arc** —  [INFERRED]
- **Stat-color identity arc** —  [INFERRED]
- **Class-guide dropdown structure arc** —  [INFERRED]
- **Damage probability tree iterative correction arc** —  [INFERRED]
- **Source-trust evolution (Fextralife purge to priority hierarchy)** —  [INFERRED]
- **Chỉ số stat pages design refinement** —  [INFERRED]

## Communities (8 total, 2 thin omitted)

### Community 0 - "Guide Pages & Core Stats"
Cohesion: 0.09
Nodes (36): Hiểu ý (Affinity stat), 5 Base Attributes (Body/Power/Defense/Agility/Momentum), Cập nhật v1.7 (Patch Note external link), 5 Thuộc tính gốc (5 Base Attributes), Sát thương đầu ra (Damage Output tree), Ba tỷ lệ chiến đấu (Three Combat Rates), Chỉ số (Stats) Section, Chỉ số tấn công (Attack Stats explainer) (+28 more)

### Community 1 - "UI Conventions & Early Sessions"
Cohesion: 0.12
Nodes (31): Project AGENTS.md (glossary + conventions store), Case Cards (case-stats / case-best / case-worst), Chỉ số Nav Group, AVIF Class-Icon-as-Nav-Icon Pattern (nav-icon-img), Damage-Flow Probability-Tree SVG Infographic, Damage-Output Probability Formula (P = a*x/100 etc.), Damage-Formula 2x2 Grid Card (dmg-formula-grid), Guide Healer thần Dropdown (Silkbind-Deluge) (+23 more)

### Community 2 - "Combat Math & Path Knowledge"
Cohesion: 0.13
Nodes (25): Sources of Truth pointer (read KNOWLEDGE.md first), Game-term glossary (chi mang/hieu y/chinh xac/etc.), Khien Ti — Lam (Silkbind-Deluge) changes, Khien Ti — Ngoc (Silkbind-Jade) changes, Liet Thach — Quan (Stonesplit-Strength) changes, Liet Thach — Uy (Stonesplit-Might) changes, Minh Kim — Anh (Bellstrike-Umbra) changes, Minh Kim — Hong (Bellstrike-Splendor) changes (+17 more)

### Community 3 - "Knowledge-Base Research & Sources"
Cohesion: 0.18
Nodes (21): Additive Rolling Model (single roll, independent bands), Damage Probability / Hit Resolution Tree, Fextralife Untrusted for WWM, Guild War Section, KNOWLEDGE.md Agent Reference File, 8 Official Paths Build Documentation, Placeholder Hiding via .is-hidden Utility, Source Trust / Priority Hierarchy (+13 more)

### Community 4 - "Project Rules & v1.7 Patch"
Cohesion: 0.17
Nodes (18): Reuse :root CSS variables — no hardcoded hex, AGENTS.md — Agent/Stack Conventions, In-game numbers are content data — don't round/fix, Cinnabar/Jade reserved for rank quality only, Stat identity colors (crit/affinity/precision), All user-facing copy stays Vietnamese, warn-card placeholder for empty pages, Arena Attunement & gear-slot Attunement tuning (+10 more)

### Community 5 - "Stat Testing & Site Polish"
Cohesion: 0.21
Nodes (17): Per-Point Stat Conversion Rates, CSS Cache-Busting (?v= query param), Chỉ số (Stats) Pages and Subpages, Colour Reservation Rule (jade/cinnabar = rank quality), Card Compaction / Infographic-First Design, Parallel Scoped CSS Class Families Pattern, Guild Page + Brand Gradient (Lunar Capital), In-Game Verification Over Aggregated Research (+9 more)

## Ambiguous Edges - Review These
- `Guild War · Tune & Kỳ thuật` → `Min/Max attack stat rule (min thuộc tính khác hệ = best)`  [AMBIGUOUS]
  index.html · relation: semantically_similar_to

## Knowledge Gaps
- **8 isolated node(s):** `allow`, `allow`, `Tank · Tâm pháp (placeholder)`, `Tank · Trang bị (placeholder)`, `Guild War · Mẹo & Thủ thuật (Tricks)` (+3 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **2 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What is the exact relationship between `Guild War · Tune & Kỳ thuật` and `Min/Max attack stat rule (min thuộc tính khác hệ = best)`?**
  _Edge tagged AMBIGUOUS (relation: semantically_similar_to) - confidence is low._
- **Why does `Stat-Identity Colors (crit-yellow / aff-orange)` connect `UI Conventions & Early Sessions` to `Stat Testing & Site Polish`?**
  _High betweenness centrality (0.104) - this node is a cross-community bridge._
- **Why does `Session: Guild War + Tank Tổng quan + Source Audit` connect `Knowledge-Base Research & Sources` to `Stat Testing & Site Polish`?**
  _High betweenness centrality (0.066) - this node is a cross-community bridge._
- **Why does `Colour Reservation Rule (jade/cinnabar = rank quality)` connect `Stat Testing & Site Polish` to `UI Conventions & Early Sessions`, `Knowledge-Base Research & Sources`?**
  _High betweenness centrality (0.052) - this node is a cross-community bridge._
- **What connects `allow`, `allow`, `Tank · Tâm pháp (placeholder)` to the rest of the system?**
  _25 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Guide Pages & Core Stats` be split into smaller, more focused modules?**
  _Cohesion score 0.08571428571428572 - nodes in this community are weakly interconnected._
- **Should `UI Conventions & Early Sessions` be split into smaller, more focused modules?**
  _Cohesion score 0.12258064516129032 - nodes in this community are weakly interconnected._