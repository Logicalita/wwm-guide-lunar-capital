---
date: 2026-05-25
slug: guild-war-tank-tongquan-source-audit
files_touched:
  - index.html
  - styles.css
  - KNOWLEDGE.md
  - AGENTS.md
  - .sessions/2026-05-25-0000-init-session.md (stub kept)
  - images/guild-war/ (13 new PNGs)
  - images/tank/ (5 new PNGs)
bugs_recorded: []
---

## User request

Long-running session covering three distinct workstreams:

1. **Build out the new "Guild War" section** — top-level entry in the sidebar with one hub + four subpages (Phân tích đội hình, Lối chơi cơ bản, Mẹo & Thủ thuật, Tune & Kỳ thuật). Drop in roster images, then iterate on visuals: convert the team-formation reference image to a native 2-col HTML/CSS infographic; pick role icons (Tank shield-half, Healer cross variants, DPS sword); colour the class tags (VD = blue, SD = purple, QC = green); split the single tune-priority card into 5 slot-specific cards (Vũ khí / Áo giáp / Mũ / Chân / Tay) with hot-cold priority badges; populate each tune card with its actual buff screenshot.
2. **Adopt wherewindsmeetcalculator.com/wiki/damage-formula as a high-trust knowledge source** for the guide and for future agents. Audit current damage-formula claims against the wiki. Apply fixes where the wiki adds load-bearing detail or where the guide's data was lifted from a less-trusted aggregated source. Per user decision during plan review: keep guide values for Body conversion (+72 HP/point, in-game tested) and Kháng (45%, in-game tested); adopt the wiki's `(Pen − Res) / 200` formula for negative-penetration over the older `Enemy Defense / (Enemy Defense + 1000)` soft cap.
3. **Add a "Tổng quan" landing subpage to the Tank guide** that summarises tâm pháp, set trang bị, the 56% effective crit target, and a stat-recommendation table for cấp thế giới 14 — with a TOC linking out to the other tank subpages. Then **hide placeholder pages** (Tank Tâm pháp + Trang bị, the entire Healer skeleton) until real content lands.

## Plan / decisions

### Guild War section (plan-mode)
- **Architecture conflict surfaced via AskUserQuestion**: the user's spec called for `guild-war/index.html`, `guild-war/phan-tich-doi-hinh.html` etc., but the project is a single-page SPA with `<section id="page-XXX">` blocks toggled by `data-page`. Surfaced the conflict before implementing. User chose a hybrid — SPA sections inside `index.html` plus a real `images/guild-war/` asset folder for PNGs.
- **Back-link pattern**: each subpage gets a `.subpage-backlinks` row at the end (`← Guild War · Trang chủ`) using the existing `.inline-link` button + `data-goto` mechanism. Hub doesn't need back-links because its `.toc` cards are the outbound nav.
- **Image markup**: `<figure>` + `<figcaption>` with minimal CSS scoped to a new `.guild-war` parent class on each section.
- Used a Plan subagent in addition to Explore to validate component reuse (`.row`/`.row-src`/`.row-val` for skill lists, `.warn-card` with `ti-clock` for the timing-milestones callout, `.toc`/`.toc-item` for hub navigation cards). Avoided rebuilding components already in `styles.css`.
- Wrote the plan to `~/.claude/plans/context-i-m-adding-a-eager-pillow.md` then ExitPlanMode → execute.

### Formation infographic conversion
- The user later asked to convert a reference image (`doi-hinh-can-bang-15-15.png`, never committed) into a native 2-col CSS infographic. Asked one design question before building (site-native gold palette vs faithful yellow/green/blue from the screenshot) — user chose site-native.
- Then iterated on icon choices for DPS and Healer (final: `ti-sword` for DPS, `ti-heart-plus` for Healer), with Healer text styled like Tank (serif/500/uppercase/0.06em letter-spacing) but in a healing mint `#7fc59a` distinct from `--jade-bright`.
- Then iterated on class-tag colours per user direction: **VD = #4f6f8c blue**, **SD = #6e5d8a purple**, **QC = #5b7d5d sage green**, all with silk text and matching borders. Deliberately deviates from AGENTS.md's jade/cinnabar rank-quality reservation but stays scoped under `.guild-war`.

### Tune cards (5-slot split)
- Replaced the single `.row`-based priority list with 5 separate `.card`s (Tune Vũ khí / Áo giáp / Mũ / Chân / Tay), each with:
  - Slot-specific icon: `ti-sword`, `ti-shirt`, `ti-helmet`, `ti-shoe`, `ti-hand-grab`
  - A new `.tune-priority` badge sitting directly under the h2 — colour-coded by priority (cinnabar-tinted `.is-high` for hot, gold for medium, neutral outline for skip)
  - Either a `<figure>` with the actual in-game tune effect screenshot (when supplied) or a `warn-card` placeholder
- The `.is-high` badge intentionally uses `--cinnabar-bright` (red) — another deliberate AGENTS.md deviation, scoped to `.guild-war`. CSS comment documents the override.

### Source-priority audit (plan-mode #2)
- Fetched `wherewindsmeetcalculator.com/wiki/damage-formula` with WebFetch and diffed against existing claims in `index.html` (Chỉ số subpages) and `KNOWLEDGE.md`.
- Categorised every difference into **A — already consistent**, **B — wiki adds load-bearing detail (high-confidence fix)**, **C — wiki conflicts with in-game data**, **D — wiki silent (keep guide)**.
- User triaged the C cases mid-plan: **C1 Body conversion** keep guide's 72 HP/point; **C2 Kháng** keep guide's 45% (treat wiki's 1.15 multiplier as a separate Panel/Display mechanic); **C3 Negative Pen formula** adopt the wiki's `(Pen − Res) / 200` and drop the old `Defense / (Defense + 1000)` line.
- For **B2 Power (Kình lực)** — wiki says +0.225 Min ATK + 1.36 Max ATK; guide says only +1.35 Max ATK. The in-game test on 2026-05-11 measured only the Max delta. Kept guide value but left a "pending in-game re-test" note inline.

### Tổng quan landing page
- Added a new `tank-tongquan` nav-child at the top of the Tank nav-group (above `tank-chiso`) and a new `<section class="page tank-tongquan" id="page-tank-tongquan">`.
- Cards: Tâm pháp 2-up (inner-set-1/2 with trade-off captions); Set vũ khí (rainwhispers); Set áo giáp (dich-tuong + quy-nguyet in a flex row with italic gold "hoặc" separator); key-card (56% target); a stat-table card for cấp thế giới 14 (Độ chính xác 100%, Tỷ lệ chí mạng 56%, Tỷ lệ hiểu ý 15.4%, Tỷ lệ chí mạng trực tiếp 4.6%, Công kích vật lý 1300–2200, Công kích thuộc tính 350–750); Nội dung guide TOC.
- The user then asked to drop two earlier explanatory cards ("Vì sao Mạc Đao là top DPS Tank" + "Hướng đi cốt lõi") in favour of the more concrete stat-recommendation table. Done — page now pivots from prose to data.

### Hide placeholder pages
- Added a single `.is-hidden { display: none !important; }` utility class to `styles.css`.
- Applied it to 12 elements: 2 tank nav-children (tâm pháp, trang bị), the whole heal nav-group, 6 placeholder `<section>` blocks (tank-tampphap, tank-trangbi, heal-chiso, heal-congphap, heal-tampphap, heal-trangbi), and 3 toc-items (intro page's Healer card + Tổng quan TOC's tâm pháp + trang bị toc-items).
- Reversible per-page when content lands — just remove the class from the nav-child + section + toc-item for that page.

## Files changed

### `index.html` (632 lines added, 19 removed in the final commit)
- Cache-bust bumped 9 times this session (final `?v=20260525-9`).
- New sidebar nav-group `nav-group-guildwar` with 5 children (Tổng quan / Phân tích đội hình / Lối chơi cơ bản / Mẹo & Thủ thuật / Tune & Kỳ thuật), inserted after the Chỉ số group.
- New `<section class="page guild-war">` for each of the 5 Guild War subpages, appended before `</main>`.
- Native 2-col formation infographic on Phân tích đội hình (`.gw-formation` → 2 `.gw-formation-col` → 3 `.gw-group` each → header + roster `<ul>` with `.gw-row.is-tank` / `.is-healer` / `.is-dps`).
- 5 tune cards on Tune & Kỳ thuật page with `.tune-priority.is-high` / `.is-medium` / `.is-skip` badges + figures for each slot's actual buff screenshot.
- New `tank-tongquan` nav-child + `<section class="page tank-tongquan">` inserted before `page-tank-chiso`.
- 12 `.is-hidden` taggings on placeholder elements.

### `styles.css` (~327 lines added)
- `.guild-war`-scoped rules: figure chrome, `.subpage-backlinks`, `.gw-team-grid`, `.gw-list`, h3 styling, `.gw-formation` 2-col grid, `.gw-formation-col`, `.gw-team-col-head`, `.gw-group`, `.gw-group-head`, `.gw-group-label`, `.gw-lane`, `.gw-group-roster`, `.gw-row` + `.is-tank` / `.is-healer` / `.is-dps`, `.gw-tag` + `.tag-vd` / `.tag-sd` / `.tag-qc` (the blue/purple/green tags), `.tune-priority` + `.is-high` / `.is-medium` / `.is-skip`.
- `.tank-tongquan`-scoped rules: figure chrome, `.tq-2col`, `.tq-or-row`, `.tq-or` separator.
- `.is-hidden { display: none !important; }` utility.
- All new rules scoped under feature classes (`.guild-war`, `.tank-tongquan`) — no global rules leak. Two AGENTS.md colour-rule deviations (cinnabar for high tune priority, jade-adjacent green for Healer text + QC tag) are CSS-commented as deliberate overrides.

### `KNOWLEDGE.md` (~43 lines net change)
- Replaced the 1-line "sources" blurb at the top with a full **Sources of Truth (priority order)** block: (1) in-game observation > (2) wherewindsmeetcalculator.com/wiki > (3) Chinese community sources > (4) English aggregated wikis > (5) Fextralife NOT TRUSTED.
- §2 Hit Resolution: added the **Affinity-squeezes-Crit when x + y > 100%** rule with the natural-cap worked example (80 + 40 → 40 Aff / 60 Crit / 0 Normal); added the C2 Kháng/1.15 discrepancy note.
- §3 Damage Calculation: replaced the `Defense / (Defense + 1000)` soft-cap line with the wiki's `(Pen − Res) / 200` for negative Pen Zone; added an explicit "(Pen − Res) / 100 for positive" line; added the `Crit DMG = Base × (1 + Base + Bonus)` and `Affinity DMG = Max ATK × (1 + Base + Bonus)` full formulas; added a Damage Deepening note.
- §3 new subsection **Healing Math (special case)**: Precision/Graze/Affinity ineffective on heals, only Crit/non-Crit branches; Physical ATK + Qiansi only; ±10% independent fluctuation; +100% low-level dungeon healing bonus. (Placed as §3 sub-subsection because §5 "Derived Combat Stats" already existed — avoided renumbering.)
- §4 Core Attributes: appended discrepancy notes for Body (wiki 60 vs in-game 72, in-game wins) and Power (wiki +0.225 Min ATK component pending in-game re-test).

### `AGENTS.md` (~3 lines added)
- New "Sources of Truth" section pointing to KNOWLEDGE.md's priority block and reiterating the chain.

### New assets committed
- `images/guild-war/`: 13 PNGs (z-du-cong, z-song-dao, van-hanh-team-thu-15-nguoi, van-hanh-team-cong-15-nguoi-v1, moc-thoi-gian-quan-trong, trick-unstuck, ky-thuat, buff-chay-nhanh, tune-vk, tune-shirt, tune-helmet, tune-shoe, tune-hand-grab)
- `images/tank/`: 5 PNGs (inner-set-1, inner-set-2, rainwhispers, dich-tuong, quy-nguyet)

### Commit
Single commit `5b03750` titled "Add Guild War section, Tank Tổng quan landing, source-priority audit" — 22 files changed, +986 / −19. Pushed to `origin/main`. Deliberately excluded from staging: `.claude/settings*.json` (local config), `.sessions/2026-05-25-0000-init-session.md` (stub), `images/shared/TDL2026.png` (13 MB unreferenced asset still pending).

## Open follow-ups

- **B2 in-game re-test pending**: Power (Kình lực) — add ~20 points via respec, measure Min ATK delta. If Δ ≈ +4.5 → update `KNOWLEDGE.md` §4 conversion table to `+0.225 Min ATK + 1.35 Max ATK` and matching `chiso-attrs` row 1276.
- **Healer chỉ số content**: When the Silkbind-Deluge build is ready to ship, the `.is-hidden` flag comes off the heal nav-group + 4 sections + intro TOC card. Healing-math reference is already in KNOWLEDGE.md §3 sub-subsection.
- **Tank Tâm pháp + Trang bị**: same — when content lands, unhide. Tổng quan TOC already has the placeholder toc-items ready, just remove the `.is-hidden` class.
- **`images/shared/TDL2026.png`** (13 MB) still untracked and unreferenced; same status as the previous session — holding off until used or confirmed disposable.
- **`.sessions/2026-05-25-0000-init-session.md`** stub is still untracked. Can be deleted or left.

## Cross-project lessons

- **HTML comments don't nest. CSS utility classes do.** When the user asked to "hide" 12 placeholder elements, a single `.is-hidden { display: none !important; }` rule + tagging each element with that class was much cleaner than commenting out 12 blocks (which would have risked nested-comment breakage from the section-header HTML comments I'd added earlier). Class-based hiding is also reversible per-element via git's `--patch` or even via a tiny dev-console toggle. Comment-based hiding is reversible only at the file-edit level.
- **Plan-mode for additive structural work pays again**: Both big workstreams (Guild War section, source-priority audit) used Plan agent + Explore agent in Phase 1, AskUserQuestion in Phase 3, then ExitPlanMode → execute. The mid-plan user revision on the audit (C1/C2 keep guide, C3 adopt wiki) was clean to apply via incremental edits to the plan file because the plan was structured as a categorised diff to begin with. Categorising findings into A/B/C/D up front made the user's per-case decision low-effort.
- **Source-priority hierarchy belongs in the agent reference file, not buried in code**: Adding a dedicated "Sources of Truth" block at the top of KNOWLEDGE.md + a 1-line pointer in AGENTS.md means future agents (including future me) see the priority chain before reading any specific claim. Without that, the only way to know which sources to trust would be to read the experience log retroactively.
- **The user is willing to deviate from project-level colour reservation rules** (`--cinnabar` reserved for rank-worst, `--jade` for rank-best) when (a) the deviation carries clear tactical/role meaning and (b) it's scoped via a feature class so it can't leak to the rank-quality surfaces. Documented this case-by-case in CSS comments. Saved as a feedback memory for future sessions.
- **Substring matches in replace_all are dangerous**: nearly broke `ti-users-group` when running `ti-user` → `ti-sword` replace_all. Caught it by reviewing matches before applying. Anchor `ti-user"` (with closing quote) or use the full `<i class="ti ti-user"></i>` pattern when icon-swapping.
- **`<figure>` chrome needs scoping**: There's no global `figure`/`figcaption` style in this project. Each feature area (`.guild-war`, `.tank-tongquan`) declares its own figure chrome. This is fine for a small site but worth turning into a global utility if a 3rd feature needs the same chrome.
