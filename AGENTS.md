# AGENTS.md — Push results to a shared web interface (dashboard → public)

> **Shared brief for everyone — humans and coding agents — building the MIRA "result on a shared web
> surface" prototype.** One spine, two worked examples:
>
> - **Central — the Sean → Kate dashboard.** A producer (**Sean**, Vogel Lab) pushes a *selected
>   subgraph* to a **read-only shared web interface — a dashboard/kanban** — that a known collaborator's
>   lab (**Kate**, a PI) navigates **in a browser, with no discourse-graph tooling**. Public for a lab
>   website, or **password-protected** for a consortium. This tier carries a **reverse current**: Kate's
>   lab can issue a **`Request`** back.
> - **The public extreme — Brian → the world.** Take the same machinery fully public: **Brian** (Quantum
>   Biology Institute) publishes a MagLOV2 result to a **public repository or database**, addressable at
>   a **URL**, for a stranger with **no relationship and no shared tooling**. One direction only.
>
> Both push to a **shared web interface** readable without a DG app; both obey the same R1–R13 rules and
> the same *pointers-not-payloads* discipline. They differ only in **how open** the surface is and
> **whether a `Request` flows back**.
>
> **Status:** Draft v0.1 · **Origin:** the Discourse Graphs team's inter-graph use-case work, the
> **Vogel × Kate** lab-collaboration sessions (Jun 2026), and the **Quantum Biology Institute** pilot.
>
> **Read this before proposing changes.** Rules in [§5](#5-rules--constraints-normative) are
> **normative** (MUST / SHOULD / MUST NOT). Items in [§10](#10-open-questions--not-decided) are
> **open** — do not implement them as if decided. When in doubt, the rules win.

---

## 0. How to use this file

- This repository captures **one spine — "push a result to a shared web interface" — in two worked
  examples**: the **central** Sean → Kate *dashboard* (a known collaborator/consortium, gated, with a
  request-back), and the **retained** Brian → *world* publication (fully public, one-directional).
- It is the **shared-web-surface** counterpart to [`lab-to-lab/`](./lab-to-lab/) (which covers importing into a
  *known collaborator's graph*). This file changes the **destination** (a shared web interface / public
  repo) and, at the collaborator tier, **re-activates the `Request`-back current** — not the data model.
- Companion artifacts in this repo:
  - **Central (Sean → Kate dashboard):**
    [`artifacts/_grounding-dashboard.md`](./artifacts/_grounding-dashboard.md) — the context pack;
    [`artifacts/ux-user-story-dashboard.md`](./artifacts/ux-user-story-dashboard.md) — the narrative;
    mockups [`06`–`09`](./artifacts/mockups/).
  - **Public extreme (QBI):** [`artifacts/_grounding.md`](./artifacts/_grounding.md);
    [`artifacts/ux-user-story.md`](./artifacts/ux-user-story.md); mockups [`01`–`05`](./artifacts/mockups/).
  - [`./low-context-user-story.png`](./low-context-user-story.png) — the user-story canvas
    (**Sean's notebook → Kate's lab → a shared web interface**);
    [`MyST-DG user story.png`](../myst-dg-interop/MyST-DG%20user%20story.png) — the adjacent MyST/Jupyter-Book variant.
- Canonical schema (separate repo, **not vendored**): **<https://github.com/MIRA-science/schema>** —
  LinkML `mira.yaml`. [§4](#4-data-model--schema) summarizes it; the repo is normative.

---

## 1. TL;DR — the user stories

> **Central — Sean → Kate's dashboard.** *Sean, a PhD student in the **Vogel Lab**, has a stress-granule
> result as markdown discourse-graph nodes in Obsidian: in stressed HeLa cells, **G3BP1 is recruited to
> forming granules before PABP1**. He wants **Kate's lab** to see it, trust it, and dig into it without
> installing anything and without a Zoom call — and **without** handing over his 80 GB of raw images or
> his private notes. **As a producer, Sean pushes a selected subgraph — the `Evidence` plus the
> `Study`/`Protocol` behind it, carrying pointers — to a read-only shared dashboard**, public or
> password-protected. **As readers, Kate (PI) glances at the summary and trusts it; her grad student
> traverses to the segmentation threshold and pulls the CSV; either can send a `Request` back** for a
> follow-up. Two currents: results out, requests back.*

> **Public extreme — Brian → the world.** *Brian (with PI **Morgan**) at the **Quantum Biology
> Institute** publishes a MagLOV2 magnetic-field-effect result — the sign of the effect flips as the
> field strengthens — to a **public repository or database** at a **URL**, so **anyone**, with no
> relationship and often no DG app, can read it cold, follow the pointers, request what's gated, and
> **cite the exact version**. One direction: producer → the public.*

What separates the two tiers:

1. **Openness.** The dashboard is **public OR password-protected (consortium)**; the QBI case is fully
   public. Both are readable **without a DG app**.
2. **The reverse current.** At the **dashboard tier, `Request` is first-class** ([R13](#5-rules--constraints-normative)) — Kate's lab requests follow-ups. The public tier is **one-directional**.
3. **Trust.** The dashboard reader is a **known collaborator** (relationship + provenance); the QBI
   reader is a **stranger** (provenance + reproducibility alone). Either way, **every node must stand on
   its own**.

---

## 2. Scope

### In scope (primary)
- **Pushing a self-describing bundle to a shared web interface** — a **read-only dashboard/kanban**
  (central) or a **public repository/database** (the extreme) — addressable at a **URL**
  ([§5 R1](#5-rules--constraints-normative), [R6](#5-rules--constraints-normative)).
- **Reading it cold with no DG tooling** — the node/board renders as a **web page** (markdown → HTML is
  fine for v0). The dashboard offers **Graph / Kanban / Table** views and **audience presets**.
- **Sharing a *subset of the vault*** — a selected subgraph, not the whole graph; with an offer to
  include connected nodes.
- **Pointers to analysis/data/provenance** (not payloads), resolving **across system boundaries**.
- **Permissions on a shared surface** — **public OR password-protected (consortium)**; deeper/restricted
  resources **request-access-gated**; **shared = visible, not editable** ([R5](#5-rules--constraints-normative)).
- **The reverse current** — at the collaborator/dashboard tier, a first-class **`Request`** back
  (for an experiment/analysis) with notification ([R13](#5-rules--constraints-normative)).
- **Citing & reusing** a published node, with the **exact version frozen** ([R9](#5-rules--constraints-normative)) — primarily the public tier.
- **Compiling many bundles into one narrative** ("specs instead of papers", [R11](#5-rules--constraints-normative)).

### Secondary (related, lower priority)
- **Authoring/automation** that lowers the cost of preparing a bundle — incl. **AI-proposed candidate
  nodes** and an **AI-drafted narrative**. A *separate track*; must never block the share/publish track.
  See [§7](#7-secondary-track-authoring--automation).

### Out of scope (for v1)
- **Writing/contributing *from* the dashboard.** It is a **read-only viewing/discovery surface**;
  authoring stays in the vault.
- **Expanding the node grammar** to `Analysis` / `ELN` / `Data` types — keep canonical `Evidence` +
  `Study` + `Protocol`; the richer reader-ladder mapping is **deferred** ([§10](#10-open-questions--not-decided)).
- **Non-text assets as first-class schema fields** (CSV / image / DNA) — a known **gap**, deferred.
- **Hosting or transferring raw data.** Large stacks live on a lab array / institute drive; we only
  **reference** them, and the reference may be access-gated.
- **Conflict resolution / graph merge** semantics. Design to *allow* it later; do not build it now.
- **A central consortium gatekeeper.** Out of scope (the inter-lab story records this too).

---

## 3. Actors & personas

The actors are the **scientists** who share and read. (Transport, schema, the dashboard build, and the
KOI sync plugin — the plumbing — are described as *components* in [§8](#8-architecture--stack), **not**
as people.)

### Central — the Sean → Kate dashboard
| Actor | Lab / role | Tool | In the story |
|---|---|---|---|
| **Sean** | Vogel Lab (bench, PhD) | Obsidian + DG plugin; 25 TB array | **Producer.** Authors the stress-granule graph; **selects the subgraph to share**; sets per-node visibility; keeps personal notes private. |
| **Kate** | Collaborating lab (PI; consortium lead) | a web browser | **Consumer (PI).** **Glances and trusts**; runs **many collaborators across projects**; wants everything **in one place, searchable, for years**. |
| **A grad student in Kate's lab** | Kate's lab | the dashboard | **Consumer (reproducer).** **Traverses to the segmentation threshold**; follows pointers; **requests access / a follow-up**. |
| **A public / funder visitor** | a recruit, a funder, a website visitor | the public dashboard | Sees an **audience preset** (questions/requests for recruiting; experiments-in-progress for a funder); needs a *plain-language* summary. |

### Public extreme — Brian → the world
| Actor | Lab / role | Tool | In the story |
|---|---|---|---|
| **Brian** | Quantum Biology Institute (bench) | Obsidian + DG plugin | **Producer.** Authors the MagLOV2 result and assembles the bundle. |
| **Morgan** | QBI (PI) | Obsidian / web | **Producer (PI).** Owns the decision to publish; wants it FAIR and interactive. |
| **A reader with NO DG app** | a reviewer, a clinician, anyone | a web browser | **Consumer.** Lands on the URL; reads cold; drills figure → analysis → data; requests what's gated. |
| **A citing / future-self reader** | any researcher | their own notebook | **Consumer.** Wants to **cite a specific result** and keep the **cited version frozen**. |

**Reader-dependent depth (the pivotal insight across every tier):** the *big idea up front* (claim + key
figure) for a glance; *traversal on demand* (Study → Protocol → data pointers) for someone checking or
reproducing — each level self-explanatory, because the reader fills in no context.

---

## 4. Data model & schema

The grammar is the **MIRA schema** (extends the Discourse Graphs core). Canonical source:
**<https://github.com/MIRA-science/schema>** (`mira.yaml`). We use the **same node & edge types as the
inter-lab story** — the destination changes, the grammar does not.

### 4.1 Node types
| Node | Definition | Notes |
|---|---|---|
| **Question** | A scientific unknown. | Centered in the dashboard's "questions/requests" preset. |
| **Claim** | An atomic assertion that (proposes to) answer a Question. | A Claim can sit **above several Evidence** bundles (the "model"). |
| **Evidence** | A specific empirical observation from one application of a method. | **Single type** — the shareable unit's hub. Key figure + observation + pointers. |
| **Study** | The research activity that produced the data artifact. | `prov:Activity`; the producers call it the **"experiment"**; carries artifacts **by reference**. |
| **Protocol** | The method the Study follows. | `prov:Activity`. **The segmentation threshold lives here.** |
| **Request** | A request for an experiment/analysis. | **First-class at the dashboard tier** ([R13](#5-rules--constraints-normative)); peripheral in the QBI case. |

> A **candidate** node (provisional/informal, pre-formalization) may be **shared as-is**; others can
> formalize it or view it. Whether the author must approve a formalization is **open** ([§10](#10-open-questions--not-decided)).
> The reader-ladder types the QBI pilot used (`Analysis` / `ELN` / `Data`) are **deferred** ([§2](#2-scope)).

### 4.2 Edge types
```
Claim     --addresses-->                Question
Evidence  --supports / opposes-->       Claim          # one Claim/model, many Evidence
Study     --grounds-->                  Evidence        # inverse: is_grounded_in
Study     --follows-->                  Protocol         # the canvas labels this "uses"; the LinkML slot is `follows`
Request   --request_for / request_target--> Study / Analysis   # the reverse current (Kate's lab → Sean) — dashboard tier
```

### 4.3 Serialization, addressability & interop
- **Local authoring format:** markdown pages in a discourse graph (Obsidian / Roam / MyST).
- **Interchange format:** **JSON-LD / RDF** — nodes reference external URLs (a git repo, a CSV, a data
  store, a Jupyter Book chapter) as first-class pointers; extensible to triples for nanopub/LOD.
- **Published / rendered forms (the deliverable):**
  - **A web page / dashboard with a URL** for the shared subset — the dashboard offers **Graph / Kanban /
    Table** views and **audience presets** (markdown → HTML acceptable for v0; v1 adds the composite view).
  - **A JSON record** packaged for **KOI** (URIs as **RID** or URL).
  - **RDF** for nanopublication / linked-open-data; **ATProto lexicon** where applicable (the QBI tier).
- **Extract graph elements *from* pages; don't compose narrative pages *from* the graph.** DG data is
  already structured (minimal transform); long-form docs from other tools must be chopped into nodes first.
- The transport must be **schema-agnostic** ([§5 R10](#5-rules--constraints-normative)).

---

## 5. Rules & constraints (normative)

> Engineers and agents: treat these as acceptance constraints. Cite them in PRs (e.g. "satisfies R5").
> R1–R12 are stable and cited throughout the mockups; **R13** is the dashboard tier's reverse current.

- **R1 — Push to a shared web interface.** The default act is **making a bundle available & addressable
  on a shared web surface**: a **read-only dashboard** (central) or a **public repo/DB** (the extreme),
  with a **stable URL**. Support more than one destination (DG dashboard, public web DB, Jupyter Book,
  desci/KOI record, nanopub) behind one action, and **both public and password-protected** surfaces.
- **R2 — Pointers, not payloads.** Nodes MUST reference analysis/data/media by pointer (git+commit+path;
  HTTP/S3 URI; curated-dataset/CSV link; freeform markdown link). Raw data (image stacks, ~80 GB) MUST
  NOT be embedded or transferred; the raw-data pointer MAY resolve to an **access-gated** archive.
- **R3 — One `Evidence` type; do not expand the grammar in v1.** Keep canonical MIRA node types
  (`Evidence` / `Study` / `Protocol`, plus `Question`/`Claim`/`Request`). Do **not** add `Analysis` /
  `ELN` / `Data` node types in this prototype (deferred — [§10](#10-open-questions--not-decided)).
- **R4 — Self-describing bundle, readable on its own.** The shareable unit is **`Evidence` + its
  grounding `Study` + that Study's `Protocol`** (plus pointers). Each node MUST be **interpretable on its
  own** — a header/summary "like a paper's methods section." **Summary up front; lineage on demand.**
- **R5 — Permissions on a shared surface; start closed.** Sharing a node is deliberate. A surface can be
  **public OR password-protected (consortium)**, and can still gate deeper resources (the raw stack)
  behind **request-access**. **Shared nodes are visible but NOT editable** by the receiving lab. The
  system MUST support a **request-access** path and MUST NOT leak the existence/metadata of un-shared or
  gated nodes — including when data is routed through KOI ("private must not become public via KOI").
- **R6 — Addressability is the deliverable.** Every shared node/board MUST be reachable at a **URL** and
  rendered as a **web page** (markdown → HTML ok for v0). **No DG app may be required to *read* it**, and
  the dashboard is **read-only** (a viewing/discovery surface, not an authoring tool).
- **R7 — Resolve pointers across system boundaries.** The shared `Evidence` is the **hub record**; its
  relations/pointers MAY point to **URLs in other systems** (GitHub, a data array, a Jupyter Book
  chapter), not only to records in the same store. Build for heterogeneous resolution.
- **R8 — Trust through visible provenance.** The page/card MUST show **who shared it, which lab, and the
  terms** (license / sharing scope), so a reader can judge it. Provenance is part of the payload.
- **R9 — Freeze the cited version.** When a reader cites/links a published node, they MUST be able to
  reference the **exact version they saw** (a frozen snapshot / pinned commit / versioned handle),
  preserved independent of later edits. (Primarily the public tier.)
- **R10 — Schema-agnostic transport; JSON-LD on the wire.** The transport layer MUST be decoupled from
  the schema (changing MIRA MUST NOT break sharing). Use JSON-LD/JSON as the working format; do NOT
  require collaborators to run a triple store. **KOI** is a structured interoperability layer between
  MIRA-compatible tools — "a specific expressway," not the open internet.
- **R11 — Publish-as-you-go, then compile; preserve intent.** Individual bundles are shared first and
  MUST remain **independently addressable** after being compiled into a longer narrative ("specs instead
  of papers"). A narrative MAY be **AI-drafted** but MUST be **human-edited**; the structured bundles are
  the source of truth, the prose is one rendering. Likewise AI MAY propose **candidate** nodes; a human
  keeps what's relevant (don't lose intent to automation).
- **R12 — Connect to publishing & provenance substrates, don't absorb them.** KOI, the dashboard web
  app, desci/ATProto, nanopub services, Jupyter Book/MyST, and PROV-O live in **adjacent layers** the
  graph **points to**, not inside the discourse-graph reasoning layer.
- **R13 — Support the reverse current at the collaborator tier.** On a shared dashboard between known
  collaborators, a first-class **`Request`** MUST be expressible (`request_for` / `request_target` →
  a follow-up `Study`/analysis), with **notification** to the producer and the ability to **claim** an
  open request. (At the fully-public tier this current is weak/absent; do not require it there.)

---

## 6. The lifecycles

The collaboration primitive differs by tier (compare the inter-lab story's `Request` lifecycle):

### 6a. Share-to-dashboard (central — Sean → Kate)
1. **Author** — Sean captures `Question → Claim → Evidence → Study → Protocol` as markdown DG nodes
   frictionlessly in Obsidian (~10s per experiment).
2. **Select the subgraph** — choose the `Evidence`; the plugin proposes the bundle (`Evidence` + `Study`
   + `Protocol` + pointers) and **offers connected nodes**. It is a **subset of the vault.**
3. **Set visibility & audience** — per-node **public / consortium / private**; raw stacks
   **request-access-gated**; personal notes **withheld** without leaking; choose **public** vs
   **password-protected** ([R5](#5-rules--constraints-normative)). A **candidate** node may ride along.
4. **Push** — export as JSON-LD; **KOI** carries it to the **read-only dashboard**; shared nodes arrive
   **visible, not editable**.
5. **Read on the dashboard** — Kate's lab opens a URL in a browser, picks a **view/preset**, glances and
   trusts, or **traverses** to the method.
6. **Request back** — Kate's lab **claims** or **sends a `Request`** for a follow-up; Sean is notified
   ([R13](#5-rules--constraints-normative)).

### 6b. Publish-to-the-world (the extreme — Brian → QBI public)
1–2. Author and **select the bundle** as above. 3. **Set public / gated.** 4. **Add context &
format-in-schema** (summary, methods table, walkthrough). 5. **Publish to destination(s)** — KOI mediates
to a public web rendering, a **Jupyter Book** micropublication, and/or a **desci/nanopub** record.
6. **Get an addressable handle** — a **URL** (+ a citable/version-frozen handle). 7. **Reuse** — readers
**cite the frozen version**, **import** it, or **request access**. 8. **Notify & attribute** — the
producer sees **who is citing/using** the node.

---

## 7. Secondary track: authoring & automation

Goal: make preparing a bundle feel **frictionless** so it rides the producer's normal workflow
(per [R11](#5-rules--constraints-normative), intent-preserving):

- A **"document/pub" container that behaves like a README** — one linkable thing pointing to a whole
  package of analysis + data files (the QBI pilot's emergent pattern).
- **Candidate nodes** as the baby step: start informal, share as-is, formalize later; **AI may propose
  candidate nodes and edges** (e.g. by crawling a repo / Snakemake / Nextflow) — but a human keeps intent.
- On share/publish, **prompt** for a summary / methods table / **video walkthrough** and offer to
  **format-in-schema**.
- **AI-drafts the narrative** from a *collection* of bundles + the spec; a human edits. Bundles remain
  the source of truth.

Build this so it **exports in the shared serialization** ([§4.3](#43-serialization-addressability--interop)).

---

## 8. Architecture / stack

Disambiguate the layers (do not "tape them together"):

| Layer | What | Notes |
|---|---|---|
| **Interface** | Discourse Graph plugin in **Obsidian** (Roam/MyST elsewhere) | Where the producer authors & **selects the subgraph**. |
| **Schema** | **MIRA** (`Question`/`Claim`/`Evidence`/`Study`/`Protocol`/`Request`) | Canonical, separate repo. |
| **Infrastructure** | **Supabase** | Current Discourse Graphs backend / DB. Backend already supports lab-to-lab sharing (**shared = visible, not editable**). |
| **Transport / mediation** | **KOI** (+ a vault-to-vault sync plugin) | Decentralized; **RIDs** (Reference IDs = pointers); "a specific expressway," not the open internet; each org manages its own relationships. Permissions currently **weak** — gating ([R5](#5-rules--constraints-normative)) needs work on top; **private MUST NOT leak through KOI**. |
| **Shared web interface** | the **dashboard web app** | **Read-only**; **Graph / Kanban / Table** views; **audience presets**; **public or password-protected**; accessible (colorblind-safe, legible). *"A small app that gets MIRA notes from KOI and renders a board."* |
| **Public destinations** | DG **web DB** (Semble), **Jupyter Book / MyST**, **desci / ATProto**, **nanopub** (Cosmik, experiment.com), **PREreview** | The fully-public end. Relations point to **URLs** across these ([R7](#5-rules--constraints-normative)). |
| **Domain / provenance ontologies** | **PROV-O** | *Connected, not absorbed* ([R12](#5-rules--constraints-normative)); supports invalidate-and-recompute when a method changes. |

### The source / transport / destination matrix (from the canvas)
| | **Producer / source** | **Transport (middle)** | **Shared web interface / destination** |
|---|---|---|---|
| **location** | local Obsidian vault | `export` → **KOI** → DG Supabase | a **read-only dashboard** (or public repo/DB) at a URL |
| **format** | markdown in a discourse graph | **JSON-LD / JSON (RID/URL)** | **web page (HTML)** · Graph/Kanban/Table · JSON record · RDF/nanopub |
| **feels like** | frictionless · rides my workflow · ~10s/experiment | a specific "expressway" | everything in one place · searchable · durable for years |
| **features** | select subgraph · offer connected nodes · set per-node visibility & audience · candidate nodes | mediate flow · mint URL/handle · freeze version · private-not-leaked | views & presets · glance-and-trust · traverse-to-method · **request back** · request access · cite (frozen) · import-if-you-DG |
| **schema** | bundle = `Evidence` + `Study` + `Protocol` + pointers; **`Request`** for the reverse current | MIRA · schema-agnostic transport | provenance + sharing scope on every node |

---

## 9. Prototype scope & acceptance criteria

**Canonical test cases.** (1) **Sean → Kate (central):** the Vogel-lab stress-granule result —
*G3BP1 is recruited to forming granules before PABP1* — live two-channel imaging of stressed HeLa cells;
analysis + curated CSV on hand; **raw NDTiff stacks ≈ 80 GB on a 25 TB array**. (2) **QBI (extreme):** the
MagLOV2 magnetic-field-effect sign-flip. Both are **real** and **unpublished** (usable for prototyping,
not redistribution; mockup figures labelled *illustrative · unpublished — workshop prototype*).

**v1 is done when** (dashboard track — see also [`ux-user-story-dashboard.md` §6](./artifacts/ux-user-story-dashboard.md)):
1. **Share path** — Sean pushes a **selected subgraph** (`Evidence` + grounding `Study` + `Protocol`,
   each with a **pointer**) from Obsidian to a **read-only dashboard at a stable URL**, with per-node
   visibility (raw stacks gated; personal note **withheld, not leaked**).
2. **Glance path** — Kate opens the dashboard in **a browser, no DG app**, picks a **view/preset**, and
   reads the result cold (summary + provenance).
3. **Traverse path** — a grad student follows the lineage to the **segmentation threshold**, resolves the
   `git`/`data` pointers across system boundaries, and watches the **video walkthrough**.
4. **Gate path** — raw stacks are **request-access-gated**; access can be requested; **no private/un-shared
   node leaks** (incl. through KOI).
5. **Request path** — Kate's lab **sends a `Request`** for a follow-up; Sean is notified and can **claim**
   it ([R13](#5-rules--constraints-normative)).
6. **Public/consortium path** — the same dashboard runs **password-protected** for the consortium and
   **public** as a lab website, with **audience presets**.

**And (QBI / public track):** publish a bundle to a **public repo/DB with a URL**, readable cold; raw data
**gated**; **cite the frozen version**; **compile** several bundles into one micropublication while each
stays independently addressable. (See [`ux-user-story.md` §6](./artifacts/ux-user-story.md).)

**Task ladder (from the canvas):** paper prototypes → mock interfaces & flows → export/publish → **dashboard/kanban + permission/account + public URL** → PoC of *share + render + gated-access + request/cite*.

---

## 10. Open questions — NOT decided

> Do not implement these as settled. Surface them in design discussions.

- **Candidate-node formalization flow.** When someone else formalizes your shared **candidate** node, do
  you approve / get notified? (Open.)
- **Non-text assets in the schema.** How CSVs / images / DNA files become **referenceable schema fields**
  rather than embedded content (a flagged **gap**).
- **Accounts & gating atop weak KOI.** How password-protected consortium views and per-resource
  request-access are enforced given KOI's weak native permissions.
- **Preset-view set.** Which audience presets ship (results / experiments-in-progress / questions-&-requests),
  and whether they're true presets or saved layouts.
- **Reader-ladder ↔ schema mapping.** How the QBI pilot's `EVD → Analysis → ELN → Data` maps onto
  canonical `Evidence`/`Study`/`Protocol` (and what is lost on publish). **Deferred for v1.**
- **Dataset archiving.** *How/whether* to archive & share the curated/raw data — **unresolved by the
  pilot itself**. What does the data pointer resolve to, and who gates it?
- **Conflict / merge.** When two labs author independently and need to reconcile graphs.
- **Figure-as-addressable-artifact.** How a sub-panel of a figure becomes a node with its own ID/URL.
- **Dangling references: signal vs. hide.** Carry a relation to a missing resource as a *signal*, or
  *hide* it until resolvable?
- **Versioning mechanics & invalidation.** How the frozen-version guarantee ([R9](#5-rules--constraints-normative))
  is implemented, and how a downstream **recompute** is triggered when a method (e.g. the segmentation
  threshold) changes.
- **Provenance boundary.** How much pipeline provenance (PROV-O / Snakemake / Nextflow) lives *in* the
  graph vs. in an external tool it points to.
- **Cross-tool validation.** Obsidian ↔ Jupyter Book/MyST ↔ Roam ↔ ATProto ↔ another platform (e.g.
  Nucleus/MIST) via the JSON-LD/JSON bridge.

---

## 11. References

- Context packs: [`artifacts/_grounding-dashboard.md`](./artifacts/_grounding-dashboard.md) (Sean → Kate,
  central) and [`artifacts/_grounding.md`](./artifacts/_grounding.md) (QBI). Narratives:
  [`artifacts/ux-user-story-dashboard.md`](./artifacts/ux-user-story-dashboard.md),
  [`artifacts/ux-user-story.md`](./artifacts/ux-user-story.md). Visual brief:
  [`artifacts/_design-brief.md`](./artifacts/_design-brief.md).
- Dashboard sessions (distilled in `_grounding-dashboard.md`; raw transcripts stay local per
  [`.gitignore`](./.gitignore)): **"Collaboration between labs experiment"** (Jun 8, 2026 — Sean demos),
  **"User story clarification session"** & **"Dashboard collaboration ideas"** (Jun 9, 2026).
- Sibling story (importing into a *known* graph; the same Sean → Kate cast, the shared rules this varies):
  [`lab-to-lab/AGENTS.md`](lab-to-lab/AGENTS.md).
- Roam (dg-team): `[[ISS]] - Document initial inter-graph use cases`; the bronze node
  `🥉Low shared context: push result to a public repository or database`; `UserPilot/Quantum biology
  institute`; `Initiative/Inter-graph functionality` ("each DG node displayable as a web page with a
  URL"; "markdown rendered as HTML is ok for v0"); `[[FLO]] - Push node to shared database`.
- Canvas: [`./low-context-user-story.png`](./low-context-user-story.png) (Sean's notebook → Kate's lab →
  shared web interface). Adjacent: [`../myst-dg-interop/`](../myst-dg-interop/) (Jupyter Book / MyST);
  [`../schema/`](../schema/) (`mira.yaml`).
- Schema source repo: <https://github.com/MIRA-science/schema>. KOI: <https://github.com/BlockScience/koi>.
- MIRA: <https://www.mira.science> · Discourse Graphs: <https://discoursegraphs.com> ·
  Quantum Biology Institute: <https://www.quantumbiology.eco>.

---

*MIRA × Discourse Graphs · push results to a shared web interface · Sean → Kate dashboard (central) + QBI → world · draft v0.1*
