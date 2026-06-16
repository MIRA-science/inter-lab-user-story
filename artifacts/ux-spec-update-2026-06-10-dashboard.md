# UX spec update — the Sean → Kate dashboard, after Kate's live walkthrough

> 🗂️ **Historical delta — folded into the current spec.** The single, up-to-date specification is
> **[`ux-spec.md`](./ux-spec.md)**; build and maintain the mockups from it. This dated update is kept only
> as the record of how these decisions were reached — and some of it has since been **superseded** (e.g.
> the PI nudge in §4 was reversed; see the changelog in [`ux-spec.md §11`](./ux-spec.md#11-changelog--provenance)). *(Consolidated 2026-06-15.)*

### Developer-handoff delta · **draft v0.1 → v0.2** (+ design-review corrections, Jun 11 — see §12) · source: *Cell imaging project update — UX feedback* (Jun 10, 2026)

> **What this is.** A focused, buildable **update** to [`ux-user-story-dashboard.md`](./ux-user-story-dashboard.md)
> (the narrative spec, v0.1) and the 06–09 mockups, derived from the first session where the **real PI
> (Kate)** drove the mockups herself instead of role-playing. Transcript + merged notes:
> [`../../cell-imaging-project-update-ux-feedback-updated.md`](../../cell-imaging-project-update-ux-feedback-updated.md).
> Speaker map: **Kate** = the consumer PI (Speaker C), **Sean** = the producer (Speaker B), **Speaker A** = facilitator.
>
> **Altitude.** v0.1 is a *user story*. This doc is the *component-level handoff* the story didn't carry —
> measurements, tokens, states, edge cases — for the **eight things the Jun 10 session changed or added.**
> It reuses the existing design system verbatim ([`tokens.css`](./mockups/tokens.css),
> [`components.css`](./mockups/components.css)); every new atom below is specified as an **extension block**,
> not a reinvention. Rules referenced as **R2** (pointers-not-payloads), **R4** (summary-first-then-traverse),
> **R5** (permissions-without-leaks), **R6** (addressable URL), **R8** (provenance/trust) follow v0.1 / AGENTS.md.

---

## 0. What changed at a glance

| # | Area | Change | Status | Anchoring quote (Jun 10) | Affects |
|---|------|--------|--------|--------------------------|---------|
| 1 | **Evidence card** | Method + experimental context **embed inline** in the Evidence card (collapsed by default), not as separate sibling nodes you must hop to. Evidence/result is read **first**. | **CHANGED** (was: traverse to separate `Study`/`Protocol` nodes) | *"Maybe they can be embedded in the evidence because I would never look at / think about them separately."* | 07, 08; spec §3 C, §4 |
| 2 | **Comments & request-back** | A **comment thread** on a result, **kept separate** from the result/method body; any comment can be **"Convert to request"** or **"push to next steps."** | **NEW** | *"If there's a thread, maybe there's a button I can press — push this to request or push this to next steps."* | 08; spec §2.4, §3 D |
| 3 | **Candidate → Evidence** | Hover a **candidate** → **"Convert to evidence"**; the full **troubleshooting history is preserved** under the promoted node. | **NEW** (v0.1 §8 had this as an *open question*) | *"The candidate will become evidence and then there'll be history of all the troubleshooting."* | 06, 07; spec §8 |
| 4 | **Kanban status-drag + prioritization** | **Drag a card between columns** to set study **status**; the **PI can reorder** a student's backlog by priority; a **stale-experiment nudge** ("no result in a while"). | **NEW** | *"I see there's no result here for a while — I'm going to move it up or down… reorder things based on priority."* | 07, 09; spec §3 E |
| 5 | **Related-work newsfeed** | A per-user **newsfeed of related work** triggered by **overlapping keywords/tags** (cell line · protein · method). | **NEW** | *"A little newsfeed of related… keywords curate the feed. That would be amazing."* | 07, 09 (new tile); spec §3 E |
| 6 | **Accessibility controls** | A **serif-font toggle** and a **spacing/line-height** control, surfaced in the dashboard chrome. | **NEW** (extends colorblind/legibility reqs) | *"For accessibility, being able to change the font… so it's all serif. And somewhat the spacing."* | 07, 08, 09; grounding §2.3 |
| 7 | **Share mechanics** | **Right-click → "Share with [group / person]"**, **ctrl-click multi-select**, **Groups** as first-class targets, and a **"feels-robust" verification preview** before any share. | **CHANGED/expanded** | *"A really big verification step — you are about to share; this here is exactly what you're sharing."* | 06; spec §3 A |
| 8 | **Publish model** | Private vs. public is a **local-server-vs-hosted** distinction, **not a password**: each user's dashboard is a **local server** (`localhost:7979`); going public is an **intentional hosting** act. | **CHANGED** (refines "password-protected") | *"Spin a local server… to make it public-facing you explicitly host it… that's what differentiates public and private."* | 06, 09; grounding §4 |

> Two things the session **confirmed as-built** (no change needed): the **Graph · Kanban · Table** view switch
> and **audience presets** ([`.viewswitch`](./mockups/components.css) / [`.preset`](./mockups/components.css)) —
> *"super cool… relaxing to look at"* — and the **per-study progress meter** already in
> [`09-consortium-view.html`](./mockups/09-consortium-view.html), which Kate singled out (*"I like this
> progression bar a lot"*). One **caveat surfaced**: the **Table view is currently a facade** (*"there's no
> table yet"*) — it must become real and **equivalent to the Graph view** before this set is acceptance-complete.

> **Design-review pass (Jun 11).** A render + WCAG audit of the **built** 06–10 mockups (distinct from the Jun 10
> *content* feedback above) surfaced **two build-blocking defects** and a **contrast gap** the v0.2 component specs
> don't yet account for. Headline: **(1)** 07's Evidence card is **clipped off the right edge** of the dashboard
> frame — the result Kate is meant to glance at and trust; **(2)** 08's comment thread **leaks raw handler code**
> as visible text; **(3)** `--faint` tertiary text and the amber/teal on-wash chips **fail WCAG AA**, contradicting
> the "legible at 60+ · colorblind-safe" stamp every screen prints. Folded into §1 (tokens), §6 (a11y), §9
> (acceptance); tracked in full with line anchors and fixes in **§12**.

---

## 1. Design language — reused tokens & new atoms

No new palette, no new fonts. Everything below composes the existing kit.

### Tokens used by this update

| Token | Value | Used for |
|---|---|---|
| `--evidence-ink` / `--evidence-wash` / `--evidence-edge` | `#d9573f` / `#fbe0da` / `#f0b1a3` | Evidence card accent (§2), result-first emphasis |
| `--protocol-ink` / `--protocol-wash` | `#8460c5` / `#ece2f8` | Embedded **method** disclosure header (§2) |
| `--request-ink` / `--request-wash` / `--request-edge` | `#4854bf` / `#e2e3f7` / `#aeb4eb` | Comment→request, the `.request` chip (§3) |
| `--study-ink` / `--question-ink` | `#3877cf` / `#c2861a` | Kanban status columns, per-question grouping (§4, §8) |
| `--warn` / `--danger` | `#c2861a` / `#d9573f` | Stale-experiment nudge (§4) |
| `--brand` / `--brand-tint` | `#2f3a8c` / `#eceefb` | Convert/primary actions, active a11y toggles |
| `--r-sm…--r-pill` | `6 / 10 / 16 / 22 / 999px` | Card radii, chips, toggles |
| `--gap` | `16px` | Base grid gutter (becomes `20px` in spacious mode, §6) |
| `--ease` / `--ease-out` | `cubic-bezier(.2,.7,.2,1)` / `(.16,1,.3,1)` | Drag, disclosure, convert motion |
| `--shadow-1…3` | layered | Card rest / hover / drag-lift elevation |
| `--font-body` / `--font-display` / `--font-mono` | Hanken Grotesk / Fraunces / JetBrains Mono | Body; serif a11y mode swaps body→Fraunces (§6) |

### Contrast corrections (a11y — from the Jun 11 review)
The reused palette is colorblind-safe, but **three tokens fail WCAG 2.1 AA** at the sizes they're used. Fix at the
**token layer** (one change propagates to every screen); measured ratios in §6. Corrected hexes verified ≥4.5:1 on
their worst-case background:

| Token | Current | Used for | Was | **Corrected → ratio** |
|---|---|---|---|---|
| `--faint` | `#9aa0aa` | eyebrows · metadata · mono labels · plot axes · the "legible at 60+" footer | 2.3–2.6:1 ✗ | **`#6b6f77`** → 4.5–5.0:1 |
| `--question-ink` / `--warn` | `#c2861a` | "illustrative" pill · stale-flag "no result · 12d" · warn text | 2.8–3.0:1 ✗ | **`#8f6210`** → 4.7–5.1:1 |
| `--source-ink` | `#1b9a92` | `.relfeed__why` match-reason chip (⟡) | 2.9:1 ✗ | **`#137068`** → ~5.0:1 |

> Node-type **badges** (EVIDENCE/STUDY/PROTOCOL on their washes) sit at 3.1–3.9:1 — they clear AA-large but fail
> AA-normal at ~10px. Lower priority (shape + position reinforce them), but darken on the next palette pass. The
> §6 serif/spacing toggle fixes *size*, not *contrast* — these token changes are the contrast half.

### Existing components reused
`.node(.evidence/.study/.protocol/.candidate/.private)` · `.ntype` · `.pointer(.git/.data/.local/.video)` ·
`.request(.open)` · `.edge(.grounds/.follows/.request_for)` · `.viewswitch` · `.preset(.active)` ·
`.share-level(.consortium/.world/.gated)` · `.dash-search` · `.toast` · `.kbd` · `.avatar(.sean/.kate/.grad)` ·
`.reveal .d1…d8` · `@keyframes rise/pulse/nudge`.

### New atoms to add (extension block at the bottom of `components.css`)

| Class | Role | Section |
|---|---|---|
| `.ecard` + `.ecard__plot/__summary/__caveats/__method/__studyfoot` | Evidence card with embedded, progressively-disclosed method | §2 |
| `.disclosure` (native `<details>/<summary>`) | Collapsible method / study details (a11y-free for keyboard) | §2 |
| `.thread` + `.comment` + `.comment__actions` | Comment thread, separate from node body | §3 |
| `.convert-menu` + `.convert-btn` | Hover/focus action: candidate→evidence, comment→request/next-step | §3, §8-flow |
| `.kcard__grip` + `.kcol--drop` + `.stale-flag` + `.nudge-btn` | Drag handle, drop target, staleness nudge | §4 |
| `.relfeed` + `.relfeed__item` + `.relfeed__why` | Related-work newsfeed + match-reason chip | §5 |
| `.a11y-pop` + `.a11y-opt` (+ body modes `.pref-serif`, `.pref-spacious`) | Accessibility controls popover | §6 |
| `.ctx-menu` + `.group-pill` + `.share-verify` | Right-click share menu, group target, verification sheet | §7 |

---

## 2. Evidence card — embedded method, result-first  *(the headline change)*

### Overview
Kate's central note: she **never thinks about evidence and method separately**, so the dashboard must stop
making her hop `Evidence → Study → Protocol` as peer cards. Instead the **Evidence card is the unit**: result
and plot on top, a one-line summary + caveats, then **method/context embedded and collapsed**, with **full study
details on click-through**. This realizes R4 (*summary for the glancer, lineage for the reproducer*) **inside one
card** rather than across the canvas. Mirrors Matthew's sketch (a `Question` card → *description · experimental
context · comments* → *Study details*).

### Anatomy / layout (top → bottom — this order is load-bearing: *"see results 1st"*)
```
┌─ .ecard  (border-left 4px var(--evidence-ink), radius var(--r-md), pad 16–20px) ─┐
│  [.ntype.evidence]              Sean · Vogel Lab   [illustrative · unpublished]  │  ← provenance byline, R8
│  .ecard__plot     two-channel intensity-vs-time figure (G3BP1 before PABP1)      │  ← the result, FIRST
│  .ecard__summary  one bold sentence: "G3BP1 is recruited before PABP1…"          │
│  .ecard__caveats  • HeLa, acute stress • n caveats — 2–4 muted bullets           │
│  ▸ .ecard__method  «Method & context»  (collapsed .disclosure)                   │  ← embeds Study+Protocol
│       motivating Question · experimental context table · segmentation threshold  │
│       pointer chips: [git][data][local· request access][video]                   │
│  .ecard__studyfoot  "Open full study details →"  (links to the Study node)       │  ← deepest tier, on demand
└──────────────────────────────────────────────────────────────────────────────────┘
```

### Tokens & spacing
| Element | Spec |
|---|---|
| Card | `background var(--surface)`; `border 1px var(--line)`; `border-left 4px var(--evidence-ink)`; `border-radius var(--r-md)`; `padding 16px` (mobile) → `20px` (≥768px); `box-shadow var(--shadow-1)`, `→ var(--shadow-2)` on hover |
| `.ecard__plot` | full-bleed minus padding; `border-radius var(--r-sm)`; caption row uses `.illus` pill (`--warn` on `--question-wash`) |
| `.ecard__summary` | `font-display 16.5px/1.28`; the protein-order clause in `--evidence-ink`, `font-style italic`, `font-weight 600` (matches `.insp .obs .accent` in mockup 07) |
| `.ecard__caveats` | `font-body 12.5px`, `color var(--muted)`, bullet gap `6px` |
| `.ecard__method` header | `font-mono 10px`, `letter-spacing .1em`, `text-transform uppercase`, `color var(--protocol-ink)` (reuse `.threshold-detail .th-h` from mockup 08); chevron rotates on open |
| `.ecard__studyfoot` | `.btn.ghost.sm`, label "Open full study details →" |

### States & interactions
| Element | State | Behavior |
|---|---|---|
| `.ecard__method` (`<details>`) | default | **Collapsed.** Header + chevron only. *"It doesn't have to be open, but I can open it."* |
| `.ecard__method` | open | Expands method table + threshold + pointer chips; chevron 0°→90°, `transition: transform .2s var(--ease)`; body `.reveal`-style fade-in (respect reduced-motion) |
| `.ecard__method` | preset = *Results centered* | Stays collapsed (PI glance path) |
| `.ecard__method` | preset = *Reproducer* / deep-link from a Request | **Auto-expanded**, scrolled to the **segmentation threshold** row |
| `.pointer.local` (raw stacks) | always | Reads **"request access"** (R5) — never a download; `.kind.local` grey |
| `.ecard__studyfoot` | click | Routes to the standalone `Study` node (kept addressable for the reproducer who *does* want the separate view) |

### Content / edge cases
- **No plot yet** (candidate or in-progress): replace `.ecard__plot` with a `--surface-2` placeholder + *"No result posted yet"*; summary line becomes the candidate observation (*"cells died" / "quantification all over the place"*).
- **Long method table**: cap embedded view at the **threshold + 4 key rows**; overflow lives behind `.ecard__studyfoot`. Don't duplicate rows shown inline.
- **"Don't duplicate results & experiments"** (explicit ask): the embedded method is a **transclusion/link** to the one `Study` node, not a copy — editing the Study updates both. Result and experiment are **linked, not mirrored** into separate columns.
- **De-dup vs. Question column**: the `Question` still exists as its own node (it's the Consortium grouping key, §8) — but within the card it appears only as the *motivating question* line, not a third peer card.

### Accessibility
- Use native `<details>/<summary>` so the disclosure is keyboard-operable and announced ("collapsed/expanded") with **no JS**.
- Focus order: byline → plot (figure, `role="img"` + alt = the finding) → summary → caveats → method `<summary>` → studyfoot.
- The italic-accent result clause must not rely on color alone (it's also bold + italic) — already colorblind-safe.

### Motion
Card enters with `.reveal`. On method-open, the threshold row gets a one-shot `pulse` (`--request`-less, `--protocol` tint) to draw the reproducer's eye. Reduced-motion: instant.

---

## 3. Comment thread + convert-to-request  *(the reverse current, made conversational)*

### Overview
Kate wants to discuss a result **without burying the result or the method** — *"the thread… is separate from
the method/next-steps, because then we get buried."* And from any comment she wants a **one-click escalation**
to a formal `Request` or a next-step. This extends v0.1 §2.4 (the request-back current) from a single send-a-Request
action into an **inline thread with promotion**.

### Anatomy
```
.ecard  ▸ collapsed "💬 Discussion (3)"  →  opens .thread (a separate region, not the node body)
  .thread
    .comment   [avatar.kate] Kate · 2d   "Why threshold = 0.18 and not Otsu?"   [⋯ .comment__actions]
    .comment   [avatar.sean] Sean · 1d   "Otsu over-segments the dim PABP1 channel."
    .comment   [avatar.kate] Kate · 4h   "Makes sense. Can we also try 3 µM?"    →  [Convert to request]
    ─ composer (read-only dashboards: disabled with "sign in to comment") ─
```

### Tokens & components
| Element | Spec |
|---|---|
| `.thread` | inset region: `background var(--surface-2)`, `border-radius var(--r-md)`, `padding 14px`, sits **below** `.ecard__studyfoot`, visually divided by `.divider` |
| `.comment` | `avatar` 21px + body `font-body 12.5px/1.5`; meta `font-mono 10px var(--faint)` |
| `.comment__actions` | `.btn.ghost.sm` kebab → `.convert-menu` |
| `.convert-menu` items | **"Convert to request"** (→ `.request` indigo, `request_for` edge) · **"Push to next steps"** (→ Study backlog, §4) · "Copy link" |
| promoted `.request` | reuses existing `.request` chip; appears in the node's request rail **and** Kate's request column |

### States & interactions
| Element | State | Behavior |
|---|---|---|
| Discussion summary | default | Collapsed pill `💬 Discussion (n)`; `n` in `.tag` |
| `.thread` | open | Expands; newest comment `.reveal` |
| `.comment__actions` → Convert to request | click | Comment text **prefills** the Request body; confirm sheet shows *what becomes a Request and who's notified*; on confirm, a `.toast` ("Request sent to Sean") + the comment gets a `.request` chip badge "→ request" |
| Convert | undo | Toast carries **Undo** for 6s before the notification fires (avoid accidental asks) |
| Composer | read-only dashboard | Disabled, hint *"viewing only"* — consistent with R-readonly (authoring stays in the vault) |

### Edge cases
- **Empty thread**: show the composer only (or, on a public read-only board, hide the region entirely — don't show "0 comments").
- **Long thread**: collapse to last 3 with *"Show 7 earlier"*; the converted-comment stays pinned.
- **Comment references a withheld node**: never render the private target (R5) — show *"links to a private note"* with no title.
- **Canonical handler (build note, §12-2)**: use the **one-level** convert handler in [`10-evidence-card.html`](./mockups/10-evidence-card.html) (adds the `→ request` chip). The spec-required **toast + 6 s Undo** must live in a `<script>` function (`convertToRequest(btn)`), **never an inline `onclick`** — the nested-quote inline version in [`08-traverse-and-request.html`](./mockups/08-traverse-and-request.html) breaks HTML attribute parsing and leaks JS as visible text.
- **Canonical thread copy**: Kate's last comment reads *"Makes sense. Can we also try a lower arsenite dose — does G3BP1 still lead if granules form more slowly?"* (the `10` wording supersedes the *"3 µM"* placeholder above and `08`'s truncated variant).

### Accessibility
Thread is a `role="log"`/`aria-live="polite"` region for new comments; the kebab is a `<button aria-haspopup="menu">`; convert actions are menu items with explicit labels ("Convert this comment to a Request").

---

## 4. Kanban status-drag, PI prioritization & the stale nudge

> **⚠ SUPERSEDED IN PART (Jun 11).** The **PI nudge / `.stale-flag` / `.nudge-btn`** specced in this section was
> **rejected by the trainee** in the Jun 11 session and is **reversed** — see
> [`ux-spec-update-2026-06-11-trainee-feedback.md`](./ux-spec-update-2026-06-11-trainee-feedback.md) §1 and the
> research layer [`ux-research-synthesis-2026-06-11-trainee.md`](./ux-research-synthesis-2026-06-11-trainee.md).
> The **status-drag** and **PI reorder** mechanics below still stand; only the **nudge** dies — replaced by a
> student-set deadline + meeting-anchored "for next sync" agenda + an opt-in "follow" (mockups 09, new 11).

### Overview
Three intra-team behaviors the consortium board didn't cover. Drag is currently **graph-spatial only**
(*"drag nodes to arrange"* in mockup 07); Kate wants **drag = status change** on the Kanban, plus the ability to
**reorder a student's backlog by priority**, plus a **nudge when an experiment has stalled** (*"no result here for
a while — move it up or down"*).

### Layout / anatomy
Reuses the `.kcol` / `.kcard` board in [`07`](./mockups/07-shared-dashboard.html) & [`09`](./mockups/09-consortium-view.html).
Columns are the **status lanes** (e.g. *Questions · In progress · Results*). Add:
- `.kcard__grip` — 6-dot drag handle, `var(--faint)`, appears on hover/focus at card top-left.
- `.kcol--drop` — drop-target state: `border 1.5px dashed var(--study-ink)`, `background var(--study-wash)`.
- `.stale-flag` — corner flag on a card with no result past threshold: `--warn` dot + `font-mono 10px` "no result · 12d".
- `.nudge-btn` — on the stale card, a `.btn.ghost.sm` "Nudge" (PI→student ping).

### States & interactions
| Action | Trigger | Behavior | Rule |
|---|---|---|---|
| **Status change** | drag `.kcard` across columns | Card lifts to `--shadow-3` + 2° tilt; target column `.kcol--drop`; on drop, the node's **`status` field updates** and the column count (`.ct`) re-tallies. Optimistic; `.toast` on sync. | status is authored data; dashboard write here is the **one** exception to read-only — gated to the **owner/PI**, never public viewers |
| **Reorder priority** | drag within a column (PI only) | Vertical reorder = **priority rank**; persists to the student's backlog order. *"This one should go to the top."* | PI-or-owner only |
| **Stale nudge** | auto: `now − last_update > N days` on an in-progress card | `.stale-flag` appears; PI sees `.nudge-btn`. Click → notifies the student *"what observations did you make?"* | a prompt, not a status change |
| Keyboard reorder | focus `.kcard__grip`, `Space` to lift, `↑/↓` move, `Space` drop | Same result as pointer drag (mandatory a11y path) | — |
| Public viewer | any | **No grip, no drag** — board is read-only for non-owners | R-readonly |

### Edge cases & content
- **Permission**: drag/reorder visible **only** to the node owner and the project PI; everyone else gets a static board. (Ties to §8 — who can mutate.)
- **Stale threshold** `N` is per-project configurable; default **14 days**; never flag candidates that are explicitly "parked."
- **Conflict**: two editors move the same card → last-write-wins + a `.toast` reconciliation note (full merge deferred, per v0.1 §7).
- **Empty column**: dashed `.kcol--drop` ghost with the column name, so it's a visible drop target.

### Motion
Lift: `transform: translateY(-2px) rotate(1.5deg)` + `--shadow-3`, `transition .18s var(--ease)`. Drop settles with `rise`. Stale-flag enters with a single `nudge` bounce (already defined). Reduced-motion: no tilt, instant settle.

---

## 5. Related-work newsfeed (keyword / tag overlap)

### Overview
The strongest *new* want: an **ambient feed of related work** so people *"know to ask"* and stop *"accidentally
doing the same thing."* Triggered by **overlapping keywords/tags** — **cell line · protein · method** — across the
lab (and consortium). Today this only happens at a *"bi-weekly roundtable"*; the feed makes it continuous. Distinct
from mockup 09's *orphaned-results* feed (that's discovery of forgotten nodes; this is **match-to-my-active-work**).

### Anatomy
A right-rail tile on the dashboard (and on the student's own board):
```
.relfeed   « Related to your work »
  .relfeed__item
     [avatar.grad] Maya · Rivera Lab
     "Gibson assembly into BL21 — low yield, codon issue"
     .relfeed__why  ⟡ shared: method=Gibson · strain=BL21        ← why it matched
     [Evidence ·candidate]   2d
  .relfeed__item  …
```

### Tokens & components
| Element | Spec |
|---|---|
| `.relfeed` | `.panel.pad`; header `.eyebrow` "Related to your work" |
| `.relfeed__item` | `.node`-lite row, `gap 10px`, hairline `.divider` between |
| `.relfeed__why` | match-reason chip: `font-mono 10.5px`, `--source-ink` on `--source-wash`, prefixed ⟡; lists the **overlapping tags** that fired the match |
| node-type dot | reuse `.ntype` mini (evidence/candidate/result) |

### States & interactions
| State | Behavior |
|---|---|
| Default | Up to 5 items, ranked by **overlap count then recency**; *"newsfeed of related"* |
| Item hover | Reveals a `.btn.ghost.sm` "Open" + "Express interest" (reuse `.recruit-btn` from mockup 09) |
| Match transparency | `.relfeed__why` **always** states the matched tags — never an opaque "recommended"; this is the trust requirement |
| Empty | *"No overlaps yet — tag your studies with cell line, protein, and method to populate this."* (teaches the tagging that powers it) |
| Mute | Per-tag mute (⋯ → "Mute method=Gibson") so the feed doesn't get noisy |

### Edge cases
- **Privacy (R5)**: the feed may surface only nodes **shared with a scope the viewer is in** — never leak a private node because its tags matched. A match against a private node shows nothing.
- **Candidate/troubleshooting items are first-class here** — the *"don't use this lot of lithium acetate"* / *"lower concentration"* kind of tacit fix is exactly what should surface.
- **Over-matching**: a ubiquitous tag (e.g. `HeLa`) shouldn't dominate — weight rarer tags higher; cap one-tag-only matches.

### Accessibility
`.relfeed` is `aria-live="polite"` (new items announced quietly); each item is a link with an accessible name combining the title + match reason ("…matched on method Gibson, strain BL21").

---

## 6. Accessibility controls — serif toggle + spacing

### Overview
A concrete, explicitly-requested addition to the existing accessibility bar (colorblind-safe, 60+-legible):
a **serif-font toggle** and a **spacing/line-height** control. *"Being able to change the font… so it's all serif.
And somewhat the spacing."* Sean's caveat — *"too much customization can be bad"* — so this is **two toggles, not a
theme editor.**

### Anatomy & tokens
Extend the existing `.a11y` chip (mockup 07) into a small popover `.a11y-pop` in the dashboard chrome:
| Control | Mechanism | Spec |
|---|---|---|
| **Serif text** | body class `.pref-serif` sets `--font-body: var(--font-display)` (Fraunces, low optical size) | **No new font import** — Fraunces is already loaded with `opsz 9..144`; use `font-optical-sizing:auto` so body weights read as text, not display. Toggle is `aria-pressed`. |
| **Spacing** | 3-stop segmented control: Compact / Default / Spacious → body `.pref-spacious` etc. | `line-height 1.5 → 1.7`; `--gap 16px → 20px`; paragraph spacing scales. Persisted to `localStorage`. |
| (existing) Colorblind-safe | always on | node palette is already CB-safe; no toggle needed |

### States & interactions
| Element | State | Behavior |
|---|---|---|
| `.a11y` trigger | rest / open | Chip in `.winbar`/board head; opens `.a11y-pop` (`role="dialog"`, focus-trapped) |
| Serif toggle | on | Whole dashboard body switches to serif instantly; persists across screens 07/08/09 and reload |
| Spacing | change | Re-flows; cards reflow gracefully (no fixed heights — see edge cases) |

### Edge cases
- **Don't break monospace**: pointers, URLs, RIDs, hashes stay `--font-mono` regardless of serif mode.
- **Reflow safety**: spacious mode increases card height — all card heights must be content-driven (no clamped `.kcard` heights), or text clips. Audit `.kcard`, `.ecard`, `.rcard`.
- **Headings**: already Fraunces; serif mode changes **body** only, so the type hierarchy is preserved (don't make everything one font).

### Accessibility
Controls are real form controls (`<button aria-pressed>`, segmented `radiogroup`); preference persists; honors `prefers-reduced-motion` for the popover. Meets the stated EU-legibility intent.

### Color contrast *(Jun 11 review — the gap the serif/spacing toggle doesn't close)*
The serif + spacing toggle addresses **size/legibility**; it does **nothing for contrast.** Measured against WCAG
2.1 AA, body text is fine (`--ink` 15.5:1, `--ink-2` 10:1, `--muted` 4.56:1), but:

| Token / use | On | Ratio | AA |
|---|---|---|---|
| `--faint` — eyebrows, metadata, mono labels, plot axes, the **"legible at 60+" footer itself** | paper / white / surface-2 | **2.35–2.63:1** | ✗ fails (incl. AA-large) |
| `--question-ink` — "illustrative" pill **and** stale-flag "no result · 12d" | `--question-wash` | **2.76:1** | ✗ fails |
| `--warn` text | paper | **2.95:1** | ✗ fails |
| `--source-ink` — `.relfeed__why` match chip (⟡) | `--source-wash` | **2.89:1** | ✗ fails |
| node badges (EVIDENCE/STUDY/PROTOCOL) | their washes | 3.1–3.9:1 | △ AA-large only |

**Required:** apply the §1 token corrections so every text-bearing token clears **4.5:1** (AA-normal). This is a
token-layer change, not a per-screen fix; once shipped, the "legible at 60+ · colorblind-safe" stamp is true as
written. Color is already never the sole channel (the result accent is bold + italic + colored) — keep that.

---

## 7. Share mechanics — right-click, groups, verification preview

### Overview
The share flow in mockup 06 (per-node visibility + audience) gains the **ergonomics Kate & Sean acted out**:
select on the canvas, **right-click → "Share with [group / person]"**, **ctrl-click to multi-select**, **Groups**
as a first-class target (Joel's idea), and — emphatically — a **verification preview that "feels robust"** showing
*exactly* what will be shared before it goes.

### Flow & anatomy
```
1. Build a collaboration canvas → select node(s)         (click; ctrl/⌘-click to add; marquee to sweep)
2. Right-click → .ctx-menu:
      Share with ▸   ── .group-pill: ⬢ Kate's Lab · ⬢ McGill Consortium · ⬢ McGill Public
                     ── people:      Kate · Maya · …
3. .share-verify sheet  «You're about to share»
      ✓ Evidence: G3BP1 before PABP1        → Kate's Lab (consortium)
      ✓ Study + Protocol (+ git/data ptrs)  → Kate's Lab
      ⚠ Raw stacks (80 GB)                  → request-access only
      ⊘ "Sean's working notes"              → withheld (not shared)
      [ Cancel ]                    [ Share 3 nodes with Kate's Lab ]
```

### Tokens & components
| Element | Spec |
|---|---|
| `.ctx-menu` | `.panel` + `shadow-3`, `font-body 13px`, item pad `8px 12px`, hover `--surface-2`; submenu on ▸ |
| `.group-pill` | hex-bullet pill, `--request-wash`/`--request-ink` for a consortium group, `--claim` for a public group; reuse `.share-level` semantics |
| `.share-verify` | modal `.panel.pad`, max-width 520px; one row per node with a **visibility verb + target**; uses `.share-level` chips and the `.node.private` hatch for withheld |
| primary action | `.btn.primary` labeled with the **count + target** ("Share 3 nodes with Kate's Lab") — never a bare "Share" |

### States & interactions
| Element | State | Behavior | Rule |
|---|---|---|---|
| Multi-select | ctrl/⌘-click, marquee | Selected nodes get a 2px `--brand` ring; count badge follows cursor |
| `.share-verify` | always before send | **Mandatory**; lists **every** node + its resolved visibility, including **withheld** ones shown as withheld (proof of no-leak) | R5 |
| Withheld row | — | `.node.private` hatch + "withheld" — the verification's whole point: *"you don't present something you don't intend to present"* | R5 |
| Gated resource | — | Raw-stack row reads **request-access**, with a `⚠` not an error | R2/R5 |
| Confirm | click | `.toast` "Shared with Kate's Lab"; nodes arrive **visible-not-editable** on the recipient dashboard | R-readonly |
| Groups mgmt | — | A group is a saved set of people; right-click target list is `groups, then people` |

### Edge cases
- **Connected private node**: if a selected node links to a withheld one, the sheet shows the withheld one **as withheld** (confirming the link won't leak it), never silently.
- **Re-share / revoke**: re-running the flow on a shared node shows current state and allows narrowing; revoke is a distinct destructive confirm.
- **Nothing selected** → right-click share is disabled with a hint.

### Accessibility
Right-click action must have a **keyboard equivalent** (Menu key / a visible "Share" button on the selection toolbar). `.ctx-menu` is a `role="menu"`; `.share-verify` is a focus-trapped `role="dialog"` with the primary action reachable and the node list as a `role="list"`.

---

## 8. Publish model & consortium grouping (delta)

### 8a. Local-server-vs-hosted, not a password
The session **replaced** "password-protected vs public" as the *mechanism*: each user's dashboard is a **local
server** they run (*"`localhost:7979`"*); it lives on their machine by default. **Public = an intentional hosting
act** (*"explicitly host the server on Amazon… find a URL to point at it"*) — closer to *publishing* than flipping
a flag, which is the **desired friction** (*"easier to open a closed system than close an open one,"* v0.1). Sharing
*to a person/group* delivers the node to **their** dashboard; sharing to a **public** user puts it on the hosted page.

| Surface | v0.1 framing | **v0.2 framing** | UX consequence |
|---|---|---|---|
| Private | password-protected | **local server, not hosted** | no login wall to build; nothing public by default |
| Consortium | password-protected, 20+ labs | a **shared/hosted instance** the group's dashboards sync to | still gated, but via hosting + membership, not a shared password |
| Public | public URL | **deliberately hosted** page | "Make public" is a **publish action with a checklist**, not a toggle |

**Open UX question (carry, don't solve):** *who* controls "make public" — *"maybe a trainee thinks it's fine to
push to the public."* Spec a **publish gate** (owner/PI confirmation) but defer the policy. (Updates v0.1 §8.)

### 8b. Consortium view — per-question grouping + progress
Kate validated [`09-consortium-view.html`](./mockups/09-consortium-view.html) (*"the consortium thing is nice…
focusing on the studies so people can coordinate"*) with two refinements:
- **Group by Question.** Consortium is organized **per question**, with **results per question** beneath — the
  `Question` is the cross-lab coordination key (and the node that the Evidence card's motivating-question line points back to).
- **Progress bar per study** — keep and reuse the existing `.progress` meter (Kate: *"I like this progression bar a
  lot"*); clicking a study opens its **Evidence card (§2)**, not a separate results view. *"Click on the individual
  study to go to the results."*

---

## 9. Updated acceptance criteria  *(delta to ux-user-story-dashboard.md §6)*

Add to the dashboard "done when":
7. **Embedded-method glance.** Opening an `Evidence` shows result → summary → caveats with **method collapsed
   inline**; expanding reveals the **segmentation threshold** without leaving the card; *full* study details are one
   click deeper. No separate `Study`/`Protocol` card is required to read the result. *(§2)*
8. **Discuss & escalate.** A result carries a **comment thread separate from its body**; any comment can be
   **converted to a `Request`** (notifying Sean, with Undo) or **pushed to next steps**. *(§3)*
9. **Candidate promotion with history.** A **candidate** can be **converted to evidence** by hover/drag; its
   **troubleshooting history is preserved** under the promoted node. *(§3-flow / open-q resolved)*
10. **Status & priority by drag.** The owner/PI **drags cards between columns** to set status and **reorders** a
    backlog by priority; stalled in-progress cards **surface a nudge**. Read-only for everyone else. *(§4)*
11. **Ambient related-work.** A per-user **newsfeed** surfaces related nodes by **overlapping cell line / protein /
    method**, always stating the **match reason**, never leaking private nodes. *(§5)*
12. **Legibility controls.** A **serif toggle** and **spacing control** are available and persist; monospace and
    heading hierarchy are preserved. *(§6)*
13. **Robust share verification.** Every share passes through a **preview that lists exactly what's shared**,
    including **withheld** nodes shown as withheld. *(§7)*
14. **Real Table view.** The **Table** view is functional and **equivalent to Graph** (no facade). *(§0 caveat)*
15. **Renders without clipping.** At every viewport ≥1080px the **Evidence card and right-rail feed render whole**
    inside the dashboard frame — no content lost to `.screen { overflow:hidden }`. *(§12-1)*
16. **No leaked handler code; one canonical convert.** The comment thread renders as **comments only** (no
    JavaScript as visible text), and `08` reuses `10`'s `convert-btn` handler verbatim. *(§12-2)*
17. **AA contrast.** Every text-bearing token meets **WCAG 2.1 AA** (≥4.5:1) after the §1 corrections. *(§1, §6)*

## 10. Open questions  *(delta to §8)*

- **Resolved into spec:** candidate→evidence formalization *with troubleshooting history* (§3) — was §8 bullet 1.
- **Still open / refined:**
  - **Who can "make public"** — owner vs. trainee gating on the publish action (§8a). *New.*
  - **Status-write on a "read-only" surface** — the board is read-only for viewers but the owner/PI mutates status & priority; define the exact write-scope and how it rides KOI. *New.*
  - **Newsfeed match scope & ranking** — which tags, how weighted, how to avoid `HeLa`-style over-matching, and the privacy boundary (§5). *New.*
  - **Convert-to-request notification/claim** propagation (carried from §8 v0.1).
  - **Non-text assets as schema fields** (carried) — now also gates the Evidence card's inline plot/CSV (§2).

## 11. Mockup work implied

| Mockup | Action |
|---|---|
| `07-shared-dashboard.html` | Swap the node inspector for the **`.ecard`** (embedded method, result-first); add **status-drag** affordances + **a11y controls** in the board head; add the **`.relfeed`** rail. |
| `08-traverse-and-request.html` | Re-cut as the **expanded `.ecard`** (method open at the threshold) + the **comment `.thread`** with **Convert-to-request**; keep the request-back as the escalation target. |
| `09-consortium-view.html` | Regroup **per Question**; keep the progress meter; wire **study→Evidence card**; show the **stale nudge** + PI reorder. |
| `06-share-to-dashboard.html` | Add **right-click `.ctx-menu` + Groups + `.share-verify`** sheet; reframe audience as **local vs. hosted** rather than password. |
| *new* `10-evidence-card.html` (suggested) | An isolated spec screen for the **`.ecard`** — the headline component — at all states (collapsed / open / candidate / with-thread). |

---

## 12. Design-review corrections  *(Jun 11)*

A render + WCAG audit of the **built** 06–10 mockups (not the spec prose). Two build-blockers first, then
consistency and contrast. Each item: where it is · what's wrong · the fix. Folded into §1 / §6 / §9 above; this
is the tracking list.

### 12-1 · 🔴 07 — Evidence card clipped off the right edge  *(build-blocker)*
The headline screen cuts off the result Kate is meant to glance at and trust. `.screen` is `max-width:1080px;
overflow:hidden` ([`components.css`](./mockups/components.css)); inside it the right-rail `.ecard` / `.relfeed`
render **415–417px wide in a 318px inspector track** — the plot `<svg viewBox="0 0 318 168">` forces a ~383px
intrinsic width and grid children default to `min-width:auto`, so `.screen.scrollWidth` is **1153** and the right
**~73px is clipped**: the `Consortium` chip reads "co…", the plot's right half and "illustrative · unpu…" are gone.
Reproduces at **every viewport ≥1080px**.
**Fix (minimal):** add `min-width:0` to `.inspector`, `.ecard`, `.relfeed` (the SVG is already `width:100%`, so it
then shrinks to the track). **Alt:** widen the track to `minmax(0,390px)` **and** raise `.screen` max-width to
~1160px. Prefer the minimal fix.

### 12-2 · 🔴 08 — comment thread leaks raw handler code  *(build-blocker)*
Kate's 3rd comment renders `'; btn.closest('.thread').insertBefore(toast, …) })(this)">` as **visible text**, with
a stray triangle glyph and an overlapping "Undo." The inline `onclick` (`08-traverse-and-request.html`, the
`#convReq` button) builds a toast whose **inner HTML contains a nested `onclick`** using `\"`-style escapes; the
HTML parser closes the outer attribute at the first un-escaped `"` and dumps the remainder as text, breaking the
DOM.
**Fix:** replace with the working **one-level** handler from `10-evidence-card.html` (chip-only). The spec-required
**toast + 6 s Undo** (§3) must move into a `<script>` function — **never an inline `onclick`** with nested quotes.
While here, **re-cut 08 as the `.ecard`** per §11 so every result surface shares one card.

### 12-3 · 🟡 Consistency — divergent copy & components
- **Convert handler:** `10` is correct, `08` is broken — same component, two implementations. Make `10` canonical (12-2).
- **Kate's 3rd comment** exists in three variants — spec §3 *"try 3 µM?"*, `10` the full sentence, `08` truncated.
  **Canonical = `10`'s** (now recorded in §3); fix the §3 anatomy line and `08`.
- **06 share-verify** is folded into one long scrolling panel; §7 specs it as a **distinct, mandatory sheet** with
  the **withheld-row proof** (*"you don't present something you don't intend to present"*). Promote it to its own step.

### 12-4 · 🟡 Contrast — fails the stamp it prints
`--faint` text (2.3–2.6:1) and the amber/teal on-wash chips (2.8–2.9:1) **fail WCAG AA**, while every footer claims
"legible at 60+ · colorblind-safe." Corrected tokens in **§1**; measured ratios in **§6**. Token-layer fix.

### 12-5 · 🟢 Carry-overs (already tracked)
- **Table view is still a facade** (07) — acceptance **#14** already requires Graph-equivalence; flag in handoff so
  it isn't mistaken for done.
- **06 dual-panel frame** — the right-click `.ctx-menu` and the full share sheet are shown open **together**, which
  reads ambiguously in a static frame. Show one live layer (recede or dismiss the menu once the sheet opens).

---

*MIRA × Discourse Graphs · push results to a shared web interface · Sean → Kate **dashboard** · UX handoff delta · draft **v0.2** + design-review corrections Jun 11 (§12) (supersedes v0.1 where they differ; narrative spec stays [`ux-user-story-dashboard.md`](./ux-user-story-dashboard.md)) · from the Jun 10 2026 Kate session*
 