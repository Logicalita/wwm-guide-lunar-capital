---
date: 2026-05-09
slug: tank-dropdown-and-session-workflow
files_touched:
  - index.html
  - styles.css
  - images/tank/.gitkeep
  - images/shared/.gitkeep
  - .sessions/.gitkeep
  - <HOME>/ai-optimizer/AGENTS.md
bugs_recorded: []
---

## User request

Two threads of work in one conversation:
1. Restructure the Tank guide nav into a collapsible "Guide to top DPS Tank" dropdown with multiple sub-pages, then iterate on which sub-page holds the content.
2. Establish an image-asset folder convention and add a per-project session-log workflow so future sessions can read recent context.

## Plan / decisions

- **Sidebar dropdown**: replaced the single `Chỉ số Tank` nav item with a `nav-group` containing a toggle + four `nav-child` items. Toggle expands/collapses; clicking a child auto-expands the parent. Active state lives only on the active child.
- **Sub-page order**: `Chỉ số recommend` (entry, has all current content) → `Công pháp` → `Tâm pháp` → `Trang bị`. The latter three are placeholders with a "Đang cập nhật" notice; the user will fill them in over time.
- **Subtitles**: sentence case (`Tối ưu hóa`, `Căn cơ trường phái`, `Tuyệt học`, `Hoàn thiện build`).
- **Icons**: parent stays `ti-shield-check`; children use `ti-target` (Chỉ số recommend), `ti-book-2` (Công pháp), `ti-flame` (Tâm pháp), `ti-sword` (Trang bị) — `ti-target` migrated from Công pháp to the new entry page when content moved.
- **Intro TOC**: now points to `tank-chiso` (the entry page) so the dropdown auto-expands when navigated from the table of contents.
- **Layout decision** (deferred): keep `index.html` monolithic. Multi-file split deferred until ≥3 class guides exist. Asset organization handled by new `images/<class>/` and `images/shared/` folders — convention is implicit via folder existence, no AGENTS.md text.
- **Session-log workflow**: per-project logs live in `<project>/.sessions/`. Cross-project bugs/experiences continue going to `<HOME>/ai-optimizer/memory/`. Workflow rules documented only in global `ai-optimizer/AGENTS.md`; project AGENTS.md inherits via its existing "follow the global loop" line and stays untouched.

## Files changed

- `index.html` — sidebar `nav-group` with toggle + 4 sub-items; split single `page-tank` section into four `page-tank-{chiso,congphap,tampphap,trangbi}` sections; intro TOC `data-goto` updated to `tank-chiso`; nav script updated to handle `nav-group-toggle` (with `aria-expanded`) and to auto-open the parent group when a child is activated.
- `styles.css` — added `.nav-group`, `.nav-chevron` (rotates when open), `.nav-children` (hidden by default, shown when `.nav-group.open`, indented with left border), `.nav-child` (slightly smaller padding/font).
- `images/tank/.gitkeep`, `images/shared/.gitkeep` — established asset folders.
- `.sessions/.gitkeep` — established the session-log folder.
- `<HOME>/ai-optimizer/AGENTS.md` — added session-log read step to "Plan" and split "Record" into cross-project + per-project sub-bullets.

## Open follow-ups

- Fill in the three placeholder pages: `Công pháp`, `Tâm pháp`, `Trang bị`.
- When concrete images are added under `images/tank/`, reference them from the corresponding sub-page cards.
- Consider updating `<HOME>/ai-optimizer/memory/playbooks/TEMPLATE-project-AGENTS.md` so newly bootstrapped projects automatically pick up the `.sessions/` workflow without any project-AGENTS.md text. **Out of scope this session.**
- Revisit a multi-file HTML split if a third class guide is added (current count: 1 class guide + 1 explainer page).

## Cross-project lessons

None recorded this session — no bug patterns surfaced. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
