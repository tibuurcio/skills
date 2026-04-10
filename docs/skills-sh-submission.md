# skills.sh Directory Submission

This document holds the ready-to-post submission for getting this repo listed on the [skills.sh](https://skills.sh) directory.

## When to submit

Submit **after** accumulating some install traction. The CLI reports anonymous install counts via telemetry, and skills.sh ranks its leaderboard by those counts. A brand-new submission with zero installs is unlikely to be accepted.

Rough heuristic: wait until the skills here have been shared publicly (social media, blog post, coworkers, dev communities) and you have reason to believe ~10+ people have installed via `npx skills add`. There are no published criteria — maintainers curate case-by-case.

## How to submit

1. Go to https://github.com/vercel-labs/skills/issues/new
2. Use the title and body below
3. Submit and wait for maintainer response

## Submission title

```
Add `tibuurcio/skills` to the skills directory
```

## Submission body

```markdown
Hi! I'd like to request that [`tibuurcio/skills`](https://github.com/tibuurcio/skills) be added to the skills.sh directory.

## Repository

https://github.com/tibuurcio/skills

## Install command

```bash
npx skills add tibuurcio/skills
```

## Skills included

### `narrative-showcase`

Builds interactive single-file HTML pages that explain technical architecture, system designs, refactors, or data flows through clickable diagrams, animated step traces, and expandable detail panels. Designed for architecture walkthroughs, onboarding explainers, before/after migration stories, and decision records.

**What makes it distinctive:**
- Warm dark editorial aesthetic (feels like a leather-bound book, not a terminal)
- 8 reusable section patterns: accordion cards, clickable diagrams, animated traces, card catalogs, timelines, comparison tables, callouts
- Fully self-contained — template + patterns embedded in the skill, zero external references for the agent to fetch
- Ships with the [Agentation](https://www.agentation.com) feedback toolbar loaded via `esm.sh` CDN, so users can annotate any element of the generated showcase and copy the feedback back to their agent for revisions

**Install:**
```bash
npx skills add tibuurcio/skills --skill narrative-showcase
```

## Compatibility

Works with Claude Code, Cursor, Cline, and any other agent the `skills` CLI supports.

## License

MIT

Thanks for maintaining the CLI and directory!
```

## Notes for later

- If more skills are added before submitting, append a new section under "Skills included" with the same template
- Update the "What makes it distinctive" bullets based on real feedback from early users
- Consider adding a hosted example URL (GitHub Pages) of a generated showcase to the submission — a visual demo is far more compelling than text
- Check recent submission issues on [`vercel-labs/skills`](https://github.com/vercel-labs/skills/issues?q=is%3Aissue+add+skills) before posting to match the current tone and format the maintainers expect

## References

- [Prior submission example #167](https://github.com/vercel-labs/skills/issues/167)
- [Prior submission example #261](https://github.com/vercel-labs/skills/issues/261)
- [Discussion on listing criteria #803](https://github.com/vercel-labs/skills/issues/803)
