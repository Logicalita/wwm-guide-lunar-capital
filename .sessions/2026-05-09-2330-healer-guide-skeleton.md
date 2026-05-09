---
date: 2026-05-09
slug: healer-guide-skeleton
files_touched:
  - index.html
  - images/heal/.gitkeep
bugs_recorded: []
---

## User request

Duplicate the existing **Guide to top DPS Tank** dropdown structure as a parallel **Guide Healer thần** for the healer role. All sub-pages start as placeholders (no real content provided yet). Also create an `images/heal/` asset folder for future healer images.

## Plan / decisions

- **User-confirmed inputs** (via AskUserQuestion this session):
  - Parent subtitle: **Silkbind-Deluge** (in-game class name).
  - Parent icon: `ti-flower` (lotus / divine, fits *thần*).
- **Mirror exactly, don't redesign** — the four sub-pages reuse Tank's icons (`ti-target`, `ti-book-2`, `ti-flame`, `ti-sword`) and Vietnamese subtitles (Tối ưu hóa, Căn cơ trường phái, Tuyệt học, Hoàn thiện build) since the page-type semantics are class-agnostic. The sub-page titles also mirror Tank, with the healer-specific entry-page h1 set to `Hướng dẫn build chí mạng Healer` (parallels Tank's `Hướng dẫn build chí mạng Tank`).
- **`data-page` slugs** use the `heal-` prefix to match the asset folder name: `heal-chiso`, `heal-congphap`, `heal-tampphap`, `heal-trangbi`. Section IDs follow the established `page-<slug>` pattern.
- **All four pages start as placeholders** per the project AGENTS.md rule (`.warn-card` with `ti-tools` and the standard "Đang cập nhật…" copy). Tank's `Chỉ số recommend` happens to have real content, but that's class-specific data — Healer's recommend page starts blank and gets filled in later.
- **Document order** keeps both class guides grouped: the four `page-heal-*` sections sit between `page-tank-trangbi` (last tank page) and `page-chiso-tancong` (first stat page). Sidebar order matches: `nav-group-tank` → `nav-group-heal` → `nav-group-chiso`.
- **TOC** got a third entry between Tank and Chỉ số, matching the parent-group naming convention (`Guide Healer thần (Silkbind-Deluge)`). Icon mirrors the parent nav (`ti-flower`). `data-goto="heal-chiso"` so clicking it auto-opens the new dropdown via the existing toggle JS.
- **Zero CSS / JS / AGENTS.md changes** — the `.nav-group` styles, the toggle handler's `querySelectorAll`, the placeholder pattern, and the `images/<class>/` folder convention all already exist. The new healer skeleton picks them all up automatically.

## Files changed

- `index.html`:
  - Sidebar — inserted `<div class="nav-group" id="nav-group-heal">…</div>` between `#nav-group-tank` and `#nav-group-chiso`. Mirrors Tank's markup with the new title/subtitle/icon and four `nav-child` buttons.
  - Intro TOC — inserted a new `<button class="toc-item" data-goto="heal-chiso">…</button>` between the existing Tank and Chỉ số entries.
  - Page sections — inserted four new `<section class="page" id="page-heal-…">` blocks immediately before `#page-chiso-tancong`. Each follows the placeholder pattern: hero with `Healer · <subpage>` eye + sub-page h1, then a `.warn-card` with `ti-tools` and the standard "Đang cập nhật…" copy.
- `images/heal/.gitkeep` — new empty file to establish the folder.

## Open follow-ups

- Fill in real content for each healer sub-page when the user has it. Replace the `.warn-card` placeholder with real cards.
- The sub-page icons (`ti-sword` for Trang bị specifically) are tank-flavored. If the user wants healer-specific icons later (e.g. `ti-flask` or `ti-leaf` for healer Trang bị), swap in place.
- The Healer entry h1 currently says `Hướng dẫn build chí mạng Healer` (mirroring Tank). If healer doesn't actually go for chí mạng builds, this needs to change when content arrives.

## Cross-project lessons

None recorded — this is structural duplication using already-established patterns. No bug pattern surfaced. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
