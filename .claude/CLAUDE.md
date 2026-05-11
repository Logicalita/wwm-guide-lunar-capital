# wwm-guide-lunar-capital — Claude Code

ROOT = ~/Library/CloudStorage/OneDrive-Personal/ai-optimizer/

## Session start

1. Read `ROOT/rules/user-preferences.md` and `ROOT/rules/global.md`
2. Read `AGENTS.md` in this project (stack conventions + game terminology)
3. Query `ROOT/memory/index.json` with task keywords; load matched bug entries
4. Skim last 5 lines of `ROOT/memory/experiences/log.jsonl`
5. Read `ROOT/projects/wwm-guide-lunar-capital.md` for current project context
6. Read the **last 3 files** in `.sessions/` (sorted by filename desc) — understand what was built recently before touching any code

## Stack

Vanilla HTML + CSS + inline JS — no build tools, no framework, no package.json.
Dev preview: open `index.html` directly in browser (no server needed).
Rule file: `ROOT/rules/global.md` (no language-specific rule file for plain HTML/CSS).

## Key constraints (abridged from AGENTS.md — AGENTS.md is authoritative)

- All user-facing copy stays in **Vietnamese**; do not auto-translate.
- Reuse `:root` CSS variables from `styles.css` — never hardcode hex colours.
- Stat identity colours: crit → `--crit*` (yellow), affinity → `--aff*` (orange), precision → `tg-prc` palette (gold/cream).
- `--cinnabar` (red) and `--jade` (green) are **reserved for rank quality** only — do not use for stats.
- New pages without real content: use `.warn-card` placeholder (see AGENTS.md for exact markup).
- In-game numbers (56%, 24%, etc.) are content data — do not modify without user confirmation.

## Session end

1. Write a session file to `.sessions/` — filename: `{YYYY-MM-DD}-{HHMM}-{slug}.md`; format matches existing files (frontmatter + User request / Plan/decisions / Files changed / Open follow-ups / Cross-project lessons)
2. Append one JSON line to `ROOT/memory/experiences/log.jsonl`
3. Update `ROOT/projects/wwm-guide-lunar-capital.md` if architecture or known issues changed
4. Increment `ROOT/state.json → sessionCount`
5. If `sessionCount % 5 === 0`, run `ROOT/reflections/_reflection_prompt.md`
