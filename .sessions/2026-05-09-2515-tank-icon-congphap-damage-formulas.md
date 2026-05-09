---
date: 2026-05-09
slug: tank-icon-congphap-damage-formulas
files_touched:
  - index.html
  - styles.css
bugs_recorded: []
---

## User request

Three pieces of work bundled in one go:
1. Replace the generic `ti-shield-check` Tank icon with the new `Stonesplit-might.avif` image at the parent nav + intro TOC.
2. Add real Công pháp content for Tank: two martial-art cards (Bát Phong Lôi Thương + Than Sinh Đao Pháp) with weapon images, plus a third "Tổng kết hướng build PVE" summary.
3. Add a damage-calculation card to the Sát thương đầu ra page covering the four outcome formulas (Crit / Affinity / Normal / Abrasion).

## Plan / decisions

- **Tank icon scope** confirmed via AskUserQuestion: parent dropdown toggle + intro TOC entry only (2 spots). Sub-page icons (target / book-2 / flame / sword) stay as tabler glyphs because they represent page TYPES, not class identity.
- **No CSS color filter on the avif** — letting the weapon image render in its native palette. Tinting it gold would muddy the detail. The neighbouring text styles (active/hover) already work since they target the parent button, not the icon.
- **Two-class image sizing** via `.nav-icon-img` (22px in sidebar) with a `.toc-item .nav-icon-img` override (28px). Matches the existing tabler icon visual sizes in each context.
- **Martial-card layout**: 180px image on the left, content (skills + passives) on the right via CSS grid. On phones (≤480px), stacks vertically with the image centered at max-width 220px so it doesn't dominate the narrow viewport. Image wraps in a bordered container so the avif's natural rectangular shape feels intentional.
- **Skill block visual hierarchy**: each skill (active 1, active 2, kỹ năng tích lực, bị động) is a `.skill-block` separated by dashed dividers. Skill name uses a small rotated-square gold marker (mirrors the diamond `◆` used in the bị động bullet list, just rotated and filled). Bị động list uses gold diamonds in front of each item.
- **Card 2 lacks "Kỹ năng tích lực" intentionally** — user data only provided active1, active2, and bị động for Than Sinh Đao Pháp. Not invented one.
- **Card 3 (Tổng kết PVE)** reuses the existing `.pri` priority-list component verbatim — same row pattern as the recommend-page "Ưu tiên chỉ số trang bị" card. Numbered chips, ivory text, optional `<strong>` highlights for stat names.
- **Damage-formula card placement**: between the existing tree card and the effective-values warn-card on `#page-chiso-damage`. Reasoning: tree explains *which* branches and *probability*; the new card explains *damage magnitude per branch*; the warn applies to the probability content below it; examples then combine probability + damage. Order reads top-down naturally.
- **2x2 grid for damage formulas** instead of the source image's 4-across row — fits the site's 760px container much better and stacks cleanly to 1-up on phones.
- **Stat-identity colors reused** — `.dmg-formula-crit` gets the yellow `--crit*` family, `.dmg-formula-aff` gets the orange `--aff*` family, established two sessions ago. Normal = silk (default), Abrasion = bone with slight opacity reduction. Matches both the source image's color coding and the rest of the site.
- **Footer note on the damage-formula card** ties it back to the existing case-card insight: "Sát thương yếu chốt ở min — đó là lý do dòng *min thuộc tính* được ưu tiên (nâng đáy = nâng cả abrasion lẫn normal/crit)" — connects this new card to the case-cards content on the chỉ số tấn công page.

## Files changed

- `index.html`:
  1. Sidebar `#nav-group-tank` parent toggle — `<i class="ti ti-shield-check">` → `<img src="images/tank/Stonesplit-might.avif" alt="Stonesplit-Might" class="nav-icon-img">`.
  2. Intro TOC button with `data-goto="tank-chiso"` — same image, with both `toc-icon` and `nav-icon-img` classes (additive so the existing TOC layout still applies).
  3. `#page-tank-congphap` — replaced the single `.warn-card` placeholder with three new cards: Bát Phong Lôi Thương (Stormbreaker-spear.avif + 4 skill blocks), Than Sinh Đao Pháp (Thundercry-blade.avif + 3 skill blocks), and Tổng kết hướng build PVE (paragraph + 3-row priority list).
  4. `#page-chiso-damage` — inserted a new `.card` containing the 4-tile `.dmg-formula-grid` between the tree card and the effective-values warn-card.
- `styles.css`:
  1. Appended `.nav-icon-img` rule with TOC override.
  2. Appended a `=== Martial-art cards ===` block: `.martial-card`, `.martial-image-wrap`, `.martial-image`, `.skill-block`, `.skill-name`, `.skill-list` and their interior styles.
  3. Appended a `=== Damage formula grid ===` block: `.dmg-formula-grid`, `.dmg-formula`, `.dmg-formula-name`, `.dmg-formula-eq`, plus the four color-variant classes.
  4. Extended the existing `@media (max-width: 480px)` block with three lines: martial-card stacks 1-column, image wrap centers + caps width, dmg-formula grid stacks 1-up.

## Open follow-ups

- **Real-device check on the avif sizing** — DevTools doesn't always render avif at the right intrinsic ratio. Worth verifying the Tank icon at 22px on iOS Safari and Android Chrome.
- **Skill copy refinement** — the user's source text used colons in some places, em-dashes in others. I normalized to em-dashes for skill names ("Chủ động 1 — Phong Lôi Khiếu") for visual consistency. If the user wants the literal "Chủ động 1 -" form, swap back.
- **Ưng Dương Hổ Thị description** has nested conditionals (shield → buff → re-buff loop). The current paragraph form may be hard to scan; could break into a numbered sub-list later if the user finds it dense.
- **Damage formula footer** says "min thuộc tính được ưu tiên" — connects this card to the case-cards page. If a future user reads this page in isolation, the reference may feel orphan. Consider adding a small inline link to the chỉ số tấn công page if a navigation system for that develops.

## Cross-project lessons

None recorded — all project-specific copy + design. The class-icon-image pattern (avif as nav icon) is generic enough that it could be useful elsewhere, but it's a one-CSS-rule pattern, not a portable bug lesson. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
