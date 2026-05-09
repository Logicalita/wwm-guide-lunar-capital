---
date: 2026-05-09
slug: chiso-group-and-damage-flow
files_touched:
  - index.html
  - styles.css
  - AGENTS.md
bugs_recorded: []
---

## User request

Six tasks in one shot:
1. Sidebar subtitle for "Chỉ số tấn công": "Chung cho mọi phái" → "Cho trường phái build crit".
2. Wrap "Chỉ số tấn công" inside a new dropdown nav-group **Chỉ số** (subtitle "Tất tần tật về chỉ số") so future stat guides have a home.
3. Hero subtitle on the chỉ số tấn công page: "Cách hệ thống stat hoạt động và cách roll gear hiệu quả cho mọi phái." → "Cách hệ thống stat hoạt động và lựa chọn chỉ số cho build chí mạng".
4. Add a second sub-page **Sát thương đầu ra** under "Chỉ số" (no sidebar subtitle).
5. Convert the attached "Damage output 1" probability tree image into an on-brand infographic and put it as the content of the new page.
6. Add a project-level rule: any new empty page/sub-page must show the standard "Đang cập nhật…" warn-card.

## Plan / decisions

- **Nav structure** mirrors the existing Tank dropdown pattern (`nav-group` + `nav-group-toggle` + `nav-children`). New parent uses `ti-chart-arcs` icon. Children use `ti-sword` (existing Chỉ số tấn công icon retained) and `ti-bolt` (Sát thương đầu ra). Children-without-subtitle are allowed — the user explicitly requested no subtitle for Sát thương đầu ra; visual layout still works because `.nav-item-sub` is just an extra div.
- **Page IDs renamed**: `page-general` → `page-chiso-tancong` and the JS `data-page` value follows. The old name no longer fit the new parent grouping. New page is `page-chiso-damage`. Historical session-log mention of `#page-general` is left intact as a time-frozen snapshot.
- **Intro TOC** updated to point at the new entry sub-page (`chiso-tancong`) and labelled with the parent group name "Chỉ số (Tất tần tật về chỉ số)" — matches the Tank TOC convention of pointing at the group via its entry page.
- **Hero on chỉ số tấn công** also got an `eye` change ("tất cả các phái" → "Chỉ số · Tấn công") to match the `<group> · <subpage>` eye pattern already used by every Tank sub-page. Not in the user's six asks but needed for visual consistency under the new grouping.
- **Infographic (Task 5)** rendered as inline SVG inside a `.card`. Why SVG over flex/grid HTML: the source image is genuinely a tree with diagonal connectors, which is awkward in CSS but trivial in SVG bezier paths. SVG also scales cleanly with the existing hero-mountains pattern. Color mapping uses the site palette — Critical = `--cinnabar-bright` (red, matching tg-crit), Affinity = `--jade-bright` (green, matching tg-aff for "hiểu ý"), Abrasion = `--bone` (cool grey, mirrors the muted blue underline in the source), Attack root = `--gold-bright` (focal). Original yellow/orange from the source image was *not* preserved — site identity for these stats is already cinnabar/jade and consistency wins over literal source-color match.
- **Mobile** handled via `.dmg-flow-wrap` with `overflow-x: auto` and `min-width: 580px` on the SVG. Vertical re-layout on narrow screens deferred — horizontal scroll is acceptable for a one-off diagram.
- **AGENTS.md placeholder rule** added under "Conventions" so it lives next to the other content rules, not in the workflow section.

## Files changed

- `index.html`:
  - Sidebar: replaced standalone `Chỉ số tấn công` nav-item with new `nav-group#nav-group-chiso` containing two `nav-child` buttons (`chiso-tancong`, `chiso-damage`).
  - Intro TOC: `data-goto`, title, subtitle, and icon updated to point at the new group.
  - Renamed `<section id="page-general">` to `<section id="page-chiso-tancong">`; updated `eye` to `Chỉ số · Tấn công` and `.sub` to the user-supplied copy.
  - Inserted new `<section id="page-chiso-damage">` after the chỉ số tấn công page, containing hero + card with intro paragraph + SVG tree (`.dmg-flow`) + footer note explaining the variables.
- `styles.css`: added `/* === Damage flow infographic === */` block with `.dmg-flow-intro`, `.dmg-flow-wrap` (scrollable), `.dmg-flow` (responsive), `.dmg-flow-branches` (opacity), `.dmg-flow-title-text` and `.dmg-flow-formula-text` (SVG text fonts/sizes), and per-node fill rules for attack/muted/crit/aff/abrasion. Inserted right before the `=== Intro page ===` block.
- `AGENTS.md`: appended one bullet under "Conventions" mandating the standard `Đang cập nhật…` warn-card markup for any new content-less page, with a pointer to existing placeholder pages as the reference markup.

## Open follow-ups

- Mobile layout for `.dmg-flow`: revisit if horizontal scroll on narrow phones feels janky. A vertical-stack alternative would require duplicating the SVG and a media-query swap.
- The note copy under the infographic intentionally repeats the variable definitions from inside the diagram. If the user wants a tighter version, drop the "Quy ước biến" line.
- Future Chỉ số sub-pages (e.g. defense stats, resistance) drop in as more `nav-child` buttons under `#nav-group-chiso` and a new `<section id="page-chiso-…">`. Per the new AGENTS.md rule, any such empty page must include the standard `Đang cập nhật…` warn-card.

## Cross-project lessons

None recorded — no bug pattern surfaced. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
