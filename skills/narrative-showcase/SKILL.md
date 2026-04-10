---
name: narrative-showcase
description: Use when building interactive single-file HTML pages that explain technical architecture, system designs, refactors, or any topic through a narrative walkthrough with clickable diagrams, animated traces, and expandable detail panels
---

# Narrative Showcase

Build interactive single-file HTML showcase pages with a warm dark editorial aesthetic. Clickable components, animated data flows, expandable detail panels, section-numbered navigation.

The page tells a story. Each section builds on the previous one. Interactive elements let readers explore depth without losing the narrative thread.

The complete design system is embedded in this skill — `template.html` has all CSS/JS, `sections.md` has all component patterns. No external references needed.

The template includes the **Agentation** feedback toolbar, loaded via `esm.sh` CDN. When you open a showcase in the browser, you can click elements, annotate them, and copy structured feedback as markdown — ready to paste back into Claude Code for revisions.

## When to Use

- Architecture walkthroughs (system design, refactors, data flows)
- Service/module documentation (catalogs, interface contracts)
- Before/after comparisons (migrations, redesigns)
- Onboarding explainers (how a system works end-to-end)
- Decision records (why we chose X over Y, with interactive diagrams)
- Product feature showcases

**Not for:** controls+preview playgrounds with prompt output (use a dedicated playground skill — e.g. `anthropics/skills/playground` on Claude Code), simple markdown docs, live apps, frequently-changing content.

## Process

1. Identify the topic and audience
2. Outline the narrative arc (4-7 sections)
3. Pick section types from `sections.md` for each section
4. Copy `template.html` as starting point
5. Replace hero placeholders (title, subtitle, stats)
6. Adjust section count — add/remove `<section>` shells and nav dots
7. Fill each section with content using patterns from `sections.md`
8. Append section-specific JS before `</script>`
9. Open in browser to verify
10. Publish (project docs, GitHub Pages, knowledge base)

**Key rules:**
- Always start from `template.html` — never write CSS from scratch
- Pick section types from `sections.md` — don't invent new patterns unless the catalog genuinely doesn't cover the need
- Hero stats: 3-4 concrete numbers summarizing the story
- Section count: 4-7 is the sweet spot
- Code snippets: curated 5-20 line extracts, not full files
- Syntax highlighting via CSS classes: `.kw` (keywords), `.fn` (functions), `.str` (strings), `.comment` (comments), `.type` (types), `.param` (parameters), `.highlight` (emphasis)

## Narrative Arc

- **Open with context** — what and why (1 section)
- **Introduce concepts** — patterns, interfaces, building blocks (1-2 sections)
- **Show the transformation** — before/after, data flows, step traces (1-2 sections)
- **Catalog the pieces** — grids, lists, inventories (1 section)
- **Deep dive** — the most complex/interesting part (1 section)

## Design Language

### Colors

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#0d0d0d` | Page background |
| `--surface` | `#1a1a1a` | Cards, panels, nav |
| `--text` | `#e8e4df` | Primary text (warm cream) |
| `--text-muted` | `#8a8580` | Secondary text |
| `--text-dark` | `#5a5550` | Tertiary/inactive |
| `--accent` | `#d4a853` | Gold — active states, CTAs |
| `--code` | `#7b9eb8` | Inline code (steel blue) |
| `--border` | `#2a2520` | Dividers (warm brown) |

### Category Colors

For color-coding components by type. Apply at opacity levels: `{hex}0a` (bg), `{hex}15` (badge), `{hex}20` (dividers), `{hex}25` (borders), `{hex}` (text).

| Color | Hex | Typical use |
|-------|-----|-------------|
| Terra cotta | `#c17b5e` | Problems, old/removed |
| Gold | `#d4a853` | New/featured, accents |
| Forest green | `#6ba368` | Workflows, success |
| Steel blue | `#7b9eb8` | Existing services, code |
| Lavender | `#b8a9c9` | Side effects, secondary |
| Teal | `#9bbec7` | Infrastructure, bridges |

### Typography

| Role | Font | Fallback |
|------|------|----------|
| Headings / UI | Space Grotesk | system-ui, sans-serif |
| Body text | Source Serif 4 | Georgia, serif |
| Code | JetBrains Mono | monospace |

### Aesthetic Principles

- **Warm dark theme** — `#0d0d0d` bg with `#2a2520` brown borders and `#e8e4df` cream text. Feels like a leather-bound book, not a terminal.
- **Noise overlay** — SVG feTurbulence at 3% opacity for paper-like grain
- **Hero grid** — Gold lines (60px, 15% opacity) drifting diagonally over 20s
- **Section fade-in** — opacity + translateY on scroll via IntersectionObserver
- **Glow-on-select** — `box-shadow: 0 0 12px rgba(color, 0.08)`
- **Detail panels, not modals** — in-page panels below content, not overlays
- **Section numbering** — `01` + horizontal rule + serif heading
