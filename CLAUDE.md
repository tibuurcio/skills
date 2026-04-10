# CLAUDE.md

Guidance for AI coding agents (and humans) working on this repo.

## What this repo is

A public collection of agent skills installable via the [`skills`](https://github.com/vercel-labs/skills) CLI. Compatible with **Claude Code, Cursor, Cline, GitHub Copilot, OpenCode, Codex**, and 40+ other AI coding agents.

Anyone can install any skill here with one command:

```bash
npx skills add tibuurcio/skills --skill <skill-name>
```

The CLI clones this repo, parses `SKILL.md` files, and either symlinks or copies the skill into the user's agent skill directory (e.g. `~/.claude/skills/` for Claude Code).

## Repo structure

```
skills/
├── README.md                 # User-facing catalog & install docs
├── CLAUDE.md                 # This file — contributor/agent guide
├── LICENSE                   # MIT
├── .gitignore
└── skills/                   # All skills live here
    └── <skill-name>/         # One directory per skill
        ├── SKILL.md          # Required: frontmatter + docs
        └── ...               # Supporting files (templates, assets, etc.)
```

The `skills/` subdirectory is the canonical location the CLI scans for `SKILL.md` files. Other supported locations (for reference): repo root, `.claude/skills/`, `.agents/skills/`, `skills/.curated/`, `skills/.experimental/`.

## Anatomy of a SKILL.md

Every skill needs a `SKILL.md` with YAML frontmatter at the top:

```markdown
---
name: skill-name
description: Use when [situation] — a one-line trigger phrase the agent uses to decide whether to invoke this skill.
---

# Skill Name

Full documentation for the skill — what it does, when to use it, how the agent should follow it.
```

**Frontmatter rules:**
- `name` — lowercase, kebab-case, must match the directory name
- `description` — starts with *"Use when..."* so agents can pattern-match user intent. Keep it under 200 chars. This is what's shown in skill listings and what drives auto-activation in Claude Code.

**Body conventions:**
- Start with a short summary of what the skill does
- Include a **When to Use** section (bullet list of use cases)
- Include a **Not for** section (what to use instead) — helps agents rule it out
- Reference supporting files by relative path (e.g. `template.html`, `sections.md`)
- Keep it self-contained — no external URLs the agent needs to fetch to function

## Adding a new skill

1. **Create the directory**
   ```bash
   mkdir -p skills/<skill-name>
   ```

2. **Write `skills/<skill-name>/SKILL.md`** with frontmatter and docs. Use an existing skill (like `narrative-showcase`) as a reference for structure and tone.

3. **Add any supporting files** the skill needs — templates, example data, catalogs, scripts. Keep everything inside the skill's own directory.

4. **Test it locally** before pushing (see below).

5. **Update `README.md`** — add a row to the Skills table and a full section describing the skill if it's substantial.

6. **Commit and push**
   ```bash
   git add skills/<skill-name> README.md
   git commit -m "Add <skill-name> skill"
   git push
   ```

That's it. As soon as the commit is on `main`, anyone can install with `npx skills add tibuurcio/skills --skill <skill-name>`.

## Testing a skill locally

Three ways to verify a skill before publishing:

**Option 1 — Install from local path (recommended)**
```bash
npx skills add /Users/gt/dev/skills --skill <skill-name>
```
The `skills` CLI supports local directory paths as a source. Fastest iteration loop.

**Option 2 — Symlink directly into Claude Code's skill dir**
```bash
ln -s /Users/gt/dev/skills/skills/<skill-name> ~/.claude/skills/<skill-name>
```
Any edit you make in the repo takes effect immediately — no reinstall needed.

**Option 3 — List without installing**
```bash
npx skills add /Users/gt/dev/skills --list
```
Dry-run that shows what skills the CLI can discover in the repo. Useful to verify your `SKILL.md` frontmatter parses correctly.

After installing, open a new Claude Code session and either:
- Invoke explicitly: `/<skill-name>`
- Or describe what you want naturally — Claude will auto-activate the skill if your request matches the `description` frontmatter

## Publishing updates

There's no release process or versioning. Just commit to `main` and push:

```bash
git commit -am "Update narrative-showcase: fix X"
git push
```

Users who run `npx skills check` will see an update available. Users running `npx skills add tibuurcio/skills` fresh will always get the latest commit on `main`.

If you want optional versioning, you can tag releases:

```bash
git tag v1.1.0 && git push --tags
```

Users can then pin to a specific version by passing a git URL with a ref.

## Getting listed on skills.sh

Listing on [skills.sh](https://skills.sh) is **not required** for the skill to work — the CLI installs any public GitHub repo with `SKILL.md` files, regardless of directory listing. Listing is purely about discoverability.

The listing process is:

1. **Your skill needs to be installable** ✅ (it already is)
2. **Accumulate some install telemetry** — the CLI reports anonymous install counts. The skills.sh leaderboard ranks by install count. Share the repo with friends, coworkers, Twitter, HN, etc.
3. **Open an issue** on [`vercel-labs/skills`](https://github.com/vercel-labs/skills/issues) titled something like:
   > "Add `tibuurcio/skills` to the skills directory"
4. **Include in the issue body**: link to the repo, 1-line description of each skill, why it's useful, any install/usage stats

Prior examples: issues [#167](https://github.com/vercel-labs/skills/issues/167), [#261](https://github.com/vercel-labs/skills/issues/261) — follow their pattern.

There are no published criteria for acceptance. Maintainers curate case-by-case.

**A pre-drafted submission is ready to post** at [`docs/skills-sh-submission.md`](./docs/skills-sh-submission.md). When you're ready to submit, update it with any new skills added since, then copy-paste the title and body into a new issue on `vercel-labs/skills`.

## Design philosophy for skills in this repo

1. **Self-contained** — a skill should not depend on external URLs, npm packages on the user side, or services that might disappear. Embed templates, catalogs, and examples directly in the skill directory.

2. **Agent-agnostic where possible** — skills should describe behavior in terms any AI coding agent can follow. Avoid Claude Code-specific terminology in `SKILL.md` unless the skill genuinely requires a Claude feature (e.g. specific MCP tools).

3. **Prefer embedded knowledge over instructions** — rather than telling the agent "search the web for X", embed the X directly. Skills are packaged capabilities, not prompts.

4. **Credit prior art** — if a skill's design or aesthetic is based on someone else's work, credit them in the `SKILL.md` or repo `README.md`.

5. **Keep SKILL.md terse at the top, detailed below** — the first few lines matter most because agents scan them to decide whether to invoke. Put the "when to use" signal early.

## Conventions

- **Skill names**: kebab-case, matches directory name, matches frontmatter `name`
- **File extensions**: `.md` for docs, `.html`/`.js`/`.css` for web templates, any format for supporting data
- **No secrets**: this is a public repo. Never commit API keys, tokens, or internal URLs
- **No project-specific references**: skills should be generic and reusable. If you wrote a skill for internal company use, strip the private bits before adding it here
- **License**: everything in this repo is MIT (see `LICENSE`). By contributing, you agree to publish your contribution under MIT

## Useful commands

```bash
# List installed skills across all your agents
npx skills list

# Search for skills on skills.sh
npx skills find <query>

# Check for updates
npx skills check

# Update all installed skills to latest
npx skills update

# Remove a skill
npx skills remove <skill-name>
```

## References

- [`skills` CLI repo](https://github.com/vercel-labs/skills) — source of truth for CLI behavior
- [`skills` CLI AGENTS.md](https://github.com/vercel-labs/skills/blob/main/AGENTS.md) — CLI internals if you need to debug discovery
- [skills.sh directory](https://skills.sh) — public leaderboard of skills
- [Anthropic skills](https://github.com/anthropics/skills) — reference skills from Anthropic
- [Vercel skills docs](https://vercel.com/docs/agent-resources/skills) — conceptual overview of agent skills
