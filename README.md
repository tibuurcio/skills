# skills

A small collection of [agent skills](https://skills.sh) by [@tibuurcio](https://github.com/tibuurcio). Compatible with Claude Code, Cursor, Cline, GitHub Copilot, and 18+ other AI coding agents via the [`skills`](https://github.com/vercel-labs/skills) CLI.

## Skills

| Skill | What it does |
|---|---|
| [`narrative-showcase`](./skills/narrative-showcase) | Build interactive single-file HTML pages that explain technical architecture, system designs, or data flows through clickable diagrams, animated step traces, and expandable detail panels. Warm dark editorial aesthetic with Agentation feedback toolbar baked in. |

---

## Install

### Via the skills CLI (recommended)

Install a specific skill from this repo:

```bash
npx skills add tibuurcio/skills --skill narrative-showcase
```

Or install all skills from this repo at once:

```bash
npx skills add tibuurcio/skills
```

The CLI auto-detects your coding agent (Claude Code, Cursor, Cline, etc.) and drops the skill into the right place.

### Manual install (Claude Code)

```bash
git clone https://github.com/tibuurcio/skills.git /tmp/tibuurcio-skills
cp -r /tmp/tibuurcio-skills/skills/narrative-showcase ~/.claude/skills/
```

After installation, invoke in any session with `/narrative-showcase` or just describe what you want — the skill auto-activates when it matches your request.

---

## narrative-showcase

Turns architecture docs, refactor writeups, and system walkthroughs into interactive single-file HTML pages. One `.html` file, no build step, no framework — just open it in a browser.

### What you get

- **Warm dark editorial theme** — `#0d0d0d` background with brown borders and cream text. Feels like a leather-bound book, not a terminal.
- **Component catalog** — 8 interactive section types: accordion cards, clickable diagrams, animated step traces, card catalogs, toggle comparisons, timelines, comparison tables, callouts.
- **Section-numbered navigation** — fixed side nav with fade-in on scroll via `IntersectionObserver`.
- **Integrated feedback toolbar** — every generated showcase ships with the [Agentation](https://www.agentation.com) toolbar loaded from `esm.sh`. Click any element, annotate it, copy the markdown, paste it back into your agent for revisions.

### Good for

- Architecture walkthroughs (data flows, request lifecycles, system refactors)
- Service/module documentation (interface contracts, catalog grids)
- Before/after migration stories
- Onboarding explainers
- Decision records with interactive diagrams

### Not good for

- Controls + preview playgrounds with prompt output — use a dedicated playground skill
- Simple markdown docs that don't need interactivity
- Live apps or frequently-changing content

### Included files

- `SKILL.md` — frontmatter, process, design system reference
- `template.html` — starting point with all CSS/JS embedded (~22KB)
- `sections.md` — catalog of 8 reusable section patterns with HTML + JS snippets

The skill is self-contained. No external references, no build step, no npm dependencies on the author side.

---

## Feedback toolbar

Every generated showcase includes the [Agentation](https://www.agentation.com) toolbar, loaded from `esm.sh` at runtime. When served over HTTP, a floating feedback toolbar appears — click elements, annotate them, and copy the feedback as structured markdown to paste back into your agent.

When opened via `file://`, the toolbar shows a hint to serve the file locally instead (browser CORS rules block CDN ESM imports from `file://`).

---

## Credits

The visual aesthetic of the `narrative-showcase` skill — warm dark theme, section numbering, clickable detail panels, animated step traces — is based on the interactive showcase work of [**@razakiau**](https://github.com/razakiau). Huge thanks for the design language.

---

## License

MIT — see [LICENSE](./LICENSE). Use, modify, and share freely.

## Contributing

Issues and PRs welcome. If you build a skill you'd like to add to this collection, open a PR with a new directory under `skills/`.
