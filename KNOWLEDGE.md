# Where Winds Meet — Combat Knowledge Base

> Agent reference file. All content sourced from Game8, Keengamer, Metabattle, AllThings.how, GamerGuides, community calculators, Steam guides, and Chinese-language sources (游民星空, 16yanyun.com, 网易云游戏).
> ⚠️ **Fextralife wiki is NOT a trusted source for this game** — it has been observed to publish outdated/incorrect mechanics (e.g. wrong Crit damage values). Always cross-validate against Chinese community sources.
> Chinese title: **燕云十六声** (Yān Yún Shíliù Shēng). Working title during development: 百面千寻.
> Developer: Everstone Studio. Publisher: NetEase Games.
> Release: November 14, 2025 (PS5 + PC); December 12, 2025 (iOS/Android). Setting: Fifth Dynasties and Ten Kingdoms, 10th-century China.
> Last updated: 2026-05-11

---

## 1. Combat Fundamentals

### Core Loop
Action RPG with observation-based combat. Real-time but rhythmic:
1. Enemy commits to attack
2. Player defends or repositions
3. Player punishes **once**
4. Reset and re-center — never overcommit after landing hits

### Key Resources
- **Qi (气 / Stance Meter)** — shown under Health bar; controls stamina for blocks, dashes, dodges; auto-replenishes when you stop blocking/dodging, attack, or stand still. When enemy's Qi is fully drained → Exhausted state. If your own Qi breaks → you become Exhausted and vulnerable.
- **Spirit / Vitality (精力)** — white circle resource in the Mystic Skills HUD; consumed by Mystic Skills; replenished by attacking, blocking, or deflecting; up to 4 Mystic Skills active at once
- **Fighting Spirit / Fury bars** — separate resource for certain weapon skills (e.g., Predator's Shield adds 2 bars)

### Attack Types by Glow
| Glow | Meaning | Response |
|------|---------|----------|
| Red aura | Parryable attack | Perfect Parry (time ~200ms before impact at brightest flash) |
| Gold/Black aura | Unblockable | Must Dodge — cannot be parried |
| None | Normal attack | Block or Dodge |

### Deflection (Parry) System
- Deflection is the **primary damage source** in most encounters — ripostes deal very high damage
- Perfect Parry on red-glow attack destroys enemy Qi bar → triggers counterattack knockdown
- Pressing parry during a skill animation immediately cancels the skill — useful for reactive defense mid-combo
- **Assist Deflect** — time-slow QTE assist for parry timing (limited-use bars shown under Qi meter); available on Story/Recommended difficulty; good for learning patterns
- **Safety technique**: Hold block while pressing parry — if you miss the parry window, the block still activates and reduces damage
- Exhaust tip: Do NOT immediately Execute after depleting enemy Qi — beat on them for a few seconds first, then hit the finisher before the window closes

### Spacing & Stamina
- Camera control and spacing matter more than damage output
- "Distance you control is better than armor you trust"
- Dodge on the **swing**, not the wind-up — dodging early is the most common mistake

---

## 2. Hit Resolution — The Precision Tree

Every attack resolves through a branching decision tree producing one of four colored damage numbers:

```
Every attack
├── PRECISION CHECK (精准判定) — Precision Rate vs enemy Judgment Resistance
│   ├── PASS → Precision Hit (scales off Max Physical Attack)
│   │   │   Single roll partitions into 3 additive bands:
│   │   ├── Roll in [0, y)        → AFFINITY HIT   [橙字 Orange, Max ATK + 35% bonus]
│   │   ├── Roll in [y, y+x)      → CRITICAL HIT   [黄字 Gold, random Min–Max × 1.5]
│   │   └── Roll in [y+x, 100]    → NORMAL HIT     [白字 White, random Min–Max ATK]
│   │       (when y + x ≥ 100%, Normal is impossible)
│   │
│   └── FAIL → Non-Precision Hit (scales off Min Physical Attack)
│       ├── Roll in [0, y)        → AFFINITY HIT   [橙字 Orange — same y stat as Precision]
│       └── Roll in [y, 100]      → ABRASION HIT   [灰字 Dark grey, Min ATK ÷ 2]
```

> ⚠️ **Important correction**: A Non-Precision hit can ONLY resolve to AFFINITY or ABRASION — never to a Normal (white) hit. The "Abrasion Conversion Rate → Normal Hit rescue" claim from Fextralife is **incorrect**.

### Key Rules
- **Crit and Affinity are ADDITIVE on a Precision hit, not sequential.** A single 0–100 roll partitions into three bands: Affinity occupies the first y%, Crit the next x%, Normal the remainder (1 − x − y). They are mutually exclusive per hit; Affinity wins ties by occupying the lower band. (Chinese source: *"如果某次攻击触发了会意效果，那么就不会再进行会心的判定"* — confirms mutual exclusivity, NOT sequential rolling.)
- **True Crit Chance = Precision Rate × Crit Rate** (NOT × (1 − Affinity Rate); the (1−y) factor only applies under a sequential-rolling model, which this game does not use).
- **Crits can ONLY occur on Precision Hits.**
- **Affinity triggers on both Precision AND Non-Precision hits** with the same `y` rate.
- **Affinity Damage** = Max Physical Attack × (1 + Affinity Damage Bonus), then +35% on top. Deterministic — not a random Min–Max roll.
- **Abrasion Conversion Rate** — exact mechanic uncertain; the Fextralife "rescue to Normal" claim is wrong (Non-Precision resolves to Affinity or Abrasion only). Likely affects Abrasion damage magnitude. Pending in-game verification.
- Effective Precision = 65% base + (Your Added Precision) / (1 + Enemy Judgment Resistance)
- **Stat caps:** Affinity Rate cap = **40%**; Critical Rate cap = **80%**; Precision Rate cap = 100%. Natural cap sum y+x = 120% → P(Normal | Precision) = 0 when both maxed.

> Note: Some English sources show Crit and Affinity as a sequential probability tree (P(Crit) = (1−y)×x). This is mathematically equivalent to additive ONLY if you treat "y" differently — DO NOT USE THE SEQUENTIAL MODEL when computing build optimization. Use additive: P(Crit | Precision) = x.

### Hit Type Quick Reference
| Color (CN) | Type | Damage Basis | Notes |
|------------|------|-------------|-------|
| 白字 White | Normal | Random roll Min–Max Physical ATK | Only on Precision hits, when roll falls in [y+x, 100] band |
| 黄字 Gold | Critical | Random Min–Max × 1.5 (+50% damage; +50% on heals) | Only on Precision hits; P = x (not (1−y)x) |
| 橙字 Orange | Affinity | Max Physical ATK + 35% bonus (deterministic) | Fires on both branches with same y rate; wins ties over Crit |
| 灰字 Dark grey | Abrasion | Min Physical ATK ÷ 2 | Only on Non-Precision when Affinity fails |

### Strategic Implications
- **Crit builds** need both Precision Rate AND Crit Rate high. True Crit rate = Precision × Crit Rate. Affinity Rate does NOT reduce Crit chance (additive model — they share Precision branch, but each gets its own band of the 0–100 roll).
- **Affinity builds** (会意流) don't need high Precision — Affinity fires on both branches at rate y. Stack **Momentum (Thế)** to push Affinity Rate toward the 40% hard cap (0.038%/point → ~1053 points to cap). Trade-off vs Power: Thế gives +0.9 Max ATK/point (vs Power's +1.35), losing 0.45 Max ATK/point in exchange for Affinity Rate. After Affinity caps, switch additional points to Power for raw Max ATK. Use Direct Affinity Rate (uncapped) to exceed the 40% cap.
- **Affinity does NOT crowd out Crit** under the additive model — both are independent bands. Old guides saying "high Affinity hurts Crit" were assuming sequential rolling and are wrong.
- **Mitigating Abrasion**: Push Precision toward 100% to eliminate Non-Precision hits. Min ATK matters most for builds with a wide Min/Max ATK gap (Max ATK–focused builds where Abrasion hits for ½ Min ATK can be catastrophic).
- **Optimal stat target (Crit builds):** Push Precision to 100%, then maximize x + y. Since cap y=40 + cap x=80 = 120 ≥ 100, hitting both caps naturally eliminates Normal hits on Precision. Direct Crit / Direct Affinity (uncapped) further boost x or y individually past the soft caps if available.

---

## 3. Damage Calculation

### Full Formula (PvE)
```
Final Damage = Base Damage
             × Crit Zone
             × Affinity Zone
             × Damage Bonus Zone     [(1 + General%) × (1 + Skill%) × (1 + Weapon%)]
             × Independent Zone
             × Damage Reduction Zone [(1−DR₁) × (1−DR₂) × ... — multiplicative stacking]
             × Penetration Zone      [(Penetration − Resistance)/200 if negative, /100 if positive]
             × Tuning Zone
             × Damage Deepening Zone
```

### Key Zone Details
- **Damage Bonus Zone:** Multiplicative between categories (General / Skill / Weapon), additive within same category. Damage bonuses and debuffs share this zone.
- **Penetration Zone:** Physical Penetration also affects fixed damage.
- **Enemy Defense Reduction:** `Enemy Defense / (Enemy Defense + 1000)` — soft cap curve.
- **Multiple DR sources** stack multiplicatively: `(1−DR₁) × (1−DR₂)` etc.
- **Crit Damage** = +50% damage (applies to both damage and healing values).
- **Affinity Damage** = +35% on top of Max Physical ATK (always deterministic — not a range roll).

---

## 4. Core Attributes (5 Combat Stats)

These are the five main leveled attributes that scale combat stats:

| Attribute | Vietnamese Equiv. | Primary Combat Effect |
|-----------|-------------------|----------------------|
| **Body** | Thể lực | Raises Max HP |
| **Power** | Kình lực | Raises Max Physical Attack (damage ceiling) |
| **Defense** | Phòng thủ | Raises Physical Defense + Max HP |
| **Agility** | Mẫn | Raises Critical Rate + Min Physical Attack |
| **Momentum** | Thế | Raises Affinity Rate + Max Physical Attack |

### Stat Conversion Rates (per point) — all in-game verified 2026-05-11 ✅
| Attribute | Effect per Point |
|-----------|-----------------|
| Body (Thể lực) | **+72 Max HP** |
| Power (Kình lực) | **+1.35 Max Physical ATK** |
| Defense (Phòng thủ) | +17 Max HP + 0.5 Physical Defense |
| Agility (Mẫn) | +0.9 Min Physical ATK + 0.076% Crit Rate |
| Momentum (Thế) | +0.9 Max Physical ATK + 0.038% Affinity Rate |

> ✅ **All 5 rates verified in-game on 2026-05-11.** Aggregated sources (Keengamer, 16yanyun.com) had Body undocumented and Power wrong by 50% (+0.9 instead of +1.35); Defense / Agility / Momentum matched. Power is NOT equal to Momentum in raw ATK — Power gives **+0.45 more Max ATK** per point but no Affinity Rate.

### Attribute vs Gear Stat Efficiency (in-game tested, level 91, max-roll gear, 2026-05-11)

How does **40.4 points of an attribute** compare to **1 max-roll direct gear substat** at level 91? This determines whether to spend gear slots on attribute boosters or direct stats.

| Attribute | 40.4 points produces | 1 max-roll gear slot | Verdict |
|-----------|---------------------|---------------------|---------|
| Body (Thể lực) | **+2908.8 HP** | +2426 HP | **Attribute > Gear** ✓ |
| Power (Kình lực) | +54.54 Max ATK | **+63.8 Max ATK** | **Gear > Attribute** ✗ |
| Agility (Mẫn) | +36.36 Min ATK + 3.07% Crit Rate | +63.8 Min ATK *or* +7.4% Crit Rate | **≈ Gear** (98%; split: 57% Min + 41% Crit) |
| Momentum (Thế) | +36.36 Max ATK + 1.54% Affinity Rate | +63.8 Max ATK *or* +3.6% Affinity | **= Gear** (100%; split: 57% Max + 43% Aff) |
| Defense (Phòng thủ) | (irrelevant — no build needs it) | — | **Skip** |

### Build Optimization (in-game tested, 2026-05-11)

**Body / Power / Agility-focused builds:**
- Raise the relevant attribute to the **"ngưỡng kích hoạt thuộc tính thiên phú"** (talent passive activation threshold) — typically **200** or **225** points depending on Inner Way (公 pháp).
- Do NOT exceed the threshold. Additional attribute points are inferior to gear substats.
- After hitting the threshold, allocate remaining gear slots to direct substats: **Min Phys ATK**, **Max Phys ATK**, or **Crit Rate** as appropriate.

**Momentum (Thế)-focused builds:**
- Either Momentum attribute OR direct Max ATK/Affinity Rate gear substats are viable — value per point is ~100% of a gear slot.
- This is the **only attribute** that matches gear substats in efficiency. Allocate based on convenience and Inner Way requirements rather than pure optimization.

**Defense (Phòng thủ):**
- **Skip entirely.** No Inner Way's talent passive threshold requires Defense, so the attribute and its gear substat are both unproductive uses of slots. Use spare attribute points on Body (for HP) or damage attributes.

### Exploration Attributes (11 total — non-combat)
No per-point combat benefit. Unlock Exploration Skills at threshold values.
- Resolve, Elegance, Perception, Coordination, Intelligence, Musicality, Constitution, Erudition, Memory, Mindset, Ambition (unused/future)

### Career Attributes
- **Scholar** sub-stats: Rant Tolerance
- **Healer** sub-stats: Muscle Diagnosis
- Increased by practicing professions; affect profession minigames only

---

## 5. Derived Combat Stats

| Stat | Description | Key Source |
|------|-------------|------------|
| **Precision Rate** | % chance each hit is Precise → gates Crits | Gear secondary stats |
| **Critical Rate** | % chance a Precision Hit crits (+50% dmg) | Agility attribute |
| **Critical Damage Bonus** | Multiplier applied on top of base Crit bonus | Gear / Inner Ways |
| **Affinity Rate** | % chance any hit (Precision or not) triggers Affinity | Momentum attribute |
| **Affinity Damage Bonus** | Multiplier on top of base Affinity bonus | Gear / Inner Ways |
| **Min Physical Attack** | Damage floor; Abrasion scales from this | Agility; flat gear |
| **Max Physical Attack** | Damage ceiling; Precision/Affinity scale from this | Momentum; flat gear |
| **Abrasion Conversion Rate** | Exact mechanic unverified — does NOT convert Abrasion to Normal hit (Fextralife claim was wrong). Likely modifies Abrasion damage magnitude. | Gear secondary stats |
| **Physical Defense** | Reduces incoming physical damage | Defense attribute + level + gear |
| **Max HP** | Total health pool | Body + Defense + gear |
| **Max Qi** | Player stagger meter — raised exclusively through gear | Gear only |
| **Balance** | Resistance to stagger/knockback | Gear |
| **Interruption Resistance** | Prevents crowd-control effects | Water Divinecraft + some Inner Ways |
| **Physical Penetration** | Reduces enemy defense | Gear secondary stats |

---

## 6. Weapon Types (7 Families)

| Weapon | Style | Strengths | Stat Family |
|--------|-------|-----------|-------------|
| **Sword** | All-rounder | Balanced offense/defense, smooth combos | — |
| **Spear** | Reach + Control | Wide arc thrusts and sweeps, utility/prep | Stonesplit |
| **Mo Blade (Heng Blade / Mạc Đao)** | Heavy/Slow | Massive per-hit damage, stagger, carve boss HP | Stonesplit |
| **Fan (Pháp Phiến)** | Ranged Support | Channels qi outward, ranged chip damage | Bamboocut |
| **Umbrella (Dù)** | Defense + Counter | Glide through projectiles, angle guards, mixed healing | Silkbind |
| **Twinblades (Song Kiếm)** | Fastest DPS | Lightning-fast, rage state, high Crit, poison application | Bellstrike |
| **Rope Dart** | Ranged Control | Pull, space, movement tricks, positioning control | Bamboocut |

### Named Specific Weapons
| Name | Type | Key Mechanic |
|------|------|-------------|
| **Thundercry Blade** | Mo Blade | Predator's Shield (25% Max HP shield, 8s); core tank weapon |
| **Stormbreaker Spear** | Spear | Storm Roar (taunt + 20%/40% DMG Reduction 16s); Drumbeat buff |
| **Snowparting Blade** | Tang Dao / Sword | Deflect-focused; Grave Frost mechanic; Throat-Piercing Art synergy |
| **Phalanxbane Blade** | Mo Blade | Burst damage; pairs with Snowparting Blade |
| **Infernal Twinblades** | Twinblades | Scales Min Physical ATK with Agility; Flamelash rage buff |
| **Everspring Umbrella** | Umbrella | Ranged AoE + mobility; pairs with Infernal Twinblades |
| **Soulshade Umbrella** | Umbrella | Self-healing sustain; solo-friendly tank option |

### Weapon Dual-Equip Notes
- Two Martial Arts active simultaneously (Primary + Secondary)
- Player can master all weapons but only 2 active in combat (4 skills each = 8 total)
- **Umbrella** can be deployed as a hovering turret while swapped to secondary weapon
- **Panacea Fan** allows self-healing mid-combat via weapon-swap

---

## 7. Paths (Class System — 8 Paths)

Where Winds Meet has no fixed class lock-in. A "Path" is a combination of 2 weapons + Inner Ways + gear. There are **8 Martial Arts Paths** (each with 2 weapons = 16 total weapon options).

| Chinese Name | English Name | Weapons | Role | Key Strengths |
|---|---|---|---|---|
| 鸣金影 (Splendor) | **Bellstrike–Splendor** | Nameless Sword + Nameless Spear | DPS Melee | High mobility, charged burst, single-target, kiting |
| 鸣金影 (Umbra) | **Bellstrike–Umbra** | Strategic Sword + Heavenquaker Spear | DPS Bleed/DOT | Wound stacking, Bleed explosion, Affinity procs, sustained ticks |
| 牵丝玉 (Jade) | **Silkbind–Jade** | Inkwell Fan + Vernal Umbrella | DPS Ranged | Sustained ranged, airborne combos, mobility |
| 牵丝玉 (Deluge) | **Silkbind–Deluge** | Panacea Fan + Soulshade Umbrella | Healer/Support | Full healing, revive, team DMG buff, group recovery |
| 破竹风 | **Bamboocut–Wind** | Infernal Twinblades + Mortal Rope Dart | Assassin DPS | Relentless flurry, Flamelash rage, top PvP burst |
| 破竹尘 | **Bamboocut–Dust** | Everspring Umbrella + Unfettered Rope Dart | Ranged AoE | Umbrella throw + Perfect Catch, Candlelight CC, T0 AoE mob clearing |
| 裂石钧 | **Stonesplit–Might** | Stormbreaker Spear + Thundercry Blade | Tank | Shields, DMG Reduction, Interruption Resistance, Taunt, AoE CC |
| 裂石力 | **Stonesplit–Strength** | Snowparting Blade + Phalanxbane Blade | Close-Range DPS | Anxi Army burst, deflect-centric, parry punish, Crit-based |

> Bamboocut–Wind and Bamboocut–Dust are two separate paths despite sharing the Bamboocut family name — different weapon pairs, different mechanics, different roles. Stonesplit–Might (tank) and Stonesplit–Strength (melee DPS) are similarly distinct.

---

## 8. Skills System (3 Types)

### Martial Arts Skills
- Weapon-specific; 4 per weapon; up to 8 active in combat (2 weapons)
- Core of the combat rotation

### Mystic Skills (40 total; boarhat.gg lists 50+)
Divided into 3 categories:
- **Offensive** — consume Vitality; active combat abilities
- **General** — utility; do NOT consume Vitality (e.g., Tai-Chi for revealing caves, Meridian Touch, Celestial Seize)
- **Movement** — traversal (Wall Run, climb, dash, Abyss Dive for underwater)

Up to **4 Mystic Skills** active at once. Vitality replenished by attacking, blocking, deflecting.

Notable offensive Mystic Skills:
| Skill | Type | CD | Vitality | Notes |
|-------|------|-----|---------|-------|
| Dragon Head | Single-target Burst | 120s | 100 | Unlocked after learning 35 Mystic Skills |
| Golden Body | Support/Shield | — | — | Panic button shield; pairs with Predator's Shield |
| Blinding Mist | Support/Assassination | 45s | 70 | Poison cloud, obscures vision, mass CC |
| Touch of Death | General/Assassination | — | 0 | Instant-kill weaker enemies from behind |
| Talon Strike | Offensive | — | — | Tank build Mystic |
| Soaring Spin | Offensive | — | — | Tank build Mystic |
| Wolflike Frenzy | Offensive | — | — | Tank build Mystic |
| Yaksha Rush | Movement | — | — | Gap close |

### Inner Ways (37 total, 3 rarities: Legendary / Epic / Rare)
Passive skills. Up to **4 equipped simultaneously** — synergy is critical.
- **Universal Inner Ways** — weapon-agnostic; work with any build
- **Martial Art-specific** — tied to specific weapon; tagged by Path

**How to unlock:** Torn Pages from NPC merchants, Legacy quests, Sanctum challenges, or Martial Arts Theft.

Notable Inner Ways:
| Name | Type | Effect Summary |
|------|------|---------------|
| **Morale Chant** | Universal | Stacking damage boost; fires with both weapons via constant attacking |
| **Art of Resistance** | Stonesplit | Extends shield duration by 4s (Predator's Shield: 8s → 12s = near 100% uptime) |
| **Flawless Defense** | Stonesplit | Unconditional flat damage reduction; scales up when HP < 60% |
| **Adaptive Steel** | Stonesplit | Blade-specific support |
| **Battle Anthem** | General | Damage-oriented support passive |
| **Exquisite Scenery** | Universal | Counterattack source during tough fights |
| **Frost-Clad Night** | Bellstrike | Enhances Snowparting Blade; rewards deflects + Grave Frost |
| **Steadfast Devotion** | Bellstrike | Boosts Phalanxbane Blade burst windows |
| **Throat-Piercing Art** | Bellstrike | After Snowparting deflect-counterattack: ignores 2 Physical Resistance + Crit DMG +2% for 8s, stacks ×3 |
| **Bitter Seasons** | Bellstrike | Applies poison; synergizes with Twinblades' high attack speed |
| **Insightful Strike** | Affinity | Boosts Max Physical ATK — efficient for Affinity builds |
| **Echoes of Oblivion** | Affinity | Boosts Affinity Damage directly |

---

## 9. Gear System

### Equipment Slots
Head, Chest, Waist, Leg, Foot, Glove + Weapons + Accessories (disc, pendant)

### Gear Rarity Tiers
Legendary (gold) > Epic (purple) > Rare (blue) > Common (green)

### Stat Lines
- **Primary stats** — fixed; define piece's core purpose
- **Secondary stats** — randomly generated; count scales with rarity and tier:
  - Tier 31+: 1 secondary stat slot
  - Tier 41+: 2nd slot tunable
  - Tier 51: 3rd slot
  - Tier 56: 4th slot

### Upgrade System
- **Slot Enhancement:** You upgrade the *slot*, not the item — upgrade carries over to any gear equipped in that slot
- **Mastery (Arsenal):** Old gear stored in Arsenal grants Mastery → passive attribute bonuses tied to your Path (e.g., Stonesplit Mastery for tank)
- **Divinecraft Upgrade System** — unlocks at level 56; choose one of 3 elements for bonus effects
- **Tuning** — gear refinement/customization
- **Breakthrough** — major power milestone; Breakthrough 10 is a notable prep point
- **Melodies of Peace** — node-based progression tree for additional stat unlocks

### Set Bonuses
- Most armor pieces belong to a 4-piece set
- 2-piece and 4-piece bonuses activate at threshold
- Can equip **two different sets simultaneously** (e.g., 2+2 or combining bonuses)

---

## 10. Gear Sets (All Named Sets)

| Set | Piece Count for Bonus | Effect |
|-----|----------------------|--------|
| **Eaglerise** | 4pc | Dealing DoT or healing → 1 stack (10s, up to 5): −1.2% damage taken per stack (−6% max). At 5 stacks: Eagle Guard — next hit's damage −90% (normal) / −45% (bosses), 30s ICD |
| **Beyond the Chill** | 2pc: +40 Max HP; 4pc | 4pc: After 10s without taking damage in combat → next hit taken −40%, and all damage within 2s after also −40% |
| **Mistwillow (Veil of the Willow)** | 4pc | Heavy Attack hit → Light Attack/Projectile +12% for 15s; Light Attack hit → Heavy +12% for 15s. Both active = Veil of the Willow: both bonuses apply simultaneously |
| **Rainwhisper** | 2pc: +40 Max HP; 4pc | 4pc: +10% Crit DMG + healing received; jumps to +25% while HP shield is active |
| **Moonflare** | 2pc + 4pc | Attacking while defending → 30% chance to generate HP shield (absorbs 10% Max HP, lasts 20s, 60s ICD). If shield exists: restore 2% HP instead |
| **Flawless Defense** | 2pc: +Physical Defense; 4pc | 4pc: −5% damage taken; additional −10% when HP < 60% |
| **Formbend** | 4pc | Extends shield duration by 2s. If Qi > 85% or Qi shield active: −20% all HP damage taken |
| **Jadeware** | 4pc | Casting a Martial Art Skill → Jadeware effect: +10% Affinity DMG. Deals +20% Affinity DMG to controlled targets or targets at < 40% Qi |
| **Hawking** | 4pc | When damage triggers Affinity → Hawking: +2% Physical ATK for 5s, stacks ×5 (max +10%) |
| **Shattered Ridge** | Gear bonus | Successfully deflecting or hitting a boss → Shattered Ridge (5s): +5% HP DMG; plus additional +5% for Light/Heavy Attack Varied Combos and Assist triggers |

---

## 11. Stonesplit-Might (Mạc Đao Tank) — Full Reference

### Identity
"Raging Mountain" / "Thunder Phalanx" archetype. Absorb damage, lock enemies, taunt, slow powerful counter-swings. Best in multiplayer co-op as frontline/aggro anchor.

### Weapons
**Primary: Thundercry Blade (Mạc Đao)** + **Secondary: Stormbreaker Spear**

Both must be equipped to track Stonesplit-Might progress.

### Key Skills

**Thundercry Blade:**
| Skill | Effect |
|-------|--------|
| **Predator's Shield** | Grants 25% Max HP shield for 8s; instantly adds 2 bars Fighting Spirit. If carrying Stormbreaker's Drumbeat buff, consumes Drumbeat and triggers Perception skill instead |
| **Galecloud Cleave** | Charged heavy attack — full sequence |
| **Avalanche** | Charged heavy finisher |

**Stormbreaker Spear:**
| Skill | Effect |
|-------|--------|
| **Storm Roar** | Sonic wave taunt: draws aggro; Shocks enemies 8s; grants player **20% damage reduction for 16s** (40% vs. multiplayer Campaign bosses) |
| **Fury Spear** | Applies Drumbeat buff (interacts with Predator's Shield) |

**Mystic Skills for Tank:** Golden Body, Blinding Mist, Talon Strike, Soaring Spin, Wolflike Frenzy, Yaksha Rush, Free Morph, Ghost Bind

### Core Rotation
```
Storm Roar (taunt + 20% DR)
→ Fury Spear (apply Drumbeat buff)
→ SWAP to Thundercry Blade
→ Predator's Shield (shield + Fighting Spirit)
→ Galecloud Cleave (charged)
→ Avalanche
```
Use **Golden Body** as panic button shield if needed.

### Inner Ways
| Inner Way | Role |
|-----------|------|
| **Morale Chant** | Baseline stacking damage |
| **Art of Resistance** | Extends Predator's Shield to 12s (matches cooldown = near 100% uptime) |
| **Adaptive Steel** | Blade-specific support |
| **Battle Anthem** | Additional damage passive |
| **Exquisite Scenery** | Counterattack source |

### Recommended Gear Sets
**Option A (Metabattle standard):**
- Rainwhisper Set — weapon, disc, pendant
- Formbend Set — helm, armor, greaves, bracer

**Option B (Game8 / community):**
- Rainwhisper + Moonflare — Moonflare generates shields → activates Rainwhisper's enhanced +25% Crit DMG/healing

**Flawless Defense Set** — alternative: −5% damage taken + −10% more when HP < 60%

### Stat Priority
Power → Affinity Rate → Stonesplit Attack → Max/Min Physical Attack → Critical Rate → Physical Penetration → Weapon/Skill Damage Boost

### Tank Ratings
- Solo PvE: A Tier (no healing — needs Soulshade Umbrella swap for solo sustain)
- Multiplayer PvE: S Tier (taunt + massive DR)
- PvP: A Tier

### Alternative for Solo: Thundercry Blade + Soulshade Umbrella
- Self-sufficient: passive healing + shield stacking
- Tradeoff: loses Storm Roar taunt (bad for co-op aggro management)
- Solo PvE: S Tier; PvP: B Tier

---

## 12. All Named Builds

| Build Name | Weapons | Path | Focus |
|------------|---------|------|-------|
| **Raging Mountain** | Thundercry Blade + Stormbreaker Spear | Stonesplit-Might | Tank, damage sponge |
| **Thunder Phalanx** | Thundercry Blade + Stormbreaker Spear | Stonesplit-Might | Heavy-hit bruiser tank |
| **Infernal Spring** | Infernal Twinblades + Everspring Umbrella | Bamboocut | AoE DPS + poison (Bitter Seasons) |
| **Snowparting Storm** | Snowparting Blade + Stormbreaker Spear | Bellstrike-ish | Deflect + interrupt + consistent damage |
| **Seasonal Snowparter** | Snowparting Blade variant | Bellstrike | Deflect DPS |
| **Spring Umbrella** | Umbrella-focused | Silkbind | Mixed sustain/damage |
| **Infernal Blade** | Twinblades variant | Bamboocut/Bellstrike | DPS |
| **Infernal Spear** | Spear variant | Stonesplit-Strength | Bruiser |
| **Midnight Slayer** | (details not confirmed) | — | — |

---

## 13. Elemental System (Divinecraft)

Three elements via consumable **Divinecraft items** (replenished from environment — fire sources, flowers, mushrooms). Not limited like healing items — use freely.

| Element | Enemy Effect | Player Bonus |
|---------|-------------|-------------|
| **Fire** | Consecutive attacks trigger explosion; ~double HP damage vs. other Divinecrafts | Duration +10s |
| **Water** | Stagnation; extra HP + Resilience damage; chance to penetrate shields | Increases player Interruption Resistance; Duration +10s |
| **Poison** | Poisoning; extra HP + Qi damage; breaks Qi Lock quickly; executions deal damage a second time | Duration +10s |

- **Fire Oil** — weapon tempered with fire; effective vs. flammable targets
- **Toxic Powder** — weapon tempered with poison; effective vs. shielded enemies
- Water attacks — shield penetration chance + Stagnation
- Divinecraft Upgrade System at level 56 (Gear > Develop menu) — choose one element for additional bonuses

---

## 14. Boss System

### Boss Types (38 total)
1. **Campaign Bosses** — main story; require energy to fight; reward story progression
2. **World Bosses** — open world; 9 across Qinghe and Kaifeng regions
3. **Quest Bosses** — only accessible during specific quests

### Boss Mechanics
- Most major bosses: **2–3 phases** with different movesets
- **Secondary Qi gauge** (beneath boss HP) — fills as you land attacks/parries; when maxed → boss enters **Exhausted state** → execute for massive damage
- **Red-glow attacks** → Perfect Parry → counterattack + knock down
- **Gold-glow attacks** → Cannot block/parry → must Dodge
- **Bell of Demoncalm** — weakens World Bosses in Qinghe region
- Elemental tactics: Fire Oil vs. flammable targets, Poison vs. Iron Body units, Water vs. shielded enemies

### Named Campaign Bosses
Heartseeker, Qianye the Witch, Dao Lord, Zheng E The Frostwing, Murong Yuan, God of Avarice, Void King (The Formless) — final boss

### World Bosses
Sleeping Daoist, Puppeteer Sheng Wu, Snake Doctor (+ others across regions)

### Secret Boss
**Miaoshan** — requires 6,000 Exploration Points in Qinghe Region + completing the Puzzle at Halo Peak Tower during Wu Hour

### Boss Talents System (Repeatable Content)
Separate progression layer for Sword Trial + Group Dungeon content. Per boss: 3 node types:
- **Offensive**: +10/20/30% damage vs. that boss when Exhausted
- **Defensive**: mitigation vs. a specific signature mechanic
- **Strategic**: +5/7.5/10/15% overall damage and healing

No respec — permanently unlocked. Unlocked via specific challenge completion during Sword Trial / Group Dungeon.

**Global Reward Talents** (Strategic nodes maxed across boss pairs):
- Formless + Seventeen: 10/20/30% extra Sword Trial chest chance
- Sleeping Daoist + Murong Yuan: 10/20% extra Attune Stone chance
- Twin Lions + Ghost Prince: 10/20% extra gear chance
- All three maxed: Custom gear selection chest at Solo Mode level 10

---

## 15. Enemy Types

- **Humanoid/Bandits** — faction soldiers, bandits
- **Wildlife** — bears, woodland animals
- **Supernatural** — Wuxia/magical entities
- Tiered: Regular Mobs → **Moderate Talents** (mini-bosses at Outposts) → **Bosses**

---

## 16. Affinity — Deep Dive

- Affinity Rate = % chance per attack to trigger Affinity
- Affinity damage always uses **Max Physical Attack** (deterministic, not a range roll) + ~20%
- **Independent of Precision** — fires on both Precision and Non-Precision hits
- **Momentum** is the premier Affinity attribute: raises both Affinity Rate AND Max Physical ATK simultaneously

### Three Build Layers for Affinity
1. **Attribute:** Stack Momentum
2. **Gear secondaries:** Affinity Rate, Affinity Damage Bonus, Attribute Attack Damage Bonus (via tuning)
3. **Inner Ways:** Insightful Strike, Echoes of Oblivion — boost Max Physical ATK or Affinity Damage

### Affinity Gear Sets
- **Jadeware** — +10% Affinity DMG after Martial Art Skill; +20% Affinity DMG vs. controlled/low-Qi targets
- **Hawking** — +2% Physical ATK per Affinity proc (stacks ×5, max +10%)
- **Veil of the Willow** — for alternating Light/Heavy attack kits
- **Formbend or Moonflare** — add survivability to Affinity builds

---

## 17. Melodies of Peace (Progression System)

Regional perk/node tree called **Harmony**. Exchange Qinghe Oddities (collectibles) to Qi Sheng merchant → activate nodes.

- Structure: Linear-with-branches — each unlocked node reveals the next; branch points offer choices (e.g., flat HP boost vs. passive utility node)
- Rewards: Martial Arts Manuals, stat boosts, passive boosts for Inner Ways, quest items
- Example node: "The Music of Laiyi" — flat bonus to Min Physical Attack (raises damage floor)
- Activation: Talk to oddity merchant → "Submit Oddities" → Melodies of Peace screen → commit
- First node: 1 oddity. All nodes require the prior to be active first
- All unlocked nodes are permanent (no deactivation)
- **Divinecraft Upgrade System** also unlocks at level 56 via Gear > Develop menu (separate from Melodies)

---

## 18. Healer Build (Silkbind–Deluge)

### Weapons
Panacea Fan + Soulshade Umbrella

### Role
Full support: burst and sustained healing, single-target and group recovery, revive (defeated allies can rejoin), team-wide DMG buff.

### Stat Priority
- External ATK — healing scales off ATK (not a separate healing stat)
- Critical Rate — Crit heals restore 50% MORE HP
- Max HP — survivability baseline

### Key Inner Ways
| Inner Way | Effect |
|-----------|--------|
| **Royal Remedy** | Cloudburst Healing +10%; Breakthrough grants free revive every 2 min |
| **Restoring Bloom** | On Crit heal → all allies get Nurturing stack (+2% healing received for 3s, ×3 max) |

### Gear Set
- **Ivorybloom Set** — primary healer gear set for sustain output

### Notes
- Healer career (profession) is separate from Silkbind–Deluge build path — profession affects NPC healing minigame; build affects combat healing output
- Healer profession: Physiotherapy and Qi Therapy prescription types; cures NPC injuries/illnesses

---

## 19. Vietnamese ↔ English / Chinese Terminology

| Vietnamese Term | English | Chinese | Notes |
|----------------|---------|---------|-------|
| chí mạng | Critical / Crit | 会心 (huìxīn) | 黄字 Gold; only on Precision hits |
| hiểu ý | Affinity | 会意 (huìyì) | 橙字 Orange; independent of Precision; checked FIRST |
| độ chính xác / chính xác | Precision | 精准 (jīngzhǔn) | Gates Crit; has its own Rate stat |
| không chính xác | Non-Precision / Abrasion path | 非精准 | Non-Precision hit branch |
| thường | Normal hit | 白字 (báizì) | White number; random Min–Max roll |
| sát thương yếu / đòn xám | Abrasion | 擦伤 (cāshāng) / 灰字 | Dark grey; ½ Min ATK; worst outcome |
| trắng (chỉ số trắng) | Raw / base stat | — | Stat as shown on gear before bonuses |
| vàng (chỉ số vàng) | Effective stat | — | After Path, Mastery, set bonuses applied |
| min/max thuộc tính khác hệ | Off-element min/max roll | — | Gear roll outside main elemental stat |
| Mạc Đao | Mo Blade / Heng Blade | 陌刀 (mòdāo) | Heavy sword; main weapon of Stonesplit-Might |
| công pháp | Martial Arts / Path Skills | 武技 (wǔjì) | Weapon-tied active skills |
| tâm pháp | Inner Ways | 心法 (xīnfǎ) | Passive skill system; up to 4 active |
| kỹ năng huyền bí | Mystic Skills | 奇术 (qíshù) | 40 total; up to 4 active |
| trang bị | Equipment / Gear | 装备 | General gear term |
| khí | Qi / Stagger meter | 气 / 真气 (qì) | Broken by perfect parries → Exhausted state |
| tinh thần | Spirit / Vitality | 精力 | Resource for Mystic Skills |
| nội công | Inner Ways | 心法 (same) | Same as tâm pháp in some contexts |
| chiêu thức | Skills / Techniques | 招式 | General skill term |
| trạng thái kiệt sức | Exhausted state | 气竭 (qìjié) | When Qi broken; stunned; Execute window |
| hệ thống hài hòa | Melodies of Peace | 和乐 | Oddities-based node progression system |

---

## 20. Key External Resources

| Resource | URL | Use Case |
|----------|-----|----------|
| WWM Damage Calculator | https://wherewindsmeetcalculator.com/ | Damage formula, PvP formula, path calculators |
| Stonesplit-Might Calculator | https://wherewindsmeetcalculator.com/build/stonesplit-might | Tank DPS optimizer |
| WWM Stats Calculator | https://wwm-stats-calculator.com/ | Gear & stat priority with Stat Priority Guide |
| DPS Calculator | https://wwm-damage-calculator.vercel.app/ | Interactive DPS optimizer |
| GitHub Community KB | https://github.com/CodeOfVirtue/where_winds_meet-knowledge-base | Raw penetration/defense data (Chinese community) |
| Metabattle Tank Build | https://metabattle.com/where-winds-meet/Thundercry_Blade_%26_Stormbreaker_Spear_Tank_PvE_Build | Full tank build reference |
| Game8 Tank Builds | https://game8.co/games/Where-Winds-Meet/archives/572866 | Tier ranked tank builds |
| Boarhat Inner Ways | https://boarhat.gg/games/where-winds-meet/inner-ways/ | Full Inner Ways list v1.2 |
| Boarhat Mystic Skills | https://boarhat.gg/games/where-winds-meet/mystic-skills/ | Full Mystic Skills list |
| AllThings Boss Talents | https://allthings.how/where-winds-meet-boss-talents-explained-and-why-they-matter/ | Boss Talent system explained |
| Keengamer Attributes | https://www.keengamer.com/articles/guides/where-winds-meet-all-attributes-and-their-effects/ | All attributes and effects |
| AllThings Abrasion | https://allthings.how/how-abrasion-conversion-rate-works-in-where-winds-meet/ | Abrasion Conversion Rate |
| Reddit Community | https://www.reddit.com/r/WhereWindsMeet/ | Community discussion |
| 游民星空 — Damage Formula | https://www.gamersky.com/handbook/202512/2063097.shtml | PvE damage formula (Chinese) |
| 16yanyun.com — Stats | https://16yanyun.com/gameguide/yanyun-basic-attributes-stats-guide | Attribute conversion rates (Chinese) |
| 16yanyun.com — Three Rates | https://16yanyun.com/gameguide/yanyun-three-rates-attributes-guide | Precision/Crit/Affinity mechanics (Chinese) |
| 网易云游戏 — Hit Judgment | https://cg.163.com/static/content/695a08e3337ecb8e73a58c0e | 精准/会意/会心 judgment order (Chinese) |
| GitHub — Penetration KB | https://github.com/CodeOfVirtue/where_winds_meet-knowledge-base | Raw penetration/defense math (community) |
| GamerSky — 九剑九枪 Build | https://www.gamersky.com/handbook/202601/2070602.shtml | Nameless Sword + Spear build guide (Chinese) |
| Steam — Full Guide | https://steamcommunity.com/sharedfiles/filedetails/?id=3605849191 | Community full guide |
| Steam — All Inner Ways 2026 | https://steamcommunity.com/sharedfiles/filedetails/?id=3648961468 | Updated Inner Ways list |
| Steam — All Mystic Skills | https://steamcommunity.com/sharedfiles/filedetails/?id=3624242409 | All Mystic Skills + how to acquire |
| AllThings Affinity Builds | https://allthings.how/where-winds-meet-affinity-builds-how-inner-ways-and-gear-scale-your-damage/ | Affinity build guide |
| Metabattle — All Builds | https://metabattle.com/where-winds-meet/ | Full build index (PvE + PvP) |
| Game8 — Best Builds Tier List | https://game8.co/games/Where-Winds-Meet/archives/564672 | PvE + PvP tier rankings (updated Apr 2026) |
| Game8 — Snowparting+Phalanxbane | https://game8.co/games/Where-Winds-Meet/archives/589037 | Stonesplit–Strength build guide |
| Game8 — Inkwell Fan Build | https://game8.co/games/Where-Winds-Meet/archives/564879 | Silkbind–Jade build guide |

---

## 21. All Paths — Build Reference (Corrected & Expanded)

### Confirmed Path List (8 Official Paths)

| Path | Weapons | Role | PvE Tier | PvP Tier |
|------|---------|------|----------|----------|
| **Bellstrike–Umbra** | Strategic Sword + Heavenquaker Spear | Melee DPS (Bleed) | S | S |
| **Bellstrike–Splendor** | Nameless Sword + Nameless Spear | Melee DPS (Mobile) | A | A |
| **Silkbind–Jade** | Inkwell Fan + Vernal Umbrella | Ranged DPS | S/A | A |
| **Silkbind–Deluge** | Panacea Fan + Soulshade Umbrella | Healer/Support | S | B |
| **Bamboocut–Wind** | Infernal Twinblades + Mortal Rope Dart | Assassin DPS | B | S |
| **Bamboocut–Dust** | Everspring Umbrella + Unfettered Rope Dart | Ranged AoE | B | B |
| **Stonesplit–Might** | Thundercry Blade + Stormbreaker Spear | Tank | A | A |
| **Stonesplit–Strength** | Snowparting Blade + Phalanxbane Blade | Close-Range DPS | S/A | A |

> Tier source: Game8 (April 2026). S = highly recommended, B = has clear issues vs. other builds. All builds are viable in skilled hands. Bamboocut–Dust is listed B overall but is considered T0 for AoE mob-clearing content (v1.4+ path).

---

### BELLSTRIKE–UMBRA (Strategic Sword + Heavenquaker Spear)

**Role:** Melee DPS — Bleed/DoT. Current PvE meta leader.
**Difficulty:** Easy. **PvE: S-Tier. PvP: S-Tier.**

**Core Identity:** Stack 5 Bleed stacks via Inner Track Slash → trigger massive burst via Sober Sorrow (River Flow: +20% HP damage buff) → Spear charged skill (5 hits, +50% DoT and Bleed burst). High damage, simple execution.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Strategic Sword (primary bleed stacker) + Heavenquaker Spear (burst finisher) |
| **Gear Sets** | Hawkwing 4-pc (weapons/accessories) + Eaglerise 4-pc (armor) |
| **Stat Priority** | Power → Affinity Rate → Bellstrike Attack → Max/Min Physical ATK → Crit Rate → Physical Penetration → Weapon/Skill Damage |
| **Inner Ways** | Morale Chant, Sword Horizon, Wolfchaser's Art, Breaking Point |
| **Mystic Skills** | Drunken Poet, Lion's Roar, Dragon's Breath, Dragon Head, Leaping Toad, Cloud Steps, Soaring Spin, Ghost Bind |

**Bleed Stack Model:** Stack-based (NOT duration timer) — you need all 5 stacks before detonation; stacks decay between rotations without Wolfchaser's Art maintaining them.

**Phase 1 — Bleed Application (Strategic Sword):**
Inner Track Slash (2nd stage) → applies 5 Bleed stacks (fastest method)

**Phase 2 — Heavenquaker Buff Setup:**
Sober Sorrow → build combo to ~10 hits → animation-cancel with Parry → secure Riverflow + Flowing River buffs → Heavenquaker Charged Heavy (1st charge stage) → apply Soul Shaken stacks (up to 5, each +10% Bleed damage)

**Phase 3 — Detonation (Strategic Sword):**
Inner Balance Strike III (Special Skill) → Crosswind Blade → repeat 3–4× during buff window
(Sword Horizon procs detonate full Bleed stacks; at 4+ stacks the Special triggers double explosion)

**Key Inner Way Details:**
- **Sword Horizon** — after casting Martial Art Skill/Special/Charged Skill, press skill at perfect timing during ending phase → cast Crisscrossing Swords; if 5 Bleed stacks on target → removes all and deals massive Bleed damage. T3: guaranteed Affinity hit when Crosswind Blade Spirit is full. T6: retains 2 Bleed stacks after detonation.
- **Wolfchaser's Art** — with 5 Bleed stacks on boss, each Sober Sorrow hit has up to 100% chance to increment combo count by 1; pushes combo meter to max almost instantly, enabling highest Heavenquaker buff tier from one engage. Without this, stacks decay between rotations — it is the rotation engine.

---

### BELLSTRIKE–SPLENDOR (Nameless Sword + Nameless Spear)

**Role:** Melee DPS — Mobile. High mobility, good CC, great defense.
**Difficulty:** Normal. **PvE: A-Tier. PvP: A-Tier.**
**Chinese name:** 九剑九枪 (Nine-Sword Nine-Spear)

**Core Identity:** Weave melee and ranged attacks; QQQ shield rotation; precise burst after deflects. High skill ceiling but very rewarding. Key loop: Spear builds damage buffs → swap to Sword → build Bleed → trigger Bleed explosion for burst.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Nameless Sword (DPS finisher) + Nameless Spear (buff setup / CC) |
| **Gear Sets** | Hawkwing 4-pc (weapons/accessories) + Eaglerise 4-pc (armor) |
| **Stat Priority** | Power → Affinity Rate → Bellstrike Attack → Max/Min Physical ATK → Crit Rate → Physical Penetration → Weapon/Skill Damage |
| **Inner Ways** | Morale Chant, Sword Morph, Mountain's Might, Battle Anthem |
| **Mystic Skills** | Talon Strike, Soaring Spin, Flaming Meteor, Free Morph, Ghost Bind, Dragon's Breath, Wolflike Frenzy, Drunken Poet |

**Sword Rotation:**
Daunting Strike → Shadow Step → Vagrant Sword → Soaring Spin → Wolflike Frenzy

**Spear Rotation:**
Qiankun's Lock → Legion Crusher (×2) → Free Morph → Soaring Spin

**Required Inner Way:** Sword Morph (剑气纵横) — activates 纵横剑 (Sword Horizon) on move endings to detonate Bleed; essential to this build.

**Chinese community tip:** Use Spear's 唤千军 (Legion Summoner) skill to apply buff at battle start, then swap to Sword; hold light attack + spam heavy attack derived input for consistent output.

### Bellstrike-Splendor — Deep Mechanics

**Sword Morph Inner Way — QQQ Shield Rotation:**
- Sword Morph's T2/T3 unlocks a shield-generation mechanic on QQQ rapid input (3 quick skill presses)
- Shield absorbs incoming hits while you continue dealing damage — enables aggressive offense without dodging
- Practical loop: QQQ (shield up) → continue Sword attacks → re-trigger before shield expires

**Vagrant Sword — Endurance Scaling:**
- Vagrant Sword skill damage scales with your current **Endurance** stat
- Each point of Endurance adds **+1% damage** to Vagrant Sword's hit
- This makes Endurance a meaningful secondary consideration for this build (unlike most other paths)
- Stack Endurance via gear secondaries to push Vagrant Sword's ceiling; does not require sacrificing core Power/Agility

**Nameless Art — Energy Generation at 4-Tier:**
- At Tier 4 (fully upgraded Nameless Art), casting Nameless Art generates **3 Sword Energy charges** per cast
- Sword Energy powers Sword Morph's enhanced attacks (the Sword Horizon procs)
- This effectively triples the frequency of Sword Horizon detonations per rotation cycle at max investment

**Nameless Spear — PvE Role:**
- In PvE the Nameless Spear functions as a **buff-setup/utility tool**, NOT a primary damage dealer
- Spear's role: apply buffs (Legion Summoner), apply CC (Qiankun's Lock), then immediately swap to Sword
- Do NOT stay on Spear to deal damage in PvE — Sword is where all significant DPS comes from
- In PvP the Spear is more actively used for spacing and interrupt

---

### SILKBIND–JADE (Inkwell Fan + Vernal Umbrella)

**Role:** Ranged DPS. Safe, sustained projectile pressure with strong buffs/debuffs.
**Difficulty:** Normal. **PvE: S/A-Tier. PvP: A-Tier.**

**Core Identity:** Set up buffs with Fan → switch to Umbrella for sustained charged projectile damage. Blossom meter = your main damage gauge. Ranged kite playstyle — never stand in melee range.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Inkwell Fan (setup/buff) + Vernal Umbrella (sustained DPS) |
| **Gear Sets** | Hawkwing 4-pc (weapons/accessories) + Eaglerise 4-pc (armor) |
| **Stat Priority** | Power → Affinity Rate → Silkbind Attack → Charge Skill Damage → Max/Min Physical ATK → Crit Rate → Physical Penetration → Projectile Damage |
| **Inner Ways** | Morale Chant, Blossom Barrage, Star Reacher, Battle Hymn |
| **Mystic Skills** | Soaring Spin, Leaping Toad, Cloud Steps, Ghost Bind, Guardian Palm, Wolflike Frenzy |

**Key Mechanics:**
| Mechanic | Description |
|----------|-------------|
| Blossom meter | Built by holding Charged Light Attack on umbrella; triggers Unfading Flower when full |
| Spring Shock stacks | Gained from Fan Heavy (Pursuit Attack); buffs Charge Skill Damage |
| Lingering Bone | Debuff from Peak's Springless Silence; activates Inner Way bonuses |
| Projectile Damage Taken | Enemy debuff from Spring Sorrow; amplifies all projectile hits |
| Emerald Barrier buff | 15s Projectile Damage buff from Fan skill; core rotation piece |

**Standard Rotation:**
1. Emerald Barrier (Fan) → 15s Projectile Damage buff
2. Swap to Umbrella → Spring Sorrow → debuff target
3. Hold Charged Light Attack → build Blossom meter
4. Unfading Flower (at full Blossom) → passive projectile pressure
5. Swap to Fan → Peak's Springless Silence → Lingering Bone + launch
6. Fan Heavy (Moon Shatter Spring) → Spring Shock stacks
7. Soaring Spin Mystic Skill → burst window
8. Leaping Toad → reposition + animation-cancel into Emerald Barrier → loop

**PvP note:** Lives and dies by spacing. Avoid direct brawling; force overcommits; stamina management is the primary weakness.

---

### SILKBIND–DELUGE (Panacea Fan + Soulshade Umbrella)

**Role:** Healer/Support. Full sustain, team healing, revive, damage buff.
**Difficulty:** Easy. **PvE: S-Tier (support). PvP: B-Tier.**

**Core Identity:** Use Umbrella near allies for passive heal aura + Dew generation → Emerald Dewtouch for burst heal → Floating Grace loop → swap to Panacea Fan for additional healing. Damage scales off ATK stat, not a separate healing stat; Crit heals restore +50% HP.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Panacea Fan (healing + Dew resource) + Soulshade Umbrella (aura heal + damage amp) |
| **Gear Sets** | Ivorybloom 4-pc + Eaglerise 4-pc |
| **Stat Priority** | External ATK → Crit Rate → Max HP → Silkbind Damage |
| **Inner Ways** | Royal Remedy, Restoring Bloom, Mending Loom, Morale Chant (replaces Esoteric Revival) |
| **Key Mechanic** | Dew resource (replaces stamina for Fan skills); generated by umbrella aura |

**Ivorybloom 4-pc bonus:** +Crit Rate; at max HP: +5% Crit chance + 15% bonus Crit healing/damage
**Royal Remedy:** Cloudburst Healing +10%; Breakthrough grants free revive every 2 min
**Restoring Bloom:** On Crit heal → all allies gain Nurturing (+2% healing received for 3s, stacks ×3)

---

### BAMBOOCUT–WIND (Infernal Twinblades + Mortal Rope Dart)

**Role:** Assassin DPS. Highest attack speed in the game; top PvP damage.
**Difficulty:** Normal (low floor, very high ceiling). **PvE: B-Tier. PvP: S-Tier.**

**Core Identity:** Build Flamelash meter via Addled Mind and light attacks → activate Flamelash rage state → 5-combo light attack spam for massive burst. Rope Dart's Bladebound Thread + Rodent Rampage combo. Poison via Bitter Seasons Inner Way. Squishy — no sustain without clean execution.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Infernal Twinblades (primary DPS) + Mortal Rope Dart (setup / Flamelash builder) |
| **Gear Sets** | Swallowcall 4-pc (weapons/accessories) + Calmwaters 4-pc (armor) |
| **Stat Priority** | Power → Affinity Rate → Bamboocut Attack → Max/Min Physical ATK → Crit Rate → Physical Penetration → Weapon/Skill Damage |
| **Inner Ways** | Echoes of Oblivion, Morale Chant, Vendetta, Breaking Point |
| **Mystic Skills** | Ghost Bind, Wolflike Frenzy, Free Morph, Dragon's Breath, Drunken Poet, Talon Strike, Soaring Spin, Flaming Meteor |

**Rope Dart Sequence:**
Bladebound Thread → Rodent Rampage → Light Attacks (build Flamelash) → weapon swap → repeat with buffed attacks

**Infernal Twinblades Sequence:**
Dragon's Breath → Addled Mind → Calamity's Greed → Addled Mind → Calamity's Greed → Dragon's Breath → Wolflike Frenzy

### Flamelash / Karmic Flame — Deep Mechanics

**Building Karmic Flame (meter):**
- Light attacks + Addled Mind skill continuously fill the Karmic Flame gauge
- Rope Dart skills (Bladebound Thread especially) are the fastest builders

**Wrathful Form (Flamelash active):**
When Karmic Flame meter is full, activate Flamelash to enter Wrathful Form:
- **Fortitude** — status immunity (cannot be stunned, knocked down, or CC'd)
- Light attacks upgrade to **Celestial Fury** — enhanced 5-hit combo with dramatically increased speed and damage
- Attack speed increases further beyond base Twinblades speed
- +**10% Critical Rate**
- +**20% Critical Damage Bonus**
- Attacks **drain HP** — sustain yourself during Wrathful Form via lifesteal from Addled Mind / Calamity's Greed; HP drain stops when Flamelash ends

**Agility Double-Dip in Flamelash:**
- Min Physical ATK scales with Agility (the Twinblades' primary scaling attribute)
- During Flamelash, **Critical Damage Bonus also scales with Min Physical ATK** — double-dipping on Agility investment
- This makes Agility the #1 priority stat for this path, unlike every other path that favors Power/Momentum

### Sin + Karma Debuff System

**Echoes of Oblivion Inner Way interaction:**
- Stacks Sin debuff on enemy via Rope Dart and Twinblades hits
- At threshold: converts Sin stacks to Karma stacks
- Karma debuff: enemy ignores **−10% Physical Defense** + **−10% Bamboocut Resistance**
- Synergizes with Twinblades' high attack speed — Sin stacks fill faster than any other path

### Rodent Rampage Economy (Token of Gratitude / Vendetta Token)
- Rodent Rampage generates **Token of Gratitude** on each rat-swarm proc hit
- Tokens convert to **Vendetta Tokens** at threshold
- Vendetta Tokens empower Bladebound Thread (extended duration, extra pulls)
- Full economy loop: hit → rat proc → Token of Gratitude → Vendetta Token → enhanced Bladebound Thread → more hits → more rat procs
- The Vendetta Inner Way extends Bladebound Thread duration, keeping the economy cycling

**Key Mechanics Summary:**
- Flamelash meter: built via Addled Mind / light attacks; at full → Wrathful Form with CC immunity + Celestial Fury
- Rodent Rampage: attacks proc rat swarms that hit simultaneously — major DPS multiplier; feeds token economy
- Echoes of Oblivion: Sin→Karma armor penetration debuff; synergizes with Twinblades' high attack speed
- PvP combo: Rodent Rampage + Addled Mind + Flamelash active = sudden kill potential ("burst assassin")

**PvE weakness:** HP drain during Flamelash + no innate sustain = requires clean burst-and-kite play; gets punished hard if overextended or Flamelash ends mid-fight.

---

### BAMBOOCUT–DUST (Everspring Umbrella + Unfettered Rope Dart)

**Role:** Ranged AoE DPS. Best AoE mob-clearing path in the game (T0 for AoE content). Moderate single-target.
**Difficulty:** Normal. **PvE: B overall / T0 AoE. PvP: B-Tier.**
**Added in:** v1.4 content update.

**Core Identity:** Throw Everspring Umbrella outward → Perfect Catch on return → repeat for sustained AoE pressure. Candlelight mechanic multi-hits → enemy Immobilize. Rope Dart punishes Immobilized/Exhausted targets. Positioning and timing-based — less button-mash, more rhythm.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Everspring Umbrella (primary AoE + Fading Crimson resource) + Unfettered Rope Dart (burst finisher) |
| **Gear Sets** | Stars Align 4-pc (BiS for Umbrella; enhances Perfect Catch bonus window) + Beyond the Chill 4-pc (−40% dmg on first hit after 10s safe) |
| **Stat Priority** | Power → Bamboocut Attack → Max Physical ATK → Affinity Rate → Crit Rate → Physical Penetration |
| **Inner Ways** | Phantom Rally, Towline Sweep, Song of Tang, Light Anew |
| **Mystic Skills** | Ghost Bind, Cloud Steps, Soaring Spin, Leaping Toad |

### Everspring Umbrella — Core Mechanics

**Fading Crimson (Resource):**
- Charges regenerate over time automatically
- **Stops regenerating while in Flower Burial stance** — manage stance uptime vs. resource refill carefully
- All umbrella throws consume Fading Crimson charges

**Scarlet Spin (Martial Art Skill):**
- Hurl the umbrella outward along a wide arc; it returns to you
- **Activates Flower Burial stance** for 12 seconds (stance boosts damage and unlocks follow-up options)
- Each throw hits enemies **twice** — once on the way out, once along the return path
- Hits multiple enemies simultaneously along the full travel arc

**Perfect Catch:**
- Press the skill button at the precise moment the umbrella returns to you
- **Effect:** Enhances the next throw significantly (more damage, wider arc) + triggers bonus effects that scale with Stars Align gear
- Timing feedback: brief visual flash as the umbrella completes its arc near your character
- Mastering the rhythm of throw → Perfect Catch → throw is the core skill expression of this path

**Candlelight Mechanic:**
- Hitting **2 or more enemies simultaneously** with an umbrella throw → apply Candlelight stack
- Stacks up to 5 — each stack shows on enemy
- At 5 stacks: target is **Immobilized for 2 seconds** (hard CC — cannot act)
- Essential for mob-clearing: chain throws, max Candlelight quickly, CC entire groups simultaneously

### Unfettered Rope Dart — Role
- Burst punisher after Immobilize: pull → follow-up while enemy locked
- Interrupt mobile enemies attempting to flee after umbrella pressure
- AoE crowd control via swing and pull patterns

### Standard Rotation
```
Scarlet Spin (umbrella out) → hit 2+ enemies → Candlelight stacks
→ Perfect Catch (on return) → enhanced throw → more stacks
→ Repeat until 5 Candlelight → Immobilize triggers
→ Swap to Rope Dart → burst Immobilized group
→ Re-enter Flower Burial → reset loop
```

**Solo Boss:** Drop to single-target mode — skip Candlelight stacking, focus Perfect Catch timing for sustained single-hit pressure + Rope Dart punishes on stagger.

**Inner Way Details:**
- **Phantom Rally** — enhances umbrella return damage; increased proc on Perfect Catch
- **Towline Sweep** — Rope Dart AoE pull range + follow-up hit
- **Song of Tang** — buff on Flower Burial entry; duration matches stance window
- **Light Anew** — resets a charge after Perfect Catch under certain conditions

---

### STONESPLIT–STRENGTH (Snowparting Blade + Phalanxbane Blade)

**Role:** Close-Range Melee DPS with defensive options. Fast weapon swaps, parry-centric, strong burst windows.
**Difficulty:** Normal. **PvE: S/A-Tier. PvP: A-Tier.**

**Core Identity:** Snowparting Blade generates and maintains Blade Momentum through deflects; Phalanxbane Blade converts setup into heavy burst damage via Anxi Army summons and charge attacks.

| Component | Selection |
|-----------|-----------|
| **Weapons** | Snowparting Blade (momentum/deflect) + Phalanxbane Blade (burst finisher) |
| **Gear Sets** | Shattered Ridge (weapons) + Obsidian Armor (or Agile Steps for PvP) |
| **Stat Priority** | Agility → Min Stonesplit Attack → Crit Rate → Physical Attack → Power |
| **Inner Ways** | Morale Chant, Frost-Clad Night, Steadfast Devotion, Throat-Piercing Art |
| **PvE Mystic Skills** | Flute of the Tides, Soaring Spin, Leaping Toad |
| **PvP Mystic Skills** | Serene Breeze, Wolflike Frenzy, Drunken Poet |

**Shattered Ridge (4-pc):** Successfully deflecting or hitting a boss → Shattered Ridge (5s): +5% HP damage + +5% Light/Heavy Attack Varied Combo and Assist damage.
**Obsidian Armor:** Reduces Crit/Affinity damage taken; synergizes with Tenacity.
**Agile Steps (PvP):** +20% damage reduction after deflections.

**Standard Rotation:**
1. Legion Summoner (Phalanxbane, opens with Anxi Army)
2. Dual-Weapon Skill (Tab) + follow-up (F) to Snowparting
3. General's Bane (Q) — engage/gap close
4. Swans Treading on Snow
5. Grave Frost (Hold LMB) + follow-up
6. Swap and cycle

**Inner Way Details:**
- **Frost-Clad Night:** Converts successful deflects and charged skills into bonus damage via Finding Spring ability
- **Steadfast Devotion:** Upgrades Burning Heart to 3-stage charging with Mountain Splitter debuff (+15% damage taken)
- **Throat-Piercing Art:** After Snowparting counterattack deflection → ignore 2 Physical Resistance + Crit DMG +2% for 8s, stacks ×3

**PvP tips:** Snowparting Blade becomes primary weapon in PvP; Phalanxbane maintains buffs. Be patient — punish with Snowparting after opponent mistakes. Animation-cancel Dragon's Breath for instant value.

---

## 22. Build Tier List Summary (April 2026)

### PvE Rankings
| Rank | Build | Weapons | Why |
|------|-------|---------|-----|
| **S** | Bellstrike–Umbra | Strategic Sword + Heavenquaker Spear | Simple, high burst, bleed mechanics work everywhere |
| **S** | Silkbind–Deluge | Panacea Fan + Soulshade Umbrella | Only true healer; irreplaceable in co-op |
| **S** | Snowparting + Phalanxbane | Stonesplit–Strength | Fast melee, smooth swaps, strong burst |
| **A** | Stonesplit–Might | Thundercry + Stormbreaker | Best tank; co-op anchor; lower solo output |
| **A** | Bellstrike–Splendor | Nameless Sword + Spear | Mobile DPS; high skill ceiling |
| **A** | Silkbind–Jade | Inkwell Fan + Vernal Umbrella | Safe ranged DPS; easy to learn |
| **B** | Bamboocut–Wind | Infernal Twinblades + Rope Dart | High skill ceiling; PvE sustain issues |
| **B/S** | Bamboocut–Dust | Everspring Umbrella + Rope Dart | B overall; T0 for AoE mob clearing via Candlelight mass-immobilize (v1.4+) |

### PvP Rankings
| Rank | Build | Why |
|------|-------|-----|
| **S** | Bamboocut–Wind | Burst assassin; Rodent Rampage = sudden kills |
| **S** | Bellstrike–Umbra | Bleed control + DoT in spacing game |
| **A** | Bellstrike–Splendor | CC, spacing, strong deflect punishes |
| **A** | Silkbind–Jade | Ranged control; kiting; team fights |
| **A** | Stonesplit–Might | Durable; taunt; 1v1 trading |
| **A** | Stonesplit–Strength | Close-range burst if managed well |
| **B** | Silkbind–Deluge | Sustain in team play; low 1v1 kill power |
| **B** | Bamboocut–Dust | Moderate; better in PvE |

---

## 23. Universal Inner Ways Tier List

| Inner Way | Tier | Builds | Effect Summary |
|-----------|------|--------|---------------|
| **Morale Chant** | S | ALL builds | Stacking damage (+1 stack per attack/heal; up to 5); breakthroughs add Penetration, duration, CC bonus — mandatory everywhere |
| **Echoes of Oblivion** | S | Bamboocut–Wind | Armor penetration debuff; synergizes with high attack speed |
| **Blossom Barrage** | A | Silkbind–Jade | Extra Spring Sorrow charge; increases damage targets receive |
| **Star Reacher** | A | Silkbind–Jade | +10% Physical ATK vs. launched/exhausted targets |
| **Sword Morph** | A | Bellstrike–Splendor | Activates Sword Horizon on move endings → detonates Bleed |
| **Wolfchaser's Art** | A | Bellstrike–Umbra | Triggers damage escalation at 5 Bleed stacks |
| **Frost-Clad Night** | A | Stonesplit–Strength | Converts deflects + charged skills → bonus damage |
| **Steadfast Devotion** | A | Stonesplit–Strength | 3-stage charge upgrade; Mountain Splitter debuff |
| **Throat-Piercing Art** | A | Stonesplit–Strength | Post-deflect Crit DMG stack ×3 |
| **Art of Resistance** | A | Stonesplit–Might | Shield duration +4s (Predator's Shield: 8s → 12s, near 100% uptime) |
| **Rock Solid** | A | Stonesplit–Might | Shield boost |
| **Royal Remedy** | A | Silkbind–Deluge | Cloudburst Healing +10%; Breakthrough = free revive every 2 min |
| **Restoring Bloom** | A | Silkbind–Deluge | Crit heal → all allies +2% healing received for 3s, ×3 max |
| **Vendetta** | B-A | Bamboocut–Wind | Extends Bladebound Thread duration |
| **Breaking Point** | B | Multiple | Additional penetration |
| **Invigorated Warrior** | B | Universal | Good early game filler |
| **Battle Anthem / Battle Hymn** | B | Multiple | +10% Charge damage |
| **Mountain's Might** | B | Bellstrike–Splendor | — |
| **Bitter Seasons** | B | Bamboocut–Wind | Applies poison; pairs with Twinblades' high attack speed |

---

## 24. Complete Gear Sets Reference (Updated)

| Set | 2-pc | 4-pc | Best Path |
|-----|------|------|-----------|
| **Hawkwing** | +Min Physical Attack | Affinity hit → Hawking: +2% Phys ATK for 5s, ×5 stacks (10% max) | Bellstrike-Umbra, Bellstrike-Splendor, Silkbind-Jade |
| **Eaglerise** | Defense stat | DoT/healing → Eaglerise stack: −1.2% dmg taken, ×5; max stacks = Eagle Guard (next hit −90% dmg, −45% vs bosses, 30s CD) | Universal defensive pairing |
| **Swallowcall** | +Min Physical ATK | Light Attacks deal +15% dmg to enemies <40% Qi (+20% to Exhausted) | Bamboocut-Wind, aggressive melee |
| **Calmwaters** | Defense | Perfect Dodges: 50% chance restore HP + Endurance | Mobile/evasive builds |
| **Shattered Ridge** | +55 Min Physical ATK | Deflect or hit boss → Shattered Ridge (5s): +5% HP DMG + +5% Varied Combo/Assist dmg | Stonesplit–Strength (Snowparting) |
| **Obsidian** | Defense | Reduces Crit/Affinity dmg taken; synergizes with Tenacity | Stonesplit–Strength (PvE) |
| **Agile Steps** | Defense | +20% dmg reduction after deflections | Stonesplit–Strength (PvP) |
| **Rainwhisper** | +40 Max HP | +10% Crit DMG + healing received; +25% when HP shield active | Stonesplit–Might (accessories) |
| **Moonflare** | Defense | Attacking while defending: 30% chance → shield (10% Max HP, 20s); heals 2% HP if shield exists; 60s CD | Stonesplit–Might (armor) |
| **Flawless Defense** | +Physical Defense | −5% dmg taken; −10% more when HP <60% | Stonesplit–Might (alt armor) |
| **Formbend** | Shield duration +2s | If Qi >85% or Qi shield active: −20% all HP dmg taken | Bellstrike–Splendor, defensive builds |
| **Ivorybloom** | +Crit Rate | At max HP: +5% Crit chance + 15% bonus Crit healing/damage | Silkbind–Deluge (healer) |
| **Beyond the Chill** | +780 Max HP | After 10s without dmg: next hit + all dmg within 2s after = −40% | Bamboocut–Dust, defensive ranged |
| **Jadeware** | +Max Physical ATK | Martial Art Skill cast → +10% Affinity DMG; +20% vs controlled/<40% Qi targets | Bellstrike–Splendor accessories |
| **Hawkwing** | (see above) | (see above) | DPS universal |
