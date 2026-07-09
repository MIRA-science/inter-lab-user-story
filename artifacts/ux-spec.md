# UX Specification — push results to a shared web interface (consolidated, current)

### MIRA × Discourse Graphs · the Sean → Kate **dashboard** (central) + QSI → **world** (public) · **current as of 2026-06-15**

> **What this is.** The **single, current UX specification** for the eleven mockups in
> [`mockups/`](./mockups/). It folds the user stories (v0.1), the design brief, and the four dated
> `ux-spec-update-*` deltas (v0.2 → v0.4) **plus** the Jun-15 glance-and-trust correction into one
> document that **describes the screens as they are actually built today** — with every superseded
> decision resolved to current truth and recorded in the [changelog (§11)](#11-changelog--provenance)
> so nothing is lost.
>
> **An agent should read _this_ file (+ [`AGENTS.md`](../AGENTS.md)) and the mockups — and need none of
> the dated docs to act.** The dated docs are retained as the dated record of *how* each decision was
> reached; this file is *what is true now*.
>
> **Two sources of truth sit above this doc, and win where they disagree with it:**
> 1. [`AGENTS.md`](../AGENTS.md) — the **normative product spec** (scope, schema, architecture, and the
>    **R1–R13** rules). This doc does not restate R1–R13; it cites them.
> 2. The **mockups + CSS** ([`mockups/tokens.css`](./mockups/tokens.css),
>    [`mockups/components.css`](./mockups/components.css)) — the **visual/coded source of truth**. Where
>    this doc and the CSS disagree on an exact value, **the CSS wins**; this doc carries the *intent*.

---

## 0. How to use this document

- **Build / maintain the mockups from §6 (screens) + §7 (components) + §8 (design system).** Each screen
  in §6 says what it shows *now*, its chrome, the components it composes, and the rules it must honour.
- **Exact developer specs** for the dashboard track (measurements, design tokens, component contracts,
  states, breakpoints, animations, a11y, and a spec-vs-built consistency report) live in
  [`dev-handoff-dashboard.md`](./dev-handoff-dashboard.md) — generated from this doc and verified against
  the built mockups 06–11.
- **Judge "done" from §9 (acceptance).** **Carry §10 (open questions) — do not implement them as
  decided.**
- **Normative rules** are `R1`…`R13` in [`AGENTS.md §5`](../AGENTS.md#5-rules--constraints-normative);
  the UX-level design principles distilled from the sessions are in [§5](#5-ux-principles-normative-for-design).
- **Provenance / context** (verified session quotes) lives in
  [`_grounding-dashboard.md`](./_grounding-dashboard.md) (central) and [`_grounding.md`](./_grounding.md)
  (QSI); the *rationale* for the no-nudge reversal is in
  [`ux-research-synthesis-2026-06-11-trainee.md`](./ux-research-synthesis-2026-06-11-trainee.md).
- **Status:** the **dashboard track (06–11)** is current through **v0.4 + the glance-and-trust
  correction**, all dated 2026-06-15. The **public track (01–05, QSI)** has been **stable since v0.1**.

### What this supersedes
This file is the canonical UX spec. It **supersedes for day-to-day work**:
[`ux-user-story-dashboard.md`](./ux-user-story-dashboard.md),
[`ux-user-story.md`](./ux-user-story.md), [`_design-brief.md`](./_design-brief.md), and **all** of
[`ux-spec-update-2026-06-10-dashboard.md`](./ux-spec-update-2026-06-10-dashboard.md),
[`ux-spec-update-2026-06-11-trainee-feedback.md`](./ux-spec-update-2026-06-11-trainee-feedback.md),
[`ux-spec-update-2026-06-15-glance-trust-and-verbatim.md`](./ux-spec-update-2026-06-15-glance-trust-and-verbatim.md),
[`ux-spec-update-2026-06-15-real-data-and-ui.md`](./ux-spec-update-2026-06-15-real-data-and-ui.md).
Those files remain in the repo as the dated decision record; their substance is here, current.

---

## 1. The product in one page

**One spine, two worked examples.** Push a scientific **result** to a **shared web interface** that
collaborators read **in a browser, with no discourse-graph tooling** — readable cold, trustworthy via
visible provenance, addressable at a URL.

| | **Central — Sean → Kate dashboard** (★ primary) | **Public extreme — QSI → world** |
|---|---|---|
| Reader | a **known collaborator** (PI + her lab) | a **stranger** (reviewer, funder, future self) |
| Surface | a **read-only shared dashboard**, local→hosted ([§5](#5-ux-principles-normative-for-design)) | a **public repo / database** at a URL |
| Currents | **two** — results out **and** a `Request` back (R13) | **one** — producer → public |
| Screens | **06–11** (6) | **01–05** (5) |
| Trust by | relationship + provenance | provenance + reproducibility alone |

**The pivotal insight (every tier): reader-dependent depth.** One bundle serves three readers — the
**glancer** (claim + key figure, *passively trusts*), the **reproducer** (traverses to the method +
pointers), the **citer** (a frozen, citable version). Summary up front; lineage and verbatim on demand.

**The schema (MIRA).** `Question · Claim · Evidence · Study · Protocol · Request`. The shareable unit is
**`Evidence` + its grounding `Study` + that Study's `Protocol`**, carrying **pointers, not payloads**.
Full data model: [`AGENTS.md §4`](../AGENTS.md#4-data-model--schema).

```
Claim    --addresses-->                  Question
Evidence --supports / opposes-->         Claim            # one Claim/model, many Evidence
Study    --grounds-->                    Evidence          # inverse: is_grounded_in
Study    --follows-->                    Protocol          # the canvas labels this "uses"
Request  --request_for / request_target--> Study/Analysis   # the reverse current — dashboard tier only
```

---

## 2. The worked example (the science) — **current**

> ⚠️ **The science spine changed in v0.4.** Earlier docs use a **Component A-before-Component B "recruitment /
> assembly-order"** placeholder. **That is retired.** Every dashboard screen (06–11) is now built on the
> lab's **real prepared discourse-graph nodes** in [`../discourse-graph/`](../discourse-graph/). If you
> see "recruitment order", "assembly order", "Component A", or "Component B" anywhere in the mockups, it is a
> regression — fix it.

### 2.1 Central worked example — cluster **composition over time** (Vogel × Kate)
Real nodes (committed in `ad8cd46`), with the discourse-graph→MIRA type morph **`Experiment → Study`,
`Result → Evidence`** (matches the JSON-LD: `Experiment ⊑ mira:Study`, `Result ⊑ mira:Evidence`).

| Node | Title | Notes |
|---|---|---|
| **Study** (was EXP) | "Live high-resolution imaging of cluster formation at staged times" | Component 1–Label-G + Component 2–Label-K in sample, ZEISS Elyra 7 |
| **Evidence** (was RES) | "Clusters change composition over time" | Pearson r(P1,P2): **negative → positive over ~10–15 min** |
| **Protocol ×3** | sample preparation · high-resolution imaging · **Segmentation & composition** | Study `follows` each; Evidence `is_grounded_in` Study |

**Generated for illustration** (each marked *illustrative / manufactured / preliminary* in-screen via
the honesty pill):

- **Q1** (anchor Question) — *"Do the two cluster components condense together, or separately and then mix?"*
- **Q2** (follow-up Question) — *"Are early clusters two merging clusters, or one cluster with sub-compartments?"* (the Result's real open second key-point)
- **C1** (Claim) — *"Components first condense separately, then mix as the cluster matures."*
- **S2** (Study) — *"Architecture test of early clusters (FRAP)"* — addresses Q2; the "on the agenda / planned" follow-up.
- **E2** (Evidence, preliminary) — *"Early clusters look like two adjacent aggregates."*
- **R1** (Request, Kate → Sean) — *"Push the time-course below 5 min"* — the early perturbant-vs-osmotic divergence.

**The load-bearing methods payoff** (what the reproducer traverses to, screen 08) is **the segmentation
choice**, *not* a single threshold: **segment on the per-pixel SUM of both channels** (not one),
thresholded just above background. That choice defines every cluster and therefore every co-localization
number.

**The result plot** — **Pearson r vs time**: a zero reference line; two perturbation conditions (**perturbant**,
**osmotic**) both climbing from negative to positive, **diverging in the first ~10–15 min then
converging.** Colour-blind-safe (coral / blue), inline labels + legend. Rendered at four sizes across
06 / 07 / 08 / 10.

**Pointers (R2)** — `git` → the analysis at a commit · `data` → the curated CSV *for this figure* ·
`local` → raw NDTiff stacks ≈ **80 GB on the lab's 25 TB array** (reads **"request access"**, never a
download) · `video` → a short walkthrough.

**Provenance byline (R8)** — *Sean · Vogel Lab · shared with Kate's lab.*

**Anonymization (preserve).** The real data redacts the components to **`Component 1` / `Component 2`** (shown
*Component 1–Label-G* / *Component 2–Label-K*). **Introduce no real component names.** Real and already-public, so
keep them: the methods (high-resolution imaging, perturbant, 3D Suite, regionprops) and the **inter-lab
provenance** (samples via the Montreal exchange; **Khalid maintains the preparation**).

**Verbatim ground truth (R4, screens 07→08).** Cards are *summaries*; the **human-authored node text is
the ground truth**, surfaced **summary-first / verbatim-on-demand** — a per-node collapsible opened by a
**plain human label** (**"Sean's notes" · "Experiment notes" · "The full protocol"**, each with a one-line
descriptor), styled as a quiet left-accent inset — **not** a cryptic "Read verbatim". Sources and what
must be reachable:

| Node | Source (`../discourse-graph/`) | Reachable verbatim |
|---|---|---|
| Evidence | `RES - Clusters Change Composition Over Time.md` | the three **Key Points** (negative→positive correlation; two-clusters-vs-sub-compartmented ambiguity; the significant early difference between the two perturbations) |
| Study | `EXP - HRI Imaging Clusters at Different Times during Formation.md` | the dated **Progress & Notes** (2025-08-23 Montreal samples / Khalid; 2025-10-14 pipeline + the two segmentation design choices) and the two **Hypotheses** |
| Protocol ×3 | `PRO-*.md` | **numbered steps** + the **Imaging / Processing parameter tables**; the preparation protocol's honest "TODO: get growth info from Khalid" stub |

### 2.2 Public worked example — QSI **MFX-2 magnetic-field effect** (stable, screens 01–05)
Use **verbatim**; caption every plot *"illustrative · unpublished — workshop prototype."*

- **Claim / model** — *the spin-pair quantum model of field-sensitivity.*
- **Evidence** — *"In MFX-2-bearing samples, the sign of the mean magnetic-field effect (MFE) flipped from positive to negative as magnetic field strength increased at low fields."*
- **Key figure** — mean MFE at **0.5 / 1.0 / 1.5 / 2.0 / 2.5 mT** (1 experiment each, **74 replicates each**; SEM with fit + cycle + scientific variation propagated): positive at low field, crosses zero, negative by ~2.5 mT.
- **Study** — *"FieldScope live signal imaging of MFX-2-bearing samples replicates across 5 magnetic field strengths."*
- **Protocol** — *"MFX-2 loading in samples; fieldscope acquisition under applied field; per-replicate MFE calculation."*
- **Pointers** — `git` → `quantum-sensing/mfe-analysis @ 7c1ae04 · /mfe_fit_analysis/` · `data` → `mfe_by_field.csv` · `local` → raw NDTiff (request access) · `video` · `doi`/`koi` once published.
- **Provenance** — *Brian & Morgan · Quantum Sensing Institute · CC-BY.*

---

## 3. Actors & personas
Scientists are the actors; KOI / the sync plugin / the schema / the dashboard build are **components**
([`AGENTS.md §3, §8`](../AGENTS.md#3-actors--personas)), not people.

**Central (Sean → Kate):** **Sean** — Vogel-Lab PhD producer (Obsidian + DG plugin; 25 TB array). **Kate**
— collaborating PI + consortium lead; **glances and trusts**; wants everything *"in one place …
searchable … three years from now."* **A grad student in Kate's lab** — the reproducer; *"what
segmentation did Sean use?"* **A public / funder visitor** — meets an audience preset; needs plain
language legible to *"a 60-year-old-plus professor."* (A real-person persona track — **Jackie**, a PI who
attends meetings on a phone/iPad mini — motivates the mobile-first surfaces; a `personas.md` is a
**deferred doc**, not a mockup.)

**Public (QSI):** **Brian** (bench producer) · **Morgan** (PI; owns the decision to publish; wants it
FAIR/interactive) · **a reader with no DG app** · **a citing / future-self reader**.

---

## 4. Data model
Canonical: [`AGENTS.md §4`](../AGENTS.md#4-data-model--schema) and the
[MIRA schema repo](https://github.com/MIRA-science/schema). Node types `Question / Claim / Evidence /
Study / Protocol / Request`; the bundle is `Evidence + Study + Protocol + pointers`. A **`candidate`**
node (provisional/informal) may be shared as-is. **Do not** add `Analysis` / `ELN` / `Data` node types
(deferred). The dashboard adds a few **node fields** that drive UI but not the schema grammar
(`next_step_by`, `agenda_for`, `state ∈ {active, parked, done}`, `followers`) — see [§7.5](#75-meeting-prep--agenda--deadlines--follow-screen-11).

---

## 5. UX principles (normative for design)
These bind the *interaction design* and are as load-bearing as R1–R13. Several **reverse earlier
readings** — they are current.

1. **Glance-and-trust is _passive_ — there is no "Endorse."** The PI trusts the summary and does **not**
   need the study's details *unless the result looks unexpected*. There is **no action for the PI to take
   on a shared result** — no endorse, approve, or sign-off. The only affordance the inspector's PI region
   needs is an **in-place expand/collapse of the grounding study & protocol** (→ verbatim). *(Was
   misread as an Endorse button through v0.1–v0.4; removed Jun 15.)* **Grep-clean for `Endorse`.**
2. **Accountability is horizontal and owner-initiated — there is no PI "nudge."** A supervisor-initiated
   stale-experiment ping is **removed**: it breaks deep-focus, reads as consumer-app pressure, and
   inverts a relationship that should be bidirectional. **The meeting is the checkpoint.** The replacement
   is owner-driven and calm: an **"For next sync" agenda tag**, a **self-set "Next step by ⟨date⟩"** that
   lapses onto the **owner's own** backlog, and an **opt-in "Follow"** the *student* grants the PI. **Only
   the node owner** sets a deadline or grants a follow; **the PI receives, never initiates.** *(Reversed
   the v0.2 nudge; current.)*
3. **Summary up front, lineage & verbatim on demand (R4).** Lead with the figure + one-line observation;
   make traversal and the verbatim node text available but **never forced open**.
4. **Read-only surface, with one narrow write exception.** The dashboard is a **viewing/discovery
   surface** — no authoring (R6); authoring stays in the vault. The **single** write exception is the
   **owner/PI** setting **status (kanban drag), priority (reorder), deadline, agenda, follow** — never a
   public viewer.
5. **Permissions visible, never leaking (R5).** Gated resources read **"request access"** (a `⚠`, not an
   error); a withheld node shows as **"withheld" / "N notes withheld"** and **must never** leak its
   contents, title, or a dangling reference — including through KOI.
6. **Public vs. private is _local-server-vs-hosted_, not a password.** Each user's dashboard is a **local
   server** by default; **public = an intentional hosting act** ("Make public" is a publish action with a
   checklist, not a flag-flip — the desired friction: *"easier to open a closed system than close an open
   one"*). Consortium = a shared/hosted instance the group's dashboards sync to, gated by hosting +
   membership. *(Refines the earlier "password-protected" framing; the screens still render a
   `.share-level.consortium` chip for the gated state.)*
7. **Derive, don't duplicate.** Deadline / agenda / staleness are **node fields**; the backlog and the
   diff view are **generated** from them (the Obligator/Notion model) — no parallel to-do app. `done` /
   `parked` nodes are **excluded** from staleness.
8. **The mockup shows the product, not the argument.** Design rationale (why no nudge, card anatomy,
   editorial quotes) lives **in this spec, not in the rendered screen.** Screens carry plain functional
   copy.
9. **Distill & quiet (the impeccable pass).** One card pattern (the discourse node / `.ecard`) — **don't
   invent sibling card styles**; **≤2 intentional washes** per surface; each colour must *mean* something
   (left-border = node type; a wash marks a payoff, not decoration); use **one** of border/background/
   shadow, not all three; no auto-playing motion.
10. **Match transparency.** Anything recommended/surfaced (the related-work feed) **always states why it
    matched** — never an opaque "recommended."

---

## 6. The screens (current state — "as built")

Eleven self-contained HTML files in [`mockups/`](./mockups/), one design system
([`tokens.css`](./mockups/tokens.css) + [`components.css`](./mockups/components.css)); v0.4 additions are
screen-local CSS. Two chrome modes: **Obsidian dark** (`.theme-obsidian`) for producer screens (01, 06);
**light paper + `.winbar`** for reader screens (02–05, 07–11). Gallery:
[`mockups/index.html`](./mockups/index.html).

### Dashboard — Sean → Kate (★ central, 06–11)

#### 06 · Share a selected subgraph — [`06-share-to-dashboard.html`](./mockups/06-share-to-dashboard.html)
*Producer side, in Obsidian.* Sean picks the `Evidence`; the plugin proposes the bundle
(`Evidence + Study + Protocol + pointers`) and **offers connected nodes**.
- **"Share with" is multi-select** — checkbox rows so a producer can pick **more than one** target at
  once (e.g. *Kate's Lab* + *McGill Consortium*); a live **"Share with N selected…"** button. People list
  includes an inter-lab face (**Khalid · Montreal**).
- **Per-node visibility:** public / consortium / private; raw stacks **request-access-gated**; a
  **"working notes" node withheld** (present in the vault, invisible & un-leaked on the dashboard).
- **Mandatory `share-verify` sheet** before any send — lists **every** node with its resolved visibility,
  **including withheld nodes shown _as withheld_** (the proof-of-no-leak; *"you don't present something
  you don't intend to present"*). The primary action names the **count + target** ("Share 3 nodes with
  Kate's Lab"), never a bare "Share".
- A **candidate** node may ride along; push via **KOI** (shared = **visible, not editable**).
- Audience framing is **local-vs-hosted**, not password ([§5.6](#5-ux-principles-normative-for-design)).
- *Rules:* R1, R2, R4, R5, R12. *Chrome:* Obsidian dark.

#### 07 · The shared dashboard — [`07-shared-dashboard.html`](./mockups/07-shared-dashboard.html) — **the headline screen**
*A read-only web page at `dashboard.mira.science/vogel-kate` where Kate glances and trusts.*
- **View switch: `Graph · Kanban`** — **the Table view is removed** (it was empty). *(Any "Table" label
  in older copy or the index card is stale.)*
- **Kanban** columns **Questions · Experiments · Results**; cards are the composition-over-time nodes
  (Q1, Study, Evidence, Q2, S2, E2, candidate).
- **Subgraph highlight on hover/focus of _any_ card** (not just Questions): the cards sharing its
  `data-sub` stay lit, the rest dim ("dim-all-then-light-the-hits"); keyboard-accessible, reduced-motion
  safe.
- **Click a card → the right-rail inspector swaps to that node.** Seven per-node panels
  (e1 / e2 / q1 / q2 / s1 / s2 / cand), **each reusing the `.ecard` atom** (no new card style). **Default
  selection = the composition `Evidence`.**
- **The inspector's Evidence card** is **result-first with the method embedded** ([§7.1](#71-the-evidence-card-ecard--headline-component)): plot → summary →
  caveats, then a **prominent "How it was done — study & protocol" expand/collapse** (the promoted
  affordance that **replaced the removed Endorse block**) with a path to the **verbatim** detail (→ 08).
  **No endorse/approve anywhere.**
- **Audience presets do real work:** *Results* / *Experiments in progress* / *Questions & requests* dim
  non-matching cards, relabel the board head, and **spotlight the request composer** on "Questions &
  requests".
- **A request composer sits _beneath_ the kanban:** a typed-`Request` (`request_for` →
  [Study / Protocol detail / Raw data]) + target chip + message + **Send → on-node confirmation (no
  email)**.
- **A "Related to your work" newsfeed** in the right rail ([§7.6](#76-related-work-newsfeed)).
- Footer counts (e.g. *nodes shared · labs · consortium · open requests*). Colour-blind-safe, legible.
- *Rules:* R6, R8. *Chrome:* light / `.winbar`.

#### 08 · Traverse & request back — [`08-traverse-and-request.html`](./mockups/08-traverse-and-request.html)
*The reproducer follows the lineage to the method, then sends a Request.*
- The **Evidence card, method open**, then **"Follow the lineage to the method"**: `Evidence ←grounds←
  Study →follows→ Protocol`, down to the **segmentation choice** payoff (**segment on the per-pixel
  SUM**), which carries the **one** protocol-wash on the screen (colour = meaning).
- **Pointer chips** `git` / `data` / `video` resolve **across system boundaries** (R7); a **video
  walkthrough** panel (*"no Zoom call"*); the 80 GB raw stacks read **"request access"** (R5).
- **Verbatim-on-demand** under each node — a plain-labelled opener (**"Sean's notes" / "Experiment notes"
  / "The full protocol"**, a quiet left-accent inset): the Evidence's 3 Key Points, the Study's dated
  Progress-&-Notes + 2 Hypotheses, the Protocols' numbered steps + parameter tables — component redaction
  preserved.
- The **Request-back card** carries a **request-ink left accent** (same "left-border = node type"
  language); sending is the escalation target of the reverse current (R13).
- **Impeccable pass applied:** the discourse node is the **single primary card**; the explanatory `.why`
  aside and the gated-stacks note are **plain notes** (no wash); ≤2 competing washes.
- *Rules:* R2, R4, R5, R7, R13. *Chrome:* light / `.winbar`.

#### 09 · The consortium view & recruiting — [`09-consortium-view.html`](./mockups/09-consortium-view.html)
*Kate as consortium lead: one board, many labs, two audiences, one toggle.*
- **One public/password toggle** flips the same board between **consortium (gated)** and **public lab
  website**.
- **Audience presets:** *Experiments in progress (funder)* / *Open questions & requests (recruiting)* /
  *Results*.
- **Kanban** status columns **Planned · Running · In analysis** (a card may append "· in progress"); **a grab handle on _every_ card**,
  persistently visible (~0.4 opacity, brightening on hover) — the reorder affordance reads on each card.
- **The meeting-anchored mechanics** ([§7.5](#75-meeting-prep--agenda--deadlines--follow-screen-11)): an **"For next sync" `agenda-tag`**, an owner-set
  **"Next step by ⟨date⟩" `due-chip`**, and an owner-granted **"Follow"**. **No `stale-flag` alert and no
  `nudge` button** — the PI affordance is *follow / suggest-for-agenda*, never poke.
- A **Collaborators** rail (Sean, Kate, and other labs) and a **"Now discoverable"** feed surfacing
  orphaned one-off results; an **"In plain language"** panel for the public/recruiting read.
- *Rules:* R5, R6, R8. *Chrome:* light / `.winbar`.

#### 10 · The evidence card — embedded method — [`10-evidence-card.html`](./mockups/10-evidence-card.html)
*The isolated reference screen for the headline component, shown at **all four states**:*
**(a)** collapsed (PI glance) · **(b)** method open (reproducer, scrolled to the segmentation choice) ·
**(c)** **candidate** (no plot → *"No result posted yet"* placeholder + troubleshooting + **"Convert to
evidence"**) · **(d)** with a comment **thread** + **"Convert to request"**.
- **No card-anatomy / region-map strip** — that was design documentation and was removed (it lives in
  [§7.1](#71-the-evidence-card-ecard--headline-component) here).
- *Rules:* R2, R4, R8 (+ candidate, + reverse current). *Chrome:* light / `.winbar`.

#### 11 · Meeting prep — since last sync — [`11-meeting-prep.html`](./mockups/11-meeting-prep.html)
*A **mobile-first** pull surface the student opens to prepare for a sync — the calm replacement for the
nudge.*
- **"Since last sync" diff** with a **date picker** (default = last sync) + **Export summary** (for the
  phone). Three groups: **On the agenda (N)** → **What changed (N)** → **Not changed in a while (N,
  folded by default)** — *staleness lives in the folded group, never as an alert.*
- **Change-verb chips** are **calm, not red**: `＋ new evidence`, `▲ advanced`, `💬 N comments`,
  `no update · N d`.
- **Rows are clickable** → open the node (→ 08); a row that already owns a control (the "set next step"
  backlog row) is exempt.
- A **functional right rail** (not a justification): *This sync* intro · *Change key* legend ·
  **Followers** (opt-in, owner-granted; "Sean can revoke") · *Accessibility* (Serif · Dyslexia-friendly).
- **No "why there's no nudge" copy** anywhere (header, rail, or footer) — the rationale is in this spec
  ([§5.2](#5-ux-principles-normative-for-design)), not the screen.
- *Rules:* R4, R5, R6 + [§5.2](#5-ux-principles-normative-for-design). *Chrome:* light / `.winbar`, mobile-first.

### Public extreme — QSI → world (01–05, stable since v0.1)

| # | File | What it shows now | Rules · chrome |
|---|---|---|---|
| **01** | [`01-publish.html`](./mockups/01-publish.html) | Brian selects the `Evidence` bundle and opens **Publish**: pick destination(s) (DG web DB · Jupyter Book · desci/nanopub · PREreview), set each node **public vs. request-access-gated** (start closed; raw stack gated), add summary + methods table + walkthrough + "format-in-schema"; a private note is **withheld without leaking**. | R1, R2, R4, R5, R12 · Obsidian dark |
| **02** | [`02-evidence-node.html`](./mockups/02-evidence-node.html) | The `Evidence` node read cold: summary MFE figure first, observation, **"what's in the sample"** methods table, **pointer chips** — published · addressable · citable. Summary↔traversal duality. | R2, R3, R4, R8 · light card |
| **03** | [`03-public-web-view.html`](./mockups/03-public-web-view.html) | **The reference screen.** The result as a public web page at a URL: summary-first, traversable `Claim ← Evidence ← Study → Protocol`, pointer chips, provenance byline (QSI · CC-BY), a **frozen citation**, a **request-access** path to gated raw data, **import-if-you-DG**. | R4, R5, R6, R7, R8, R9 · light / `.winbar` |
| **04** | [`04-micropublication.html`](./mockups/04-micropublication.html) | Several independently-addressable `Evidence` bundles **compile into one Jupyter Book micropublication**; a `Claim`/model (spin-pair) emerges above them; narrative **AI-drafted, human-edited**; each subfigure links back to its **still-live** bundle. | R7, R11 · public web / book |
| **05** | [`05-public-database.html`](./mockups/05-public-database.html) | The published graph as a public dashboard — every node a URL with provenance and a **"cited by N"** signal — plus **citing a specific result with a frozen version pin** and the author seeing **who's using their work**. | R1, R6, R8, R9 · light / `.winbar` |

---

## 7. Component specifications
Reuse the base kit and the QSI / dashboard extension blocks in `components.css`; v0.4 atoms (request
composer, agenda/due/follow, diff view, subgraph-highlight) currently live as **screen-local CSS**.
**Don't reinvent the node card** — every result surface is the one `.ecard`.

### 7.1 The Evidence card (`.ecard`) — headline component
Result-first, with the method **embedded and progressively disclosed** inside one card (realizes R4
*within* a card, not across the canvas — Kate *"never thinks about evidence and method separately"*).

**Anatomy (top→bottom; order is load-bearing — _see results first_):**
```
┌─ .ecard  (border-left 4px var(--evidence-ink), radius --r-md, pad 16–20px) ─────────┐
│  [.ntype.evidence]            Sean · Vogel Lab    [illustrative · unpublished]      │ ← provenance, R8
│  .ecard__plot      Pearson-r-vs-time figure (perturbant + osmotic, neg→pos)           │ ← the result, FIRST
│  .ecard__summary   one bold sentence: "Clusters change composition over time"│
│  .ecard__caveats   2–4 muted bullets (sample, acute perturbation, n caveats)                │
│  ▸ "How it was done — study & protocol"   (collapsed .disclosure — PROMOTED)        │ ← embeds Study+Protocol;
│       motivating Question · context table · the segmentation choice (per-pixel SUM) │   replaces the old endorse block
│       pointer chips [git][data][local·request access][video] · the node's notes →  │
│  .ecard__studyfoot  "Open full study details →"  /  "…study & protocol →" (08)       │ ← deepest tier, on demand
└──────────────────────────────────────────────────────────────────────────────────────┘
```

| Aspect | Spec |
|---|---|
| **States** | **collapsed** (PI glance / *Results* preset) · **method-open** (auto-expanded + scrolled to the segmentation choice for the *reproducer* preset or a deep-link from a Request) · **candidate** (no plot → `--surface-2` placeholder *"No result posted yet"* + troubleshooting + **Convert to evidence**) · **with-thread** (§7.2) |
| **Disclosure** | native `<details>/<summary>` — keyboard-operable, announced, **no JS**; chevron 0°→90° on open; respect reduced-motion |
| **No Endorse** | the PI region has **no** endorse/approve/sign-off — only the study/protocol expander + the verbatim path |
| **`.pointer.local`** | always reads **"request access"** (R5), never a download |
| **Don't duplicate** | the embedded method is a **transclusion/link** to the one `Study` node, not a copy — editing the Study updates both |
| **Long method** | cap inline at the **payoff + ~4 key rows**; overflow behind "Open full study details →" |
| **A11y** | focus order byline → plot (`role="img"`, alt = the finding) → summary → caveats → method `<summary>` → studyfoot; the result accent is **bold + italic + colour** (never colour alone) |

### 7.2 Comment thread + convert-to-request
A discussion **kept separate from the result/method body** (*"otherwise we get buried"*), with one-click
escalation.
- `.thread` (inset `--surface-2`) below the studyfoot; `.comment` = avatar + body + a kebab
  `.comment__actions`.
- `.convert-menu`: **"Convert to request"** (→ indigo `.request`, `request_for` edge) · **"Push to next
  steps"** · "Copy link".
- **Convert** prefills the Request body, shows a confirm sheet (*what becomes a Request, who's notified*),
  then a **`.toast` with a 6 s Undo** before the notification fires.
- **Canonical handler:** the **one-level** `convertToRequest(btn)` in a `<script>` (as in `10`) — **never
  an inline `onclick` with nested quotes** (that leaks JS as visible text; the old `08` bug — fixed).
- Read-only board: composer disabled (*"viewing only"* / *"sign in to comment"*); empty thread on a public
  board is **hidden**, not "0 comments". A comment referencing a withheld node shows *"links to a private
  note"* with **no title** (R5).
- A11y: `role="log"` / `aria-live="polite"`; kebab `aria-haspopup="menu"`.

### 7.3 Candidate → evidence promotion
Hover/drag a **candidate** → **"Convert to evidence"**; the **full troubleshooting history is preserved**
under the promoted node. (Resolved what v0.1 left open.)

### 7.4 Kanban — status-drag, priority, grab handles (**no nudge**)
- `.kcard` + **`.kcard__grip`** on **every** card, **persistently visible** (~0.4 opacity → brighter on
  hover); `.kcol--drop` drop-target state.
- **Status change** = drag a card across columns (card lifts to `--shadow-3` + slight tilt; column count
  re-tallies; optimistic + `.toast`). **Priority** = vertical reorder within a column. Both are the
  **owner/PI write exception** ([§5.4](#5-ux-principles-normative-for-design)); **public viewers get a static board** (no grip, no drag).
- **Keyboard path mandatory:** focus grip → `Space` lift → `↑/↓` → `Space` drop.
- **Connection paths:** selecting a card should emphasize its connected cards across columns (the graph
  structure, legible in the board).
- **Removed:** `.nudge-btn` and the `.stale-flag` *as an alert*. Staleness is re-homed as the calm
  `no update · N d` row inside the folded diff group ([§7.5](#75-meeting-prep--agenda--deadlines--follow-screen-11)).
- Conflict = last-write-wins + a reconciliation toast (full merge deferred). `done`/`parked` never
  flagged stale.

### 7.5 Meeting prep — agenda / deadlines / follow (screen 11)
**Node fields drive everything (derive, don't duplicate — [§5.7](#5-ux-principles-normative-for-design)):** `next_step_by` (date, **owner-set**) ·
`agenda_for` (meeting ref) · `state ∈ {active, parked, done}` (excludes done/parked from staleness) ·
`followers` (people, **owner-granted**).

| Atom | Spec |
|---|---|
| `.agenda-tag` | "For next sync · ⟨date⟩" pill (request-tint). Set by the **owner**, or *proposed* by a followed PI as a suggestion ("Kate added to agenda") — never a poke. |
| `.due-chip` | owner-editable **"Next step by ⟨date⟩"** (question-tint); on lapse → calm `--muted` **"overdue · N d"** (never red). Lapse surfaces on the **owner's own** `.backlog` + the diff's stale group. |
| `.follow-btn` / `.follow-pill` | **owner-only grants**; once followed → "Kate follows" + avatar. A followed card that lapses badges the follower with the **same** signal the owner sees — **in-board badge by default**; email/iOS push **opt-in & deferred**. |
| `.diff-since` / `.diff-group` / `.diff-row` | the "Since last sync" view: date picker (default = last sync) + Export; three groups (agenda → changed → **folded** not-changed); calm change-verb chips. **Mobile-first.** |
| `.backlog` | the owner's personal "my experiments" board surfacing lapsed `next_step_by`. |

**The PI cannot initiate** — there is no PI-poke affordance anywhere.

### 7.6 Related-work newsfeed (`.relfeed`)
A right-rail ambient feed of related work, triggered by **overlapping tags — sample line · component ·
method** (so people *"know to ask"*). `.relfeed__item` rows; **`.relfeed__why`** match-reason chip
(`⟡ shared: method=… · strain=…`) — **always states why it matched** ([§5.10](#5-ux-principles-normative-for-design)), never opaque. Ranked by
overlap-count then recency; per-tag mute; **weight rare tags** (don't let `sample` dominate). **Privacy
(R5):** surface only nodes the viewer is already scoped into — a match against a private node shows
nothing. Candidate/troubleshooting items are first-class here.

### 7.7 Share mechanics
Right-click `.ctx-menu` with **multi-select checkbox** targets (**Groups** first-class, then people) and
**ctrl/⌘-click marquee multi-select** on the canvas; the **mandatory `.share-verify` sheet** before send
([screen 06](#06--share-a-selected-subgraph--06-share-to-dashboardhtml)) listing every node + resolved visibility, **withheld rows shown as withheld**. Re-share narrows;
revoke is a distinct destructive confirm. Right-click must have a keyboard equivalent.

### 7.8 Request composer
Beneath the kanban on 07 (and the escalation target on 08): a typed-`Request` composer — `request_for`
[Study / Protocol detail / Raw data] + a target chip + a message + **Send → on-node confirmation, no
email**. Spotlit by the *Questions & requests* preset.

### 7.9 Graph view — **partly built**
- **Built (07):** subgraph **highlight** — hover/focus any node dims the rest, lights its `data-sub`
  set; per-question "N linked" count; keyboard-accessible.
- **Specced but _not yet built_ (carried forward):** a real **top-to-bottom** graph view (questions top →
  claim → evidence → protocol below; left-to-right reserved for the Kanban), **filter-by-creator**,
  **filter-by-node = subgraph (bidirectional)**, **hover-emphasize / click-reorient**. Build the Graph
  view before the Table view (Table is removed). *Don't represent the full Graph view as done.*

### 7.10 Accessibility controls
A `.a11y-pop` popover in the dashboard chrome: **Serif** toggle (`.pref-serif` swaps body → Fraunces with
optical sizing — no new font) · **Spacing** (Compact/Default/Spacious → line-height 1.5→1.7, `--gap`
16→20) · **Dyslexia-friendly** (`.pref-dyslexia`: increased letter/word spacing, slightly heavier weight,
generous line-height; from the iOS HIG). Persist to `localStorage`. **Never** restyle monospace
(pointers/URLs/RIDs/hashes stay mono); headings stay Fraunces (serif mode changes **body** only); card
heights must be **content-driven** so spacious mode never clips. *"Two toggles, not a theme editor"* — no
new customization beyond these. Real form controls (`aria-pressed`, segmented `radiogroup`); honors
reduced-motion.

### 7.11 Shared atoms (reference)
`.viewswitch` (**Graph · Kanban** — Table removed) · `.preset(.active)` (functional audience filter) ·
`.dash-search` · `.node(.candidate/.private)` + `.ntype` · `.pointer .kind.{git,s3,local,video,data,doi,
koi}` (**chips, never a raw-data download**) · `.edge.{addresses,supports,grounds,follows,request_for}` ·
`.share-level.{world,gated,consortium,public,lab,private}` · `.cite(.frozen)` · `.reuse` ("cited by N") ·
`.request(.open)` · `.toast` · `.avatar.{sean,kate,grad,vogel,lab,brian,morgan,qbi,world}` (one colour
per person, everywhere) · `.frame-caption` + `.eyebrow` · `.reveal .d1…d8`.

---

## 8. The design system
**The CSS is canonical** ([`tokens.css`](./mockups/tokens.css) + [`components.css`](./mockups/components.css)) — link and reuse it; the values below
mirror it for reference. **Where this disagrees with the CSS, the CSS wins.** The fullest reference
screens are [`03-public-web-view.html`](./mockups/03-public-web-view.html) (public) and
[`07-shared-dashboard.html`](./mockups/07-shared-dashboard.html) (dashboard).

**Aesthetic.** Warm-paper lab-notebook meets a credible open-science publication: minimal-scientific,
editorial, generous negative space, a faint dotted grid on `body`, soft **layered** shadows (never flat),
hairline rules. *"A result, with just enough of its lineage, standing on its own at a URL — trustworthy to
a stranger."* One memorable motion moment per screen.

**Type.** Three families, each with a job — **Fraunces** (`--font-display`: headings, node titles,
mastheads) · **Hanken Grotesk** (`--font-body`: all UI/body) · **JetBrains Mono** (`--font-mono`:
*anything machine-facing* — pointers, URLs, JSON-LD, RIDs, DOIs, hashes, eyebrows, edge labels). *If a
human wrote it → Fraunces/Hanken; if a machine emitted it → mono.* `h1 clamp(28,4vw,44)`, `h2 24`, `h3
18`, body `15/1.5`. One Google-Fonts `@import` in `tokens.css`; add no other fonts.

**Colour — node-type palette (FIXED & SEMANTIC; apply only via classes — never recolour):**

| Type | `-ink` | `-wash` | `-edge` | |
|---|---|---|---|---|
| Question | `#8f6210` | `#fbf0d4` | `#e7cd8f` | gold |
| Claim | `#2c9a61` | `#ddf2e5` | `#a6dcbd` | green |
| Evidence | `#d9573f` | `#fbe0da` | `#f0b1a3` | coral |
| Study | `#3877cf` | `#dbe9fb` | `#a6c6ef` | blue |
| Protocol | `#8460c5` | `#ece2f8` | `#c9b3ea` | violet |
| Request | `#4854bf` | `#e2e3f7` | `#aeb4eb` | indigo *(the reverse current)* |
| Source | `#137068` | `#d7f0ee` | `#98d9d3` | teal |

**Colour — chrome & status.** `--ink #1c2024` · `--ink-2 #3a3f47` · `--muted #6b7280` · `--faint
#6b6f77` · `--paper #faf8f4` · `--surface #fff` · `--surface-2 #f4f2ec` · `--line #e7e3d8` / `--line-2
#ece9e0` · `--brand #2f3a8c` / `--brand-press #232c6e` / `--brand-tint #eceefb`. Status: `--ok #2c9a61` ·
`--warn #8f6210` · `--danger #d9573f` · `--private #8a8f98`.

> **AA contrast is fixed at the token layer (done).** `--faint`, `--question-ink`/`--warn`, and
> `--source-ink` were darkened so every **text-bearing** token clears **WCAG 2.1 AA ≥ 4.5:1** — the
> "legible at 60+ · colorblind-safe" stamp is now true as printed. Colour is never the sole channel
> (the result accent is bold + italic + coloured). **Don't revert these hexes.** *(Node-type **badges** on
> their washes sit at AA-large; darken on a future palette pass.)*

**Radii / motion.** `--r-sm 6 · --r-md 10 · --r-lg 16 · --r-xl 22 · --r-pill 999`; `--gap 16` (→20
spacious); `--ease cubic-bezier(.2,.7,.2,1)` / `--ease-out (.16,1,.3,1)`; three layered shadows
`--shadow-1/2/3`. **Dark mode:** `.theme-obsidian` (producer screens).

**Build constraints.** Self-contained single `.html` (relative CSS hrefs, no build step, no network
beyond Google Fonts); **static-first** (correct with JS disabled — tiny inline vanilla JS only, **no
frameworks**); CSS-only motion that **honors `prefers-reduced-motion`**; centre at ~1080 px (responsive to
~720; **11 and the 07 glance surface are mobile-first**). **Caption every screen** (`.frame-caption` +
`.eyebrow` like `MOCKUP 07 · SHARED DASHBOARD` + `<h2>` + one sentence naming the rule it embodies).
**Every plot labelled** *"illustrative · unpublished — workshop prototype."*

---

## 9. Acceptance criteria (consolidated, current)
"Done when" for the worked cases. (This list **resolves** the superseded items — no endorse, no nudge,
no Table facade; see [§11](#11-changelog--provenance).)

**Dashboard track — Sean → Kate (composition-over-time):**
1. **Share** — Sean pushes a **selected subgraph** (`Evidence + Study + Protocol`, each with a pointer)
   from Obsidian to a **read-only dashboard at a URL**, with per-node visibility (raw stacks gated;
   working note **withheld, not leaked**); **≥2 share targets selectable at once**; a **mandatory verify
   sheet** lists every node incl. withheld.
2. **Glance** — Kate opens it in a **browser, no DG app**, picks a **view (Graph · Kanban) / preset**, and
   reads the result cold from summary + provenance. **No endorse/approve affordance exists** (grep-clean
   for `Endorse`).
3. **Embedded-method card** — an `Evidence` opens **result → summary → caveats** with the method
   **collapsed inline**; expanding reveals the **segmentation choice (per-pixel SUM)** without leaving the
   card; full study details and **verbatim** node text are one click deeper.
4. **Traverse** — a grad student follows the lineage to the segmentation choice, resolves `git`/`data`
   pointers across system boundaries, watches the walkthrough; the **verbatim** key-points / dated notes /
   hypotheses / parameter tables are reachable.
5. **Gate** — raw stacks are **request-access-gated**; access can be requested; **no private/un-shared
   node leaks** (incl. through KOI).
6. **Discuss & escalate** — a result carries a **thread separate from its body**; any comment **converts
   to a `Request`** (notifies Sean, with Undo) or pushes to next-steps; no leaked handler code; one
   canonical convert handler.
7. **Request** — a typed `Request` (composer on 07; escalation on 08) goes back; Sean is notified and can
   **claim** it.
8. **Candidate promotion** — a candidate **converts to evidence**, troubleshooting **history preserved**.
9. **Status & priority by drag** — the **owner/PI** drags cards for status and reorders for priority;
   **read-only (no grip) for everyone else**; every card shows a visible grab handle.
10. **Self-directed staleness (not a nudge)** — the **owner** sets `next_step_by`; on lapse it surfaces on
    the **owner's own backlog** + the diff view; **no `.nudge-btn` exists**; `done`/`parked` excluded.
11. **Meeting-anchored agenda** — nodes can be marked **"For next sync"**; the meeting-prep view groups
    *on-the-agenda* first; **rows navigate** (keyboard + click).
12. **Opt-in follow** — a **student** grants a PI **Follow**; the PI then gets the **same** lapse signal
    and **cannot initiate** one.
13. **Diff view** — a **"Since last sync"** view lists what changed since a date, stale work **folded**
    beneath, in **calm** styling, on **mobile**; no nudge justification in the rendered text.
14. **Ambient related-work** — a newsfeed surfaces related nodes by **overlapping sample line / component /
    method**, **always stating the match reason**, **never leaking** private nodes.
15. **Subgraph highlight** — hovering/focusing **any** card dims non-subgraph cards.
16. **Functional presets** — *Results / Experiments in progress / Questions & requests* relabel the board
    and dim non-matching cards; **no Table tab**.
17. **Click-to-inspect** — clicking a kanban card swaps the inspector to that node (each panel reuses
    `.ecard`).
18. **Legibility controls** — Serif + Spacing + Dyslexia-friendly toggles persist; monospace and heading
    hierarchy preserved.
19. **One card pattern** — every result surface is the `.ecard`; ≤2 competing washes per surface; AA
    contrast on every text token.
20. **Real data** — every screen is composition-over-time; **no** Component A/Component B/"assembly-order" residue;
    component redaction preserved; `Experiment→Study` / `Result→Evidence` morph on every badge.

**Public track — QSI (stable):**
P1. **Publish** a bundle to a **public repo/DB with a stable URL** (over KOI as JSON-LD). · P2.
**Read-cold** at the URL as a web page, **following the pointers**. · P3. **Gate** — raw stacks
request-access-gated; no dangling-reference leak. · P4. **Cite the exact, frozen version**; the producer
**sees who's citing**. · P5. **Compile** several bundles into one micropublication while each stays
**independently addressable**.

---

## 10. Open questions — NOT decided
Carry into design; do not implement as settled (superset of [`AGENTS.md §10`](../AGENTS.md#10-open-questions--not-decided) + the session deltas):
- **Who can "make public"** — owner vs. trainee gating on the publish/hosting action.
- **Status-write scope on a read-only surface** — exactly what the owner/PI may mutate, and how it rides
  KOI.
- **Notification channel of last resort** — a lapsed *followed* card: in-board badge only, or opt-in
  email/iOS push? (No push by default; consented allowed; channel unspecified.)
- **Meeting date source** the agenda anchors to (shared consortium calendar field vs. per-collaborator).
- **`done`/`parked` semantics** — owner-set state so the diff never nags finished work.
- **Mobile scope for v1** — responsive web now vs. an iOS app (which is what "unlocks" consented
  notifications + TestFlight whitelisting).
- **Newsfeed match scope & ranking** — which tags, weighting, `sample`-style over-matching, privacy
  boundary.
- **Candidate-node formalization** — approval/notification when someone formalizes your shared candidate.
- **Accounts & gating atop weak KOI**; **non-text assets** (CSV/image/binary) as referenceable schema fields
  (also gates the `.ecard` inline plot/CSV); **versioning mechanics & invalidation** (R9 + downstream
  recompute when the segmentation choice changes); **conflict / merge**; **figure-as-addressable-
  artifact**; **dangling references: signal vs. hide**; **reader-ladder ↔ schema mapping** (deferred);
  **dataset archiving**; **cross-tool validation** (Obsidian ↔ Jupyter Book/MyST ↔ Roam ↔ ATProto).

---

## 11. Changelog / provenance
The dated record, compressed. Each row's detail lives in the dated doc it names (retained in-repo).

| Version | Date · source | What changed (current effect) |
|---|---|---|
| **v0.1** | initial · the user stories + `_design-brief.md` | The spine, cast, two currents, five moments, the design system, screens 01–09. *(Then used the now-retired Component A/Component B placeholder science.)* |
| **v0.2** | Jun 10 · Kate's live walkthrough (`ux-spec-update-2026-06-10-dashboard.md`) | **Embedded-method `.ecard`** (result-first); **comment thread + convert-to-request**; **candidate→evidence with history**; **kanban status-drag + PI reorder**; **related-work newsfeed**; **a11y serif/spacing**; **share via right-click + Groups + verify sheet**; **local-server-vs-hosted** reframing. **+ Jun 11 design-review:** fixed the 07 card clipping & the 08 handler-code leak; **AA contrast corrected at the token layer.** *(The v0.2 PI nudge was reversed the next day — see v0.3.)* |
| **v0.3** | Jun 11 · trainee reaction (`ux-spec-update-2026-06-11-trainee-feedback.md`; rationale in `ux-research-synthesis-2026-06-11-trainee.md`) | **PI nudge REMOVED** → **meeting-anchored agenda** + **self-set deadline** + **opt-in Follow** + the **"Since last sync" diff (new screen 11)**; **real Graph-view spec** (top-to-bottom, creator filter, node→subgraph) + Kanban connection paths; **mobile-first** meeting/glance surfaces; **dyslexia-friendly** a11y. |
| **v0.4** | Jun 15 · real-data + UI walkthrough (`ux-spec-update-2026-06-15-real-data-and-ui.md`) | **Science spine → composition-over-time** on real nodes (`Experiment→Study`, `Result→Evidence`); **multi-select share** (06); **subgraph highlight on any node** + **click-to-inspect** + **functional presets** (07); **request composer** below the kanban; **grab handle on every card** (09); **de-justified 11** (rationale out of the screen); **Table view REMOVED**; **impeccable distill pass on 08**; **card-anatomy strip removed from 10**; **clickable diff rows on 11**. |
| **+** | Jun 15 · author clarification (`ux-spec-update-2026-06-15-glance-trust-and-verbatim.md`) | **"Endorse" REMOVED** — glance-and-trust is passive; **promoted the study/protocol expander** in its place; **verbatim study/protocol content must be reachable** (summary-first / verbatim-on-demand). |

---

*MIRA × Discourse Graphs · push results to a shared web interface · Sean → Kate dashboard (central) + QSI → world (public) · **consolidated UX spec, current as of 2026-06-15** · normative rules: [`AGENTS.md`](../AGENTS.md) · coded truth: [`mockups/`](./mockups/)*
