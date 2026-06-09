# Design brief — MIRA inter-lab mockups

> Read with `_grounding.md`. Every mockup is a **single self-contained `.html` file** in
> `artifacts/mockups/` that links the two shared stylesheets and adds only screen-specific CSS.
> Built per Claude's **frontend-design** skill: distinctive, production-grade, never generic AI slop.

## Non-negotiables

1. **Link the shared system, don't reinvent it:**
   ```html
   <link rel="stylesheet" href="tokens.css">
   <link rel="stylesheet" href="components.css">
   ```
   Reuse `.node`, `.ntype`, `.btn`, `.pointer`, `.edge`, `.panel`, `.toast`, `.avatar`,
   `.steps`, `.share-level`, `.screen`, `.winbar`, `.reveal`, etc. Add a `<style>` block only
   for layout/composition unique to the screen. Do **not** import other fonts or restate the palette.
2. **Node-type colours are fixed** (Question gold · Claim green · Evidence coral · Study blue ·
   Protocol violet · Request indigo · Source teal) — apply via the component classes.
3. **Use the worked stress-granule example verbatim** from `_grounding.md` §3. Label any plot
   "illustrative · unpublished — workshop prototype." Never invent contradicting science.
4. **Honour the rules visually** — pointers not payloads (R2: show git/S3/local/video chips,
   never a raw-data download); summary-up-front-then-traverse (R4); privacy & progressive
   disclosure (R5: the un-shared "Sean's thoughts" node appears greyed/dashed or as a redacted
   "1 related node not shared" line — never leak its content).
5. **Caption every screen.** Top of `<body>`, before the screen, include a `.frame-caption`
   with an `.eyebrow` (e.g. `MOCKUP 02 · EVIDENCE NODE`), an `<h2>` title, and one sentence on
   what the screen shows and which rule/insight it embodies.
6. **Animate the page load** with staggered `.reveal .d1…d8` and at least one signature motion
   moment appropriate to the screen (edge being drawn, traversal expanding, toast sliding in,
   request being claimed). CSS-only. Respect `prefers-reduced-motion` (already handled in CSS).
7. **No external JS frameworks.** Optional tiny inline vanilla JS for one interaction is fine
   but the screen must look complete and correct with JS disabled (static-first).
8. **Self-contained & openable** — must render correctly by double-clicking the file (relative
   CSS hrefs, no build step, no network beyond Google Fonts). Width: center the `.screen`/content,
   target ~1080px max; fully responsive down to ~720px is a plus, not required.

## Aesthetic direction (commit to it)

Warm-paper **lab-notebook meets modern knowledge-graph tool**. Minimal-scientific, confident,
editorial. Generous negative space, a faint dotted grid (already on `body`), soft layered
shadows, hairline rules. Fraunces for headings (characterful serif), Hanken Grotesk for UI,
JetBrains Mono for anything machine-facing (pointers, JSON-LD, edge labels, RIDs). The feeling:
*"a result, with just enough of its lineage, moving cleanly from one lab's graph into another's."*

Two chrome modes:
- **In-app (Obsidian)** screens use `.theme-obsidian` on a wrapper to get the dark editor pane,
  with a faux left ribbon / file list and an editor surface; the DG nodes sit inside.
- **Web / standalone** screens use the light paper theme and the `.winbar` with an address bar.

## Cross-screen consistency

- Same people, same colours: Sean = blue avatar, Kate = green, Anton = amber, "Kate's Lab" = lab/violet.
- Same node titles/wording across screens (copy from `_grounding.md` §3) so the five mockups
  read as one continuous story.
- Edge labels use the exact relation names: `supports`, `grounds`, `follows`, `addresses`,
  `request_target`, `request_for`.
- Footer line on each screen (tiny, muted): `MIRA × Discourse Graphs · inter-lab exchange · draft v0.1`.

## What "great" looks like

Not a wireframe — a screen you could hand to an engineer and a user. Real copy, real data
pointers, real permission states, real typography hierarchy, purposeful colour, one memorable
motion moment. Restraint and precision over decoration.
