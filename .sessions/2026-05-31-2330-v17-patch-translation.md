---
date: 2026-05-31
slug: v17-patch-translation
files_touched:
  - capnhat-v17.html
  - KNOWLEDGE.md
  - .sessions/2026-05-31-2330-v17-patch-translation.md
  - (also referenced — committed in prior session: AGENTS.md, index.html, styles.css, CLAUDE.md, graphify-out/)
bugs_recorded: []
---

## User request

Translate the new `capnhat-v17.html` patch-note page (Where Winds Meet v1.7 balance update, 28/05/2026) from English to Vietnamese. The page was authored earlier in mixed VN/EN form; this session brings the entire content into Vietnamese (with English in parenthesis on first encounter per established convention) and logs the canonical EN↔CN↔VN glossary to `KNOWLEDGE.md`.

Multi-pass scope that grew over the session:

1. **Martial arts + inner ways + sets** — cross-reference against the official Traditional Chinese patch note (`wherewindsmeetgame.com/hmt/news/update/Adjustment528.html`); use Hán Việt readings for proper-noun feel.
2. **Remaining English effect/skill/buff names** — second pass against the CN patch note; same Hán-Việt-with-parenthetical rule per `<h3>` section.
3. **Stat-line bonus terms** — translate to **normal Vietnamese (sentence-case)** not Hán Việt: `Tỷ lệ hiểu ý`, `Tăng tấn công vật lý`, `Đòn nhẹ thường`, etc.
4. **Talent-name retroactive lowercase** — sentence-case all `Tăng cường …` and `Tăng ST …` talents, plus a one-off rename `Sạc → tụ lực`.
5. **Arena → Võ đài** — replace English "Arena" with Vietnamese "Võ đài"; reorder compound `Arena Định âm → Định âm võ đài` (lowercase `võ đài` suffix); apply same to existing `Đấu trường` occurrences.
6. **Hành giả → người chơi** — replace wuxia-flavoured `Hành giả` (lit. "wanderer") with the more familiar `người chơi` across the whole file.
7. **§II B targeted block translation** — translate the cluster of leftover English combat-mechanics terms in the "Phòng thủ, né & trúng đòn" sub-section to Vietnamese; **keep `Guild War` and `Ultimate` in English** per explicit user instruction.
8. **Log all confirmed translations to `KNOWLEDGE.md` §25** as canonical glossary for future agents.

Also handled mid-session: user noticed I had not addressed them as "Pito" consistently or read CLAUDE.md at session start (verification check was free-floating because the SessionStart hook injects the Pito reminder). Ran the full project session-start protocol on demand.

## Plan / decisions

### Translation strategy (recurring pattern this session)

- **Hán Việt** for proper-noun-like entities (martial arts, inner ways, sets, named skills, named buffs/states, mystic skills) — preserves the wuxia register, matches `Vô Danh Thương/Kiếm Pháp` already in the codebase.
- **Normal Vietnamese (sentence-case)** for stat-line bonus names — `Tỷ lệ hiểu ý`, `Tăng tấn công vật lý tối thiểu`, `Đòn nhẹ thường`. Per user: "they are not names".
- **Sentence-case for talent names** — first word capitalised, the rest lowercase: `Tăng cường hiểu ý kiếm khí`, `Tăng cường kết toán khi tụ lực`. Embedded proper-noun martial-art names stay capitalised (`Tăng ST Hồi Toàn Tán`).
- **`Tỷ`** not `Tỉ` (user-confirmed).
- **First occurrence per `<h3>` section**: render as `<em>VN Name</em> (EN Name)`. Subsequent occurrences in the same section: `<em>VN Name</em>` only.
- **Section-context aware** for ambiguous EN terms — most notable case: `Concentration` maps to **Khán Phá** in MK-Ảnh (Insightful Strike buff = 看破) but **Ngưng Tâm** in KT-Lâm (Restoring Blossom buff = 凝心). The Python transform detects school via the h3's Vietnamese school marker and dispatches accordingly.
- **No CN inline** — keep markup clean; CN lookup lives in `KNOWLEDGE.md` §25.
- **Keep EN** for: sub-school names in body text (already shown in h3 headings), unverified entries (Soulbound, Soul Return, Shocked, Phantom Umbrella, Soul Sweep, Midnight Judgment, Shattered Ridge, Blade Momentum, Fading Crimson, Bleeding), and explicit user keep-EN markers (`Guild War`, `Ultimate`, `Hit`, `Deflect`, `combo`).

### CN-source canonicalisation

The translation work hit an early failure mode (e017 / e024 echo): I started guessing CN names without authoritative source. User said "this is not the correct approach, look up the CN patch note" — I then `curl`-ed the Traditional Chinese patch note (`wherewindsmeetgame.com/hmt/news/update/Adjustment528.html`), extracted all 227 `「名」` quoted entities via regex, and cross-referenced from there. Every Hán Việt translation in §25.2 / §25.3 / §25.4 / §25.6 / §25.7 / §25.8 traces to a quoted CN name in the patch note.

### Concentration disambiguation

The English patch note conflates two different CN buffs as "Concentration":
- MK-Ảnh, Insightful Strike (心法 凝神章): buff `看破` ("kan-po" / insight) — applied during the inner way's active phase.
- KT-Lâm, Restoring Blossom (心法 杏花不見): buff `凝心` ("ningxin" / solidified heart) — healing-flow buff.

Resolved with a section-context map in the transform script (`CONTEXT_MAP`). Python detects which school's h3 owns the current segment and dispatches to the correct VN reading. Worked first-try — 3 occurrences correctly resolved, 0 left as EN.

### Cyclone Waltz / Scarlet Spin unification

CN patch note shows the in-patch rename `紅綃香斷 → 紅消香斷` (same Hán Việt: Hồng Tiêu Hương Đoạn). The EN translator left both forms in the EN file. Per user decision: use one VN throughout (`Hồng Tiêu Hương Đoạn`); each EN form still gets its own first-occurrence parenthetical via the script's per-EN `seen` set.

### Arena → Võ đài compound reorder

User-preferred term is `Võ đài` (martial stage, common-noun) over `Đấu trường` (battlefield). For the compound `Arena Định âm`, user specified reorder to `Định âm võ đài` (lowercase `võ đài` as suffix). For standalone `Đấu trường` → `Võ đài` capitalised, preserving prior convention. Three substitution patterns, applied in compound-first order to avoid double-rewrites.

### §II B targeted translation

Block had a cluster of leftover English terms inconsistent with the rest of the file. User-approved per-term casing — most lowercase (`bất hoại`, `đạn`) but some capitalised (`Phòng thủ phản công`, `Animation sau khi dùng chiêu`). User reverted three of my initial proposals back to EN-keep: `combo`, `Hit`, `Deflect`. Also dropped the parenthetical for `bản đồ (map)` → straight `bản đồ`.

### Pito greeting verification (Pre-translation hook setup, recap)

Earlier in the session (before /compact) user added a SessionStart hook and Stop hook to `~/.claude/settings.json` injecting reminders to greet/farewell as "Pito", with a corresponding `## User Identity` section in `~/.claude/CLAUDE.md`. **Verification design flaw caught mid-session**: the SessionStart hook's `additionalContext` injects the Pito reminder directly, so the greeting can fire without ever reading CLAUDE.md — the test doesn't actually test what it claims to test. Documented this for the user; they accepted the test is now belt-and-suspenders rather than a strict verification, with the rule documented in CLAUDE.md as the real source of truth.

## Files changed

### `capnhat-v17.html`

Total transformations over multiple Python-script passes:
- **Pass 1**: 53 first-occurrence + 70 subsequent EN→VN replacements for martial arts / inner ways / sets (§25.1–25.4 glossary).
- **Pass 2**: 127 first-occurrence + 81 subsequent EN→VN for buffs/states/skills/mystic-skills/stat-lines (§25.6–25.10). 9 talent-name string fixes (sentence-case + the `Sạc → tụ lực` rename). 3 Concentration occurrences resolved by section context.
- **Pass 3** (Arena → Võ đài): 11 `Arena Định âm` → `Định âm võ đài`, 3 `Định âm Đấu trường` → `Định âm võ đài`, 22 standalone `Đấu trường` → `Võ đài`. Total 36 substitutions.
- **Pass 4** (Hành giả + §II B): 10 `Hành giả` → `người chơi` (9 lowercase, 1 capitalised after period), plus 8 §II B sentence-level rewrites translating combat-mechanics terms.
- **Pass 5** (user-revised §II B): 7 substitutions reverting `combo`/`Hit`/`Deflect` to EN, dropping `(map)` parenthetical, swapping `Phản công sau phòng thủ → Phòng thủ phản công`, `Bất khả xâm phạm → bất hoại`, `tung chiêu → dùng chiêu`, `Đầu đạn → đạn`.

Final residual English `<em>` count: 17 — all intentional (8 sub-school refs already shown in h3, 6 unverified items kept-EN, 3 stat resources `Blade Momentum`/`Fading Crimson`/`Shattered Ridge`).

### `KNOWLEDGE.md`

Added §25 "Vietnamese Name Glossary — EN ↔ CN ↔ VN" with 11 subsections:
- §25.1 Schools / Sub-paths (8 rows)
- §25.2 Martial Arts (15 rows)
- §25.3 Inner Ways (22 rows, includes Fivefold Bleed late-add)
- §25.4 Sets (5 rows)
- §25.5 Translation conventions (Hán-Việt vs normal-VN rule, sentence-case for stat lines, first-occurrence parenthetical rule, section-context awareness, keep-EN list)
- §25.6 Buffs / States / Conditions (31 rows)
- §25.7 Named Skills (45 rows)
- §25.8 Mystic Skills & Perception Skills (11 rows)
- §25.9 Stat-line bonuses (normal Vietnamese, 17 rows)
- §25.10 Weapon Families (3 rows)
- §25.11 Combat Mechanics (canonical, user-confirmed 2026-05-31) — 15 rows with explicit casing + rationale for each, including the 2 keep-EN entries (Guild War, Ultimate)

Also added a single glossary row for `Võ đài | Arena | 止戈 (zhǐgē)` in the existing §19-ish glossary table (~line 621).

### Prior-session-uncommitted files included in this commit

These were modified before this session started but never committed; they are the supporting infrastructure for `capnhat-v17.html`:
- `AGENTS.md` — added "Logging manual translations" rule pointing to KNOWLEDGE.md as canonical glossary (this is what §25 implements).
- `index.html` — sidebar nav link to `capnhat-v17.html`; cache-bust bumped `?v=20260525-9 → ?v=20260528-2`.
- `styles.css` — `.patch-doc` + `.patch-topbar` + `.patch-section` + `.patch-version-tag` etc. styles for the standalone patch-note page (167 lines).
- `CLAUDE.md` (project root) — graphify rules added.
- `graphify-out/` — knowledge graph snapshot (graph.json, graph.html, GRAPH_REPORT.md, manifest.json, cost.json) for the project-CLAUDE.md graphify query rule to work on other machines.
- `.sessions/2026-05-25-0000-init-session.md` — prior init stub.
- `.sessions/2026-05-25-2100-guild-war-tank-tongquan-source-audit.md` — prior session note from session 19.

### `.gitignore`

Extended to include `.claude/settings*.json`, `.claude/graphify-out/cache/` (local config + regenerable cache).

### Deliberately excluded from staging

- `images/shared/TDL2026.png` (12 MB unused asset, still holding per project notes).
- `.claude/settings.json` + `.claude/settings.local.json` (per-machine config).
- `.claude/graphify-out/cache/` (regenerable).

## Open follow-ups

- **§II B is the only sub-section explicitly translated to-VN for combat mechanics**. Other §II sub-sections (A "Sức bền…", C "Guarding Qi Core…", D "Vũ khí…", E "Công pháp & Tâm pháp", F "Mystic Skill", G "Arena Định âm") still have leftover English (`Tenacity`, `Stagger`, `Knockback`, `Deflect`, `Dodge`, `Perfect Dodge`, `Invincibility`, etc.). §25.11 is now canonical; ask user whether to extend the §II B translations across the rest of the file for consistency. Current state: §II B is more Vietnamese than the rest of §II.
- **`Concentration` casing inconsistency**: I left `Khán Phá` / `Ngưng Tâm` capitalised, but Pito's later casing direction for stat-line bonuses was sentence-case. Buffs-as-named-states stay capitalised (proper-noun-like) which is the intended split, but worth a flag in case Pito prefers all caps-after-first-letter-only.
- **B2 Power conversion re-test** still pending (carried from session 19).
- **`images/shared/TDL2026.png`** still untracked — same status, 12 MB unreferenced, hold off.
- **§25.11 entries** are the first canonical user-confirmed translations for *generic combat mechanics* (vs. proper-noun named entities in §25.2/3). Future patch-note translation work should consult §25.11 first.

## Cross-project lessons

- **Look up the source first, don't guess** — for translation work that crosses languages, an authoritative source is non-negotiable. My first table was guesses based on training-data familiarity with the game; user rejected it with "this is not the correct approach, look up the CN patch note". The `curl`-and-regex-extract-`「name」` approach gave 227 verified CN entities in one shot. Bake into the workflow: identify the source, fetch it, extract structurally, then map — never guess.
- **WebFetch summarisation is lossy for large pages** — the WebFetch tool's auto-summarisation dropped many entities on the first fetch ("Note: Several terms from your English list... are not discrete named buff/debuff states with Chinese labels in these sections"). Working around it by `curl`-ing the raw HTML and parsing locally was reliable. Pattern: if WebFetch returns suspiciously short or "I couldn't find X" results, escalate to `curl` + local parsing.
- **Section-context-aware translation requires segmenting then dispatching** — the `Concentration` case (one EN word → two VN translations depending on which school's h3 governs the segment) was solved cleanly by: split content on h3 boundaries → detect each segment's school via h3 marker text → use a `CONTEXT_MAP` per EN term with school-keyed VN options. Pattern: when an EN term ambiguously translates depending on context, push the context detection into a transform-level map rather than per-occurrence editing.
- **First-occurrence parenthetical rule scopes per `<h3>`, not per file** — Section III contained multiple already-translated terms appearing for the first time *in that section*, so the script correctly gave them `(EN)` parenthetical even though they'd been translated earlier in §I/§II. This is the right behaviour for a long single-page document; each section reads standalone.
- **The hook-injected SessionStart reminder cannot be the verification mechanism it tries to be** — if the hook tells me what to greet with, my greeting proves nothing about whether I read CLAUDE.md. Verification tests need to require a token the model cannot already see in the immediate context. Pattern for any "did you read X?" check: have the test require quoting a specific token from X that the hook does NOT provide.
- **Session-start protocol pays back even on a clearly-scoped task** — running the full read-rules / read-AGENTS / read-projects / read-experiences / read-sessions sweep after the user called me out surfaced AGENTS.md §19 (the canonical-glossary logging rule that justifies §25's existence), the cache-bust state (`?v=20260528-2` for `capnhat-v17.html`), known issues (B2 re-test, TDL2026.png holding), and the reflection-due state (`sessionCount=20 % 5 === 0`, reflection pending at session end). Doing this at the *start* would have produced the same insights cheaper.
- **Casing direction often comes after the translation choice** — user revised casing on multiple terms (`Tỷ` not `Tỉ`, `Tăng cường kết toán khi tụ lực` lowercase, `bất hoại` and `đạn` lowercase). Don't assume Title Case or sentence-case without checking; surface the casing as part of the proofread table when ambiguous.
