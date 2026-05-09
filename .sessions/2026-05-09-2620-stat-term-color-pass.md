---
date: 2026-05-09
slug: stat-term-color-pass
files_touched:
  - styles.css
  - index.html
bugs_recorded: []
---

## User request

Color every inline occurrence of "Chí mạng" / "chí mạng" with the crit-yellow stat-identity color, and every "Hiểu ý" / "hiểu ý" with the affinity-orange color — wherever they appear in body text across the site.

## Plan / decisions

- **Two new utility classes**: `.term-crit { color: var(--crit-bright); }` and `.term-aff { color: var(--aff-bright); }`. Defined at the **bottom** of styles.css (after the `.dmg-formula-*` block) so they win source-order ties against single-class component rules like `.dmg-l { color: var(--text-ivory); }`. Skipped `!important` and skipped element-attached selectors (`strong.term-crit`) because the wrap-with-`<span>` pattern below avoids the specificity collision entirely.
- **Wrap with `<span>`, never add class to existing element.** A nested `<span class="term-crit">…</span>` always has its own color rule applying directly to the span — no need to fight inheritance from `.compare-text strong`, `.skill-list strong`, etc.
- **Skipped three component contexts that already apply the correct color via parent class** — no changes needed:
  - `.tg-lbl` inside `.tg-crit` / `.tg-aff` (the stat tiles on the recommend page)
  - SVG `<text class="dmg-flow-title-text">` inside `.dmg-flow-crit` / `.dmg-flow-aff` (the damage-tree nodes — also can't take HTML `<span>` since they're SVG)
  - `.dmg-formula-name` inside `.dmg-formula-crit` / `.dmg-formula-aff` (the damage-formula tiles)
- **Wrap order matters for nested `<strong>` cases**: e.g. `<strong>P(Chí mạng)</strong>` becomes `<strong>P(<span class="term-crit">Chí mạng</span>)</strong>` — span sits *inside* the strong so the `P(` prefix and `)` suffix stay in the strong's default color.
- **Replace_all used aggressively where the pattern was unambiguous**: `<span class="dmg-l">Chí mạng</span>`, `<strong>chí mạng</strong>`, etc. Multi-term strongs (`<strong>chí mạng / hiểu ý</strong>`, `<strong>chí mạng &gt; Hiểu ý</strong>`) needed individual edits since the bulk replace_all on standalone strongs didn't catch them.

## Files changed

- `styles.css` — appended a small "Inline stat-term highlighters" comment block + two one-line classes at the very end of the file.
- `index.html` — wrapped 27 occurrences across the file:
  - 7 bulk replace_alls covered the standalone patterns: `<span class="dmg-l">{Chí mạng|chí mạng|Hiểu ý}</span>` and `<strong>{Chí mạng|chí mạng|Hiểu ý|hiểu ý}</strong>`.
  - 13 individual edits covered the irregular contexts: TOC subtitle (line 177), Tank/Healer h1 titles (220, 464), key-card label (227), recommend-page note (263), compare-text with two terms in one strong (297), damage comparison footer (300), three skill-list bullets where the strong wraps a longer phrase (381–383), Chỉ số tấn công hero sub (561), warn-card "chí mạng / hiểu ý" combined-strong (568), Sát thương đầu ra hero sub (727), the quy-ước-biến note (785–786), the damage-formula footer (814), and the `P(Chí mạng)` / `P(Hiểu ý)` formula entries (897–898).

## Open follow-ups

- **Capitalization is intentionally preserved**. The codebase mixes "chí mạng" (lowercase, when used as a noun in running prose) with "Chí mạng" (capitalized, when used as a label/heading). I didn't normalize because the user only asked about color. If they later want consistent capitalization, that's a separate pass.
- **No CSS rule for the four "untouched" contexts** changed. If a future session swaps them to a structure where the parent class no longer auto-colors the term, just add `<span class="term-crit">` / `<span class="term-aff">` like everywhere else.
- The "Crit" English variant in some prose (e.g. "Crit lấy trung bình roll" on line 300, "ngưỡng crit" on line 568) was deliberately left uncolored — user asked about Vietnamese terms specifically. If they want English variants colored too, repeat the pass with `crit` / `Crit` patterns.

## Cross-project lessons

None recorded — straightforward semantic-coloring pass; the "wrap with span vs. attach class" tradeoff is well-known territory. No new entries in `<HOME>/ai-optimizer/memory/bugs/`.
