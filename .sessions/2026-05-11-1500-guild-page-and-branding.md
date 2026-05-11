---
date: 2026-05-11
slug: guild-page-and-branding
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

Multi-step request that arrived in pieces over the session:

1. Add a new top-level page **"Guild Lunar Capital"** (right under the intro) with recruitment info from the provided poster: 3 selling points, weekly schedule, Discord link, monthly giveaway detail, other games.
2. Reframe the intro page from solo "mình" voice to **guild-collective "chúng tôi" voice** — title changes to "Chúng tôi là guild Lunar Capital", preface card rewritten, warn-card routed to Discord.
3. Clarify that this is now a **dual-purpose site** — both a guide for guild members and a recruitment/marketing tool for the guild.
4. **Monthly giveaway detail**: 2 Battle Pass giveaways per month — one for healers, one for guild-war participants.
5. **Branding**: site title → "Lunar Capital GuideBook"; sidebar text logo → `logo.png` image; favicon → `logo.png`.
6. **Brand gradient**: every visible "Lunar Capital" text instance gets a pastel-rainbow gradient matching the logo's palette.
7. **Discord card**: gradient link + QR code (provided as `linkbreakers-io-qr-code-generator.svg`) anchored to the right border.
8. Final wording change: intro hero h1 → **"GuideBook dành cho Lunar Capital"**.
9. Logo path correction: `logo.png` lives at `images/shared/logo.png`, not project root.

## Plan / decisions

- **Plan-mode round-trip**: Used the plan-mode workflow to draft the Guild page + intro reframe before implementing; user approved with one tweak (added monthly giveaway detail).
- **Translation/design questions** asked via AskUserQuestion before writing copy: (a) full preface rewrite vs. surgical paragraph edit — chose full rewrite for tonal consistency; (b) QR display — chose text-link + serial first (later user added the SVG QR so we got both); (c) warn-card — reframed to point at Discord guild.
- **Card pattern parity**: Followed the existing playbook of distinct CSS class families per visual semantic — `.guild-features` (3-up overview tiles), `.guild-giveaway-grid` (2-up reward tiles), `.guild-schedule-row` (day-chip + event + time row), `.guild-discord` (Discord CTA card with QR on the right). All scoped to `#page-guild` so they don't leak globally.
- **Dual-purpose framing**: Educational reader → potential recruit funnel. Added a soft `.inline-link` button at the end of the preface paragraphs that navigates to the Guild page when clicked. Broadened the `[data-goto]` JS handler from `.toc-item`-only to `[data-goto]`-any, so the new inline-link works without duplicating handlers.
- **Brand gradient `.lc-grad`**: Single class applied via `<span>` wrappers. Pastel 5-stop linear-gradient `(135deg, #f5b9d6 → #b8e7d5 → #b9d6f0 → #d2c0e8 → #f5cdb3)` clipped to text using `background-clip: text` + `color: transparent`. Sampled directly from the logo image. Applied to all 8 visible "Lunar Capital" instances (skipped `<title>` and `alt=""` since those can't contain HTML).
- **Logo path bug**: First pass referenced `logo.png` at project root (didn't exist); fixed after a `find` revealed it at `images/shared/logo.png`.
- **QR on right of Discord card**: Switched `.guild-discord` layout from `flex-direction: column` to a flex row — text block flexes (`flex: 1`), QR is fixed 120×120 with white inner padding so the dark SVG-on-dark-card stays scannable. Mobile stacks vertically with QR centered at 140×140.
- **Forward-looking line** in preface: "Guidebook sẽ tiếp tục phát triển miễn là guild còn tồn tại — sắp tới sẽ có thêm guide cho nhiều class DPS khác."
- **Cache-bust discipline**: Bumped the stylesheet `?v=` query string twice this session (`20260512`, `20260512-2`, `20260512-3`) because the stale-CSS rule (e021) requires it whenever new rules referenced by new HTML classes are added.

## Files changed

- `index.html`:
  - `<title>`: `"Where Winds Meet · Build Guide VN"` → `"Lunar Capital GuideBook"`.
  - Added `<link rel="icon" type="image/png" href="images/shared/logo.png">`.
  - Stylesheet link: `?v=` bumped to `20260512-3`.
  - Sidebar header: text logo replaced with `<img src="images/shared/logo.png" class="sidebar-logo-img">`; sidebar title → "Lunar Capital GuideBook".
  - Added new nav item `<button data-page="guild">` between Intro and Tank guide.
  - Intro hero h1 reworded twice — final: **"GuideBook dành cho Lunar Capital"**; sub rewritten to collective tone.
  - Preface card: h2 → "Về guide này"; replaced 4 solo paragraphs with 5 collective paragraphs (greeting → who we are → method → roadmap promise → CTA-to-Guild via `.inline-link`).
  - Warn-card: feedback channel changed from "nhắn cho mình" to Discord link `dsc.gg/lunarcapital`.
  - Inserted new `<section id="page-guild">` between intro and tank pages. 5 cards: 3-up features overview · 2-up monthly giveaway · 5-row weekly schedule · Discord CTA with QR on the right · "Giải trí khác" one-liner.
  - Wrapped 8 visible "Lunar Capital" instances in `<span class="lc-grad">` for the brand gradient.
  - JS `[data-goto]` handler broadened from `.toc-item` to any `[data-goto]` element.
- `styles.css`:
  - Added `.sidebar-logo-img` (64×64 contain block, centered).
  - Added `.lc-grad` (pastel 5-stop linear-gradient clipped to text).
  - Added `.inline-link` (transparent button styled as inline text link, gold underline).
  - Added Guild page patterns: `.guild-features` + `.guild-feature` (3-up grid), `.guild-giveaway-grid` + `.guild-giveaway` (2-up grid), `.guild-schedule-list` + `.guild-schedule-row` + `.guild-schedule-day/event/time` (3-col schedule rows), `.guild-discord` (flex row with `.guild-discord-text` flexing + `.guild-discord-qr` fixed 120×120).
  - Mobile media query at 720px stacks all the multi-col patterns.

## Open follow-ups

- **`images/shared/TDL2026.png`** (12.7 MB) is present in the assets folder but not referenced anywhere yet. Likely a stash for future use. Holding off committing it until it's used or the user confirms.
- **`.DS_Store` files** appeared from macOS Finder browsing; suggest adding to a project `.gitignore` to stop them from polluting future commits.
- **Brand consistency on other surfaces**: Tank/Heal/Chỉ số page heroes still use plain silk-colored h1s. If we want full brand cohesion, those h1s could pick up `.lc-grad` for the entire site title pattern — but currently scoped only to the literal "Lunar Capital" wordmark, which seems intentional.

## Cross-project lessons

- **Brand gradient as a reusable text class**: `background-clip: text` + 5-stop gradient + `color: transparent` is a low-overhead way to apply a holographic/pastel brand mark across arbitrary text. Wrapping each visible occurrence in `<span class="lc-grad">` keeps the rule trivial and selective; no need for descendant selectors or color overrides.
- **Plan-mode for additive page work pays off**: A 6-card new page with custom CSS patterns benefits from drafting in plan mode + AskUserQuestion for translation choices. Faster than implementing first and re-correcting.
- **Asset paths drift**: When the user says "use logo.png", search the whole asset tree before committing to a path. The convention in this project is `images/shared/`, not project root — a one-line `find` saved a broken-image bug.
- **Multi-step session = single log entry is fine** when the work is thematically coherent (here: branding + recruitment surface). The slug captures the theme, the body captures the chronology.
