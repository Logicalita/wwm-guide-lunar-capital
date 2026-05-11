---
date: 2026-05-11
slug: cache-bust-styles
files_touched:
  - index.html
bugs_recorded:
  - id: stale-css-cache-after-css-additions
    summary: Browser served cached pre-update styles.css after a deploy that added new CSS rules + matching HTML classes — new classes had no rules to apply, producing unstyled fallback layout. Fixed by adding a `?v=YYYYMMDD` query param to the stylesheet link.
---

## User request

After pushing the chiso pages work to main, the user opened the live site and reported that the "40,4 điểm thuộc tính vs 1 stat max-roll" card looked completely unstyled — content stacked vertically with no grid layout, no verdict tag chrome, labels in body case instead of uppercase. Asked me to diagnose.

## Plan / decisions

- **Initial hypothesis**: maybe a CSS syntax error broke parsing.
  - Counted braces (297 open / 297 close) and `/* */` comment markers (24/24) — file is balanced.
  - Verified `.attr-cmp-*` rules exist at HEAD: `git show HEAD:styles.css | grep attr-cmp` returned all 30+ expected rules.
- **Confirmed hypothesis**: browser cache.
  - The pushed styles.css contains the new rules.
  - The screenshot shows behavior consistent with the new HTML classes existing but the new CSS rules being absent — exactly what happens if the browser loaded HTML fresh from the push but served a cached old styles.css from before the rules existed.
  - All three new pattern families (`.attr-cmp-*` comparison, `.attr-conv-*` per-point, `.attr-rec-*` recommendation) would be affected the same way.
- **Fix**: Add a cache-busting query param to the stylesheet link: `styles.css` → `styles.css?v=20260511`. Browsers treat the URL as new and re-fetch.
- **Recurrence prevention**: Document the pattern — for plain HTML/CSS projects without a build pipeline, every CSS change that adds NEW rules referenced by new HTML classes should bump the `?v=` number to invalidate caches. (CSS changes that only modify existing rules are less critical because the cached file is still partially functional; new-rule additions break cleanly.)

## Files changed

- `index.html` line 11: `<link rel="stylesheet" href="styles.css">` → `<link rel="stylesheet" href="styles.css?v=20260511">`.

## Open follow-ups

- **Versioning policy**: Future CSS changes should bump `v=YYYYMMDD`. Manual process; could automate via a tiny shell script that touches the version on every CSS commit, but probably overkill for this site's pace.
- **Tabler icons + Google Fonts links** also lack versioning, but those are CDN URLs (versioned externally) so probably fine.

## Cross-project lessons

- **Plain HTML/CSS sites need explicit cache-busting on CSS link tags**: Browsers and CDNs cache `styles.css` aggressively. Adding new CSS rules referenced by new HTML classes produces a confusing failure mode — HTML loads fresh and visibly broken because the cached CSS doesn't know about the new classes. Diagnostic flag: the page is partially styled (everything that existed before) but specifically the new content has no styling.
- **The git-show diagnostic step is high-value**: `git show HEAD:<file>` instantly distinguishes "the rules are in the repo but not loading" from "the rules aren't in the repo at all." Saved a lot of investigation on this one — once I confirmed the CSS was pushed, the only remaining explanation was cache.
- **Symptom signature for cache miss**: divs/spans rendering as default block/inline (everything stacked, no grids, no flex columns, no `text-transform: uppercase` on labels) while OUTER chrome (card border, page header, color theme) renders correctly. The "outside is fine, inside is naked" pattern strongly implies cached CSS missing newer descendant rules.
