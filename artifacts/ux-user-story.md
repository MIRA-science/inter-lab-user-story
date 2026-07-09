# A result, standing on its own at a public URL — trustworthy to a stranger

> 🗂️ **Superseded for day-to-day work by [`ux-spec.md`](./ux-spec.md)** — the single current UX spec (this
> QBI narrative is folded into it as the public track, screens 01–05, which are stable). Retained for
> reference; its embedded design-system mirror (§9) predates the AA token corrections — `ux-spec.md §8` and
> the CSS are current and win on any disagreement. *(Consolidated 2026-06-15.)*

### Publishing a result to a public repository or database — MIRA × Discourse Graphs, 🥉 low-shared-context variant, draft v0.1

> **Premise.** Brian, at the Quantum Biology Institute, has a result living as markdown discourse-graph
> nodes in his Obsidian: in MagLOV2-expressing *E. coli*, the sign of the magnetic-field effect flips
> from positive to negative as the field strengthens. He and his PI Morgan want to **put it in front of
> the world** — not into one trusted colleague's graph, but into a **public repository or database**,
> at a **URL**, where a reviewer, a prospective collaborator, or his own future self can find it.
> **As a producer, Brian wants to publish a small, self-describing subgraph — the `Evidence`, plus the
> `Study` and `Protocol` that produced it, carrying *pointers* to the analysis and the curated dataset,
> never the raw image stacks — addressable on its own and citable.** **As a reader, anyone in the world
> can open the URL, understand it cold, follow the pointers, request access to what's gated, and cite
> the exact version.** One direction: results flow producer → the public (AGENTS.md §1; _grounding.md §0).

This is the **bronze tier** of the shared-context ladder — the public extreme. Where this repo's
**central** Sean → Kate dashboard has a *known* recipient, here there is **no known recipient and no
shared tooling**: the work must stand entirely on its own, and trust is earned by **visible provenance
and reproducibility**, not by who sent it.

---

## 1. The cast

Four people (and a stranger) stand around one published result. The pivotal insight is the same as the
inter-lab story — readers need *different depths* of the same bundle — but here it is aimed at people
who do not know the producer (AGENTS.md §3; _grounding.md §2).

#### Brian — the producer (bench)
- **Role:** researcher at the **Quantum Biology Institute**. Made the MagLOV2 result and assembled the
  evidence bundle.
- **Tool:** Obsidian + Discourse Graph plugin, everything local; analysis scripts and a curated dataset
  on hand, raw stacks on an institute drive.
- **Wants:** to publish the result *and* keep it part of his normal workflow — "make a README-like
  container that points to the whole package," not a separate chore (_grounding.md §3).
- **Defining moment:** "We made an evidence bundle!" — the `Evidence`, the analysis that `produces` it,
  the notebook that `describes` it, snapped together on the canvas (pilot, `Jik1z-xgT`).

#### Morgan — the producer (PI)
- **Role:** PI at the Quantum Biology Institute; owns the decision to publish.
- **Tool:** Obsidian / the web.
- **Wants:** the publication to be **FAIR** and ideally **interactive** — "she'd love the pub to be
  interactive, perhaps like distill/idyll" (_grounding.md §2.1).
- **Defining quote:** showing the metadata in the open made her happy — provenance you can *see*.

#### A reader with NO Discourse Graph app — the web-view persona
- **Role:** a reviewer, a clinician, a prospective collaborator — or anyone who lands on the link.
- **Tool:** **a web browser.** No Obsidian, no Roam, no plugin.
- **Wants:** to open the result at a **URL as a web page**, understand it cold, drill from the figure to
  the analysis and methods "in one space, in one spot," and follow the pointers (_grounding.md §2.3,
  quote `XSOMnSUyq`).
- **Defining quote:** "I hover on the figure panel… how do I see the scripts and understand the data
  collection procedures? I'm gonna go to the bundle that links to all those."

#### A citing / future-self reader — the reuse persona
- **Role:** any researcher who wants to *build on* the result — including Brian himself, months later,
  citing it from a new notebook.
- **Tool:** their own notebook (MyST / Obsidian / Roam) or a manuscript.
- **Wants:** to **link/cite a specific result** and trust that the **version they cited stays frozen**
  — "we've frozen it; it will be this way for as long as GitHub exists" (_grounding.md §3, `TAXQ_j36-`).

> **Naming note.** Brian and Morgan are QBI *scientists* and are first-class actors. The DG-team,
> schema, and connector people around the pilot are **roles/components** ([§8 of AGENTS.md](../AGENTS.md#8-architecture--stack)), not user-story actors.

---

## 2. The spine — one direction, producer → the world

The inter-lab story has two currents (results out, requests back). **This tier has one:** results flow
**producer → the public.** There is no known consumer to request anything back; the act is a
**publication**, addressable and citable, that *anyone* can pick up (AGENTS.md §1).

**The schema, compactly** (MIRA, the same grammar as the inter-lab story; AGENTS.md §4). The bundle is
the familiar self-describing subgraph — we keep `Evidence`/`Study`/`Protocol` and **defer** the richer
reader-ladder node types the pilot sketched (Analysis/ELN/Data) for v1:

> **Question · Claim · Evidence · Study · Protocol**

```
Claim     --addresses-->          Question
Evidence  --supports / opposes--> Claim          # one Claim/model, supported by many Evidence
Study     --grounds-->            Evidence        # inverse: is_grounded_in
Study     --follows-->            Protocol        # the canvas labels this "uses"
```

Two rules ride on this spine and must never be violated:
- **Pointers, not payloads** — the analysis is a git pointer, the data is a curated-CSV pointer, the
  raw stacks are a *request-access* pointer to an institute drive — never embedded (R2; _grounding.md §4).
- **Self-describing for a stranger** — every node reads on its own "like a paper's methods section,"
  because the person on the other end has no context to fill in (R4).

---

## 3. The four moments

When Brian publishes, the question is no longer *"who is on the other end?"* (he doesn't know) but
*"will this stand on its own, and can a stranger trust it?"* The story has four moments, each told as a
journey and pointed at the mockup that depicts it (AGENTS.md §6; _grounding.md §2).

The worked content throughout: the QBI `Evidence` — *"in MagLOV2-expressing E. coli, the sign of the
mean magnetic-field effect flipped from positive to negative as field strength increased at low
fields"* — grounded in a bacterioscope live-imaging `Study` across five field strengths, following a
MagLOV2-expression + applied-field `Protocol` (_grounding.md §2.2).

### Moment A — Publish to the public
*Brian turns a private bundle into a public, addressable record.*
→ **mockups/01-publish.html**

1. **He selects the unit and hits Publish** (mockup 01). In Obsidian he selects the `Evidence` node;
   the plugin assembles the default self-describing subgraph — `Evidence` + grounding `Study` +
   `Protocol` + pointers (R4).
2. **He chooses the destination(s)** — the DG **web database**, a **Jupyter Book** micropublication, a
   **desci/nanopub** record, or a **PREreview** request — behind one publish action (R1; AGENTS.md §8).
3. **He sets public vs. gated, starting closed** (R5). The three-node spine and the analysis/curated-
   data pointers go public; the **raw image stacks stay request-access-gated** on the institute drive;
   a private working note ("not sure the low-field point is real yet") is **withheld without leaking
   its existence**.
4. **He adds context and formats in schema** — a one-line summary, the "what's in the dish" methods
   table, a short walkthrough — so a stranger can read it the first time (AGENTS.md §7).

### Moment B — Read it cold, at a URL, with no app
*A stranger opens the link and understands the result without any tooling.*
→ **mockups/02-evidence-node.html · mockups/03-public-web-view.html**

1. **The result is a web page at a URL** (R6). For v0 this is markdown rendered as HTML — feasible
   precisely because the authoring format already *is* markdown (Roam `Initiative/Inter-graph
   functionality`: "each DG node displayable as a web page with a URL"; "markdown rendered as HTML is
   ok for v0").
2. **The reader reads it cold** (mockup 02 → 03). The page leads with the summary `Evidence` figure —
   the MFE sign-flip — captioned *illustrative · unpublished — workshop prototype*, then the observation
   statement and the methods table.
3. **They trust it via provenance** (R8). The byline shows *Brian & Morgan · Quantum Biology Institute ·
   CC-BY* — a stranger can see who stands behind the figure.
4. **They follow the pointers** (R2). The `git`, `data`, and `video` chips are live links — the analysis
   script, the curated CSV *for this figure* (not 50 CSVs), the walkthrough — while the page never hosts
   the raw stacks.
5. **They traverse on demand** (mockup 03). From the `Evidence` they follow `grounds` → `Study`, then
   `follows` → `Protocol`, resolving each pointer **across system boundaries** — the published record is
   the hub, its relations point to URLs in other systems (R7; _grounding.md §4).
6. **They hit a gate, gracefully** (R5). The raw-stack pointer reads *request access*; they can ask, and
   nothing un-published leaks.

### Moment C — Cite it, frozen
*A reader builds on the result from their own notebook, and the producer sees the reuse.*
→ **mockups/05-public-database.html**

1. **Cite the exact version** (R9). The reader grabs a citable handle pinned to the **version they saw**
   — frozen, so later edits don't silently change what they cited.
2. **Reuse across tools.** They drop the citation into a MyST/Obsidian/Roam notebook or a manuscript;
   if they run a graph, they can import the bundle instead.
3. **The producer sees who's using their work** — a "cited by N" signal on the public dashboard
   (_grounding.md §10: "as a cited author, see who's using my work").

### Moment D — Compile bundles into a narrative
*Once several results tell one story, they become a micropublication — without ceasing to be units.*
→ **mockups/04-micropublication.html**

1. **Publish-as-you-go** (R11). Each `Evidence` bundle is published first, independently.
2. **A claim emerges above them.** The radical-pair model isn't inside any single bundle — it's a
   `Claim` that several `Evidence` nodes `support` (_grounding.md §2.4).
3. **Bundle them once the story is clear** — compile into a **Jupyter Book** micropublication; each
   subfigure panel **links back to its still-addressable bundle**.
4. **AI drafts, a human edits.** Point an assistant at the collection of bundles + the spec to draft the
   connecting prose; a human edits it. "If we have the spec and the units, we can just assemble it into
   a UI" — the bundles ARE the publication; the narrative is one rendering (Joel's framing, _grounding.md §2.4).

---

## 4. Journey maps

### The PRODUCER journey — Brian (& Morgan)
*author → select the bundle → set public/gated → add summary + walkthrough → publish → get a URL/handle.*

| Stage | What Brian does | Feels like / rule |
|---|---|---|
| **Author** | Captures `Question → Claim → Evidence → Study → Protocol` as markdown DG nodes, in his normal workflow. | Frictionless; rides the existing workflow (AGENTS.md §7). |
| **Select the bundle** | Picks the `Evidence`; the plugin proposes `Evidence` + `Study` + `Protocol` + pointers. | Self-describing subgraph, readable cold (R4). |
| **Set public / gated** | Spine + analysis/curated-data public; **raw stacks gated**; private note withheld. | Permissions even when public; start closed; no leak (R5). |
| **Add context** | One-line summary, "what's in the dish" table, short walkthrough; format-in-schema. | Context for a stranger's first read (AGENTS.md §7). |
| **Publish** | Exports as JSON-LD; KOI mediates to the web DB / Jupyter Book / desci. | Pointers not payloads; schema-agnostic transport (R2, R10, R12). |
| **Get a handle** | Receives a **public URL** (and a citable, version-frozen handle). | Addressability is the deliverable (R6, R9). |

### The READER journeys — anyone in the world (this is R4 made concrete: it forks)
*The same published bundle serves three readers at three depths.*

**Branch 1 — the glancer (a busy reviewer / PI).** Opens the URL, sees the summary figure (the MFE
sign-flip) and the one-line observation, reads the provenance byline, and **trusts or moves on**. No
traversal needed (mockup 03).

**Branch 2 — the reproducer (a grad student / skeptic).** Follows `grounds` → `Study` → `follows` →
`Protocol`; reads the field strengths and colony counts; resolves the `git` pointer to the exact
analysis commit and opens the curated `mfe_by_field.csv`; **requests access** to the raw stacks only if
they intend to re-run (mockups 02 → 03).

**Branch 3 — the citer / future self.** Grabs the **frozen** citable handle and drops it into their own
notebook or manuscript; the producer later sees the **reuse** (mockup 05).

> **Why the fork matters:** one published bundle, three satisfied readers — *summary up front for the
> glancer; traversal for the reproducer; a frozen handle for the citer* (R4, R9; _grounding.md §2.3).
> Publish once; serve all three.

---

## 5. How the mockups map to the story

| Mockup file | What it shows | Rule / insight it embodies |
|---|---|---|
| `mockups/01-publish.html` | Brian selecting the `Evidence` bundle in Obsidian and the **Publish** dialog: destination picker, **public vs. request-access-gated** per node, the withheld private note, prompts for summary + methods + walkthrough. | R1 (publish to a public repo/DB); R2 (pointers); R4 (default bundle); R5 (permissions even when public, no leak). |
| `mockups/02-evidence-node.html` | The `Evidence` node — summary MFE figure first, observation statement, "what's in the dish" methods table, `git / data / local / video` **pointer chips** — framed as **published · addressable · citable**. | R2 (pointers not payloads); R3 (one Evidence type); R4 ("interpretable on its own"); R8 (provenance). |
| `mockups/03-public-web-view.html` | The result as a **public web page at a URL** for a reader with no DG app — summary-first, traversable `Claim ← Evidence ← Study → Protocol`, pointers, provenance byline (QBI · CC-BY), and sidebar CTAs: **cite (frozen)**, **request access** to the raw data, **import if you run a DG**. *(The reference screen.)* | R4, R5, R6 (URL), R7 (cross-boundary resolution), R8, R9 (freeze). |
| `mockups/04-micropublication.html` | Several independently-addressable `Evidence` bundles **compiled into one Jupyter Book micropublication**; a `Claim`/model emerges above them; the narrative is **AI-drafted, human-edited**; each subfigure links back to its live bundle. | R7; R11 ("specs instead of papers"; bundles stay addressable). |
| `mockups/05-public-database.html` | The **public DG database / dashboard** — a gallery/kanban of published nodes, each with a public URL, provenance, and **"cited by N"** — plus a reader **citing a specific result** with a **frozen version pin**, and the author seeing **who uses their work**. | R1, R6, R8, R9; _grounding.md §10 (see who's using my work). |

---

## 6. Acceptance — "done when" (from AGENTS.md §9)

For the worked MagLOV2 case:
1. **Publish path.** Brian publishes the bundle (`Evidence` + grounding `Study` + `Protocol`, each with
   a **pointer**) from Obsidian; it travels over KOI as JSON-LD and lands in a **public repo/database
   with a stable URL.**
2. **Read-cold path.** A reader with **no DG app** opens the URL as a **web page** (summary figure +
   methods table + walkthrough) and **follows the pointers** to the analysis and curated dataset.
3. **Gated-resource path.** The raw image stacks are **request-access-gated**; a reader can request
   access; **no un-published / gated node leaks** via a dangling reference.
4. **Cite path.** A reader **cites the exact, frozen version** from their own notebook; the producer
   can **see who is citing it**.
5. **Compile path.** Several published bundles are **compiled into one micropublication** (a Claim/model
   above them), and each bundle **remains independently addressable**.

---

## 7. Out of scope (v1)

- **Expanding the grammar** to Analysis / ELN / Data node types — **deferred**; v1 keeps canonical
  `Evidence`/`Study`/`Protocol` (AGENTS.md §2, §10).
- **Hosting or transferring raw data** — referenced only; the raw-stack pointer may be access-gated.
- **Conflict resolution / graph merge** — design to allow later; don't build now.
- **A central consortium gatekeeper** — out of scope (as in the inter-lab story).

## 8. Open questions carried into design

Surfaced, **not solved** (AGENTS.md §10; _grounding.md §5):

- **Reader-ladder ↔ schema mapping** (EVD → Analysis → ELN → Data vs. Evidence/Study/Protocol) — the
  next data-model task; deferred for v1.
- **Dataset archiving** — *how/whether* to archive & share the data is unresolved by the pilot itself
  ("punt dataset to later"); what does the data pointer resolve to, and who gates it?
- **Figure-as-addressable-artifact** — how a sub-panel becomes a node with its own ID/URL.
- **Dangling references: signal vs. hide** — carry an indicator of a missing related resource, or hide
  it? (MAP hides; Matt leans signal.)
- **Versioning mechanics** — how the frozen-version guarantee is implemented across the web DB, Jupyter
  Book, and desci/nanopub.
- **Gated permissions atop weak KOI** — enforcing request-access and per-resource gating.
- **Cross-tool validation** — Obsidian ↔ Jupyter Book/MyST ↔ Roam ↔ ATProto via the JSON-LD bridge.

---

## 9. Design system & usage constraints (for dashboard prototyping)

> **For the collaborator building prototypes.** Everything below is the shared visual language used
> across all nine mockups (`01`–`05` public/QBI, `06`–`09` Sean → Kate dashboard). **The canonical
> source of truth is the CSS, not this section** — link and reuse it, don't re-key the values:
> [`mockups/tokens.css`](./mockups/tokens.css) (palette · fonts · radii · shadows · motion) and
> [`mockups/components.css`](./mockups/components.css) (every component class). The fullest reference
> screen is [`mockups/03-public-web-view.html`](./mockups/03-public-web-view.html); the headline
> dashboard screen is [`mockups/07-shared-dashboard.html`](./mockups/07-shared-dashboard.html). The
> prose rationale lives in [`_design-brief.md`](./_design-brief.md). This section is a self-contained
> mirror so the doc can stand alone — if it ever disagrees with the CSS, the CSS wins.

### 9.1 Aesthetic direction

**Warm-paper lab-notebook meets a credible open-science publication.** Minimal-scientific, confident,
editorial. Generous negative space, a faint dotted grid on the page body, soft *layered* shadows (never
flat drop-shadows), hairline rules. The feeling to hold: *"a result, with just enough of its lineage,
standing on its own at a URL — trustworthy to a stranger."* Restraint and precision over decoration;
one memorable motion moment per screen, not many.

### 9.2 Type system — three families, each with a job

| Family | Token | Used for |
|---|---|---|
| **Fraunces** (serif) | `--font-display` | Headings, node titles, mastheads. Weights 400–700; optical sizing on. |
| **Hanken Grotesk** (sans) | `--font-body` | All UI and body text, buttons, labels. Weights 400–800. |
| **JetBrains Mono** | `--font-mono` | **Anything machine-facing** — pointers, URLs, JSON-LD, KOI RIDs, DOIs, version hashes, eyebrows, edge labels. Weights 400–600. |

Rule of thumb: **if a human wrote it, it's Fraunces or Hanken; if a machine emitted it, it's mono.**
`h1` is `clamp(28px, 4vw, 44px)`; `h2` 24px; `h3` 18px; body 15px / line-height 1.5. Loaded via one
Google Fonts `@import` at the top of `tokens.css` — don't add other fonts.

### 9.3 Colour — brand & chrome (warm paper + ink)

| Token | Hex | Role |
|---|---|---|
| `--ink` | `#1c2024` | Primary text (near-black, faintly warm) |
| `--ink-2` | `#3a3f47` | Body text on cards |
| `--muted` | `#6b7280` | Secondary text |
| `--faint` | `#9aa0aa` | Tertiary / metadata |
| `--paper` | `#faf8f4` | Page background (warm paper) |
| `--surface` | `#ffffff` | Cards / panels |
| `--surface-2` | `#f4f2ec` | Inset surfaces, code wells, pointer chips |
| `--line` / `--line-2` | `#e7e3d8` / `#ece9e0` | Hairlines |
| `--brand` | `#2f3a8c` | **MIRA accent** — primary actions, links, frozen-version pin |
| `--brand-press` | `#232c6e` | Primary pressed |
| `--brand-tint` | `#eceefb` | Brand wash (public-to-world pill, active preset) |

### 9.4 Colour — the node-type palette (FIXED & SEMANTIC — never recolour)

Each discourse-node type owns three tones: `-ink` (solid label / left border / badge text), `-wash`
(soft card fill), `-edge` (card border). **Colour carries meaning here** — it tells the reader what
*kind* of node they're looking at, so it is not a stylistic choice. Apply only via the component
classes (`.ntype.evidence`, `.node.study`, `.edge.grounds`, …).

| Type | `-ink` | `-wash` | `-edge` | Mnemonic |
|---|---|---|---|---|
| **Question** | `#c2861a` | `#fbf0d4` | `#e7cd8f` | gold |
| **Claim** | `#2c9a61` | `#ddf2e5` | `#a6dcbd` | green |
| **Evidence** | `#d9573f` | `#fbe0da` | `#f0b1a3` | coral |
| **Study** | `#3877cf` | `#dbe9fb` | `#a6c6ef` | blue |
| **Protocol** | `#8460c5` | `#ece2f8` | `#c9b3ea` | violet |
| **Request** | `#4854bf` | `#e2e3f7` | `#aeb4eb` | indigo *(the request-back current)* |
| **Source** | `#1b9a92` | `#d7f0ee` | `#98d9d3` | teal |

Status/signal reuse the same hues: `--ok` = claim green, `--warn` = question gold, `--danger` =
evidence coral, `--private` = `#8a8f98` (greyed "not shared").

### 9.5 Two chrome modes

- **In-app (Obsidian)** — add `.theme-obsidian` to a wrapper: dark editor pane (`--paper` → `#1e1e22`,
  `--surface` → `#26262b`, `--ink` → `#e6e6ea`), faux ribbon / file list, DG nodes sitting inside.
  Used by the *producer* screens (`01`, `06`).
- **Public web / standalone** — light paper theme + `.winbar` with a real address bar / journal-style
  masthead. Used by the *reader* screens (`03`, `04`, `05`, `07`, `08`, `09`).

### 9.6 Component vocabulary (reuse these — don't reinvent)

**Base kit** (`components.css`): `.node` + `.ntype` (a discourse node as a card + its type badge);
`.node.private` (dashed, greyed, "not shared"); `.btn` (`.primary` / `.ghost` / `.sm`); `.pointer`
with `.kind` chips; `.share-level`; `.edge` (typed relation labels); `.panel`, `.divider`, `.avatar`,
`.toast`, `.steps`, `.tag`, `.frame-caption` (+ `.eyebrow`), `.screen`, `.winbar`, `.kbd`; the
`.reveal .d1…d8` stagger + `.pulse`.

**Pointer kinds** (`.pointer .kind.*`) — the R2 primitive. `git` `#24292f` · `s3` coral · `local`
grey · `video` `#b3358f` · **`data`** green (curated CSV) · **`doi`** brand indigo (citable handle) ·
**`koi`** request-indigo (KOI RID / desci record). **Show pointer chips — never a raw-data download.**

**Dashboard atoms** (the pieces a dashboard prototype leans on most):

| Class | What it is |
|---|---|
| `.viewswitch` | Segmented control: **Graph · Kanban · Table**. Active tab gets a white surface + shadow-1. |
| `.preset` | Audience-preset chip (dashed → solid when `.active`): *Results* / *Experiments-in-progress (funder)* / *Questions & requests (recruiting)*. |
| `.dash-search` | The "everything in one place / searchable" bar (pill, search glyph). |
| `.request` / `.request.open` | The **request-back** chip (indigo); `.open` = unclaimed (dashed). |
| `.node.candidate` / `.ntype.candidate` | A provisional/informal node that can ride along, shared as-is. |
| `.cite` (+ `.frozen`) | "Cite this · version frozen" chip — the R9 pin (frozen text in brand indigo). |
| `.reuse` | "Cited by *N*" reuse signal (green dot, bold count). |
| `.share-level.*` | `world` (public, brand-tint) · `gated` (public page, restricted resource — gold) · `consortium` (password-protected, 20+ labs — request-indigo) · `public` (green) · `lab` (blue) · `private` (grey). |

**Cast avatars** (gradient discs, fixed per person): `.brian` blue→teal · `.morgan` green→blue ·
`.qbi` indigo→violet · `.world` grey (a stranger) · `.sean` blue→indigo · `.kate` green→teal ·
`.grad` teal→green (Kate's student) · `.vogel`/`.lab` lab marks. Keep a person's colour constant
across every screen.

### 9.7 Usage constraints — the rules the visuals must honour

These are *behavioural* constraints, not decoration; a prototype that breaks one is wrong even if it
looks right (rule IDs map to §2–§3 above and the design brief):

- **R2 · Pointers, not payloads.** Analysis = `git`+commit chip; data = curated-CSV chip; raw stacks =
  a *request-access* `local` chip. Never embed or offer a raw-data download.
- **R4 · Summary up front, then traverse.** Lead with the summary figure + one-line observation; make
  `Claim ← Evidence ← Study → Protocol` traversal available but not forced (PI glances; reproducer drills).
- **R5 · Permissions visible even when public, no leaks.** Gated resources read **"request access"**;
  a withheld note shows as **"1 note withheld"** and **must never leak its contents or a dangling
  reference**. The dashboard is **read-only** — show no edit affordances.
- **R6 · Addressable at a URL.** Show the `.winbar` address bar / a citable handle; the published
  record is a web page.
- **R8 · Trust via provenance.** Every result carries a byline — *author · lab · license* (e.g.
  *Brian & Morgan · Quantum Biology Institute · CC-BY*; *Sean · Vogel Lab · shared with Kate's lab*).
- **R9 · Freeze the cited version.** The `.cite` chip pins the exact version the reader saw, so later
  edits don't silently change what was cited.
- **Two currents on the dashboard.** Results flow out **and** a `Request` flows back — render the
  reverse current with the indigo `Request` colour + `.request` chip.
- **Every plot is labelled** *"illustrative · unpublished — workshop prototype."* Never invent
  contradicting science; use the worked example verbatim from `_design-brief.md` §4 (QBI MagLOV2) or
  the dashboard worked example (G3BP1 / PABP1 assembly order).
- **Every screen is captioned** — a `.frame-caption` with an `.eyebrow` (e.g. `MOCKUP 07 · SHARED
  DASHBOARD`), an `<h2>`, and one sentence naming the rule it embodies.

### 9.8 Accessibility & build constraints

- **Colourblind-safe and legible for "a 60-year-old-plus professor."** Don't rely on hue alone —
  node-type colour is always paired with a text label / badge, and the type's name is shown. Maintain
  the ink-on-wash contrast in §9.4.
- **Static-first.** Screens must look complete and correct **with JS disabled**; a tiny inline vanilla
  interaction is fine, but **no external JS frameworks**.
- **Motion is CSS-only and respectful.** Staggered `.reveal .d1…d8` on load + one signature moment
  (an edge drawing, a toast sliding in, a version pin snapping, kanban columns sliding in).
  `prefers-reduced-motion: reduce` is already handled — don't override it.
- **Self-contained & openable** by double-click: relative CSS hrefs, no build step, no network beyond
  Google Fonts. Centre content at ~1080px max; responsive down to ~720px is a plus.
- **Radii / shadows / motion tokens** are fixed: radii `--r-sm 6 · --r-md 10 · --r-lg 16 · --r-xl 22 ·
  --r-pill 999`; three layered shadows `--shadow-1/2/3`; easings `--ease` / `--ease-out`.

---

*MIRA × Discourse Graphs · publish to a public repository · 🥉 low-shared-context variant · draft v0.1*
