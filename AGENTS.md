# AGENTS.md — Inter-lab exchange of results & requests

> **Shared brief for everyone — humans and coding agents — building the MIRA inter-lab sharing prototype.**
>
> **Status:** Draft v0.1 · **Origin:** MIRA workshop, *"Collaboration between labs experiment"* working session, 2026-06-08.
>
> **Read this before proposing changes.** Rules in [§5](#5-rules--constraints-normative) are **normative** (MUST / SHOULD / MUST NOT). Items in [§10](#10-open-questions--not-decided) are **open** — do **not** implement them as if they were decided. When the transcript and this file disagree, this file wins (it encodes decisions made *after* the discussion).

---

## 0. How to use this file

- This repository captures **one** user story: **inter-graph exchange of results and requests between collaborating labs.** This file is its source of truth.
- It exists so that engineers and their coding agents in *different* labs can build interoperable pieces from one shared context, vocabulary, and set of constraints.
- Companion artifacts in this repo:
  - [`user-story-tldraw.png`](./user-story-tldraw.png) — the user-story canvas (the source/transport/destination matrix below is transcribed from it).
  - [`Schema diagram.png`](./Schema%20diagram.png) — the MIRA node grammar with a worked DNA-discovery example.
  - The source discussion (verbatim Granola transcript, 2026-06-08) is held **locally only** and intentionally **not committed** here — it covers unpublished data.
- Canonical schema (separate repo, not vendored here): **<https://github.com/MIRA-science/schema>** — LinkML source ([`mira.yaml`](https://github.com/MIRA-science/schema/blob/main/mira.yaml)) generating [`mira.ttl`](https://github.com/MIRA-science/schema/blob/main/mira.ttl) / [`mira.jsonld`](https://github.com/MIRA-science/schema/blob/main/mira.jsonld). Treat that repo as the normative schema; [§4](#4-data-model--schema) is a summary, not a replacement.

---

## 1. TL;DR — the user story

> *Sean (Vogel Lab) has an experimental result living as markdown nodes in his personal Obsidian discourse graph on his laptop. Kate's lab wants it. **As a data producer, Sean wants to share a small, self-describing subgraph — the result plus the experiment and protocol that produced it — with Kate's lab, at a granularity and permission level he controls, such that Kate's lab can import it into their own graph and trace back to the underlying data via pointers. As a data consumer, Kate's lab wants to receive results, get notified of updates, and issue requests for experiments/analyses that don't exist yet** — which the producing lab (or another collaborator) can claim and fulfil.*

Two directions of exchange — both are first-class:

1. **Results** flow producer → consumer (Sean → Kate): a subgraph of `Evidence` + `Study` + `Protocol`, carrying **pointers** to data/code/video, not the data itself.
2. **Requests** flow consumer → producer (Kate → Sean): a `Request` node ("we need this experiment/analysis") that a collaborator can **claim** and point a new `Study`/`Evidence` at.

---

## 2. Scope

### In scope (primary)
- **Peer-to-peer (P2P) sharing.** Each lab/org mediates its **own bilateral relationships**; there is **no central gatekeeper**. (This is the explicit decision — see [§5 R1](#5-rules--constraints-normative). The "consortium / central-PI-as-project-manager" model raised in the session is **out of scope** for this prototype.)
- **Permissions**, designed in from the start — they belong to the sharing track, not a later add-on.
- **The data model** ([§4](#4-data-model--schema)) and its **JSON-LD/RDF serialization** as the interchange format.
- **Pointers to data/code/provenance** (not payloads).
- A **`Request`** node type and its lifecycle (create → discover → claim → fulfil → notify).

### Secondary (related, lower priority)
- **Lowering the barrier to authoring nodes** (automation + LLM-assisted, intent-preserving capture). Real and on the canvas, but a *separate track*; build it so it never blocks or breaks the sharing track. See [§7](#7-secondary-track-authoring--automation).

### Out of scope (for v1)
- Hosting or transferring **raw data** (microscopy stacks are 80 GB+; stored on lab arrays / S3 / hard drives). We only ever **reference** it.
- **Conflict resolution / graph merge** semantics (deprecate-and-merge, `owl:sameAs`, edge-union). Design the model to *allow* it later; do not implement it now. See [§10](#10-open-questions--not-decided).
- Centralized consortium permission orchestration / milestone-gated bilateral unlocking.
- A bespoke triple store. (RDF is the *conceptual* model; JSON-LD is the *working* format — see [§5 R7](#5-rules--constraints-normative).)

---

## 3. Actors & personas

The user story is about **collaborating labs exchanging data.** The actors are the scientists who produce and consume it. (Infrastructure and tooling — transport, automation — are described as *components* in [§8](#8-architecture--stack), not as people.)

| Actor | Lab / role | Tool | In the story |
|---|---|---|---|
| **Sean** | Vogel Lab (cell biology) | Obsidian + Discourse Graph plugin | **Producer.** Owns the HeLa stress-granule result, experiment, protocol; controls what/how he shares. |
| **Kate** | PI, collaborating lab | Obsidian *or Roam — TBD* | **Consumer.** Wants results "in one place, any time," update notifications, and to issue requests. |
| **Anton** | Synthetic biology | **MyST** (MyST Markdown); data on S3, GenBank files | Cross-domain collaborator; wants synbio ↔ cell-bio compatibility; champions a domain ontology layer (SBOL). |

**Recipient-dependent depth (key persona insight):** a **PI** typically needs only the summary `Evidence` (≈ one figure panel / one slide) to trust and act. A **grad student** wants to traverse back to the `Study`, `Protocol`, and data pointers — and only needs raw data if they intend to *re-analyze*. The send mechanism must serve both from the same shared subgraph (summary up front, traversal on demand).

---

## 4. Data model & schema

The grammar is the MIRA schema, which builds on the Discourse Graphs core and is being adopted *by* Discourse Graphs. Canonical source: **<https://github.com/MIRA-science/schema>** ([`mira.yaml`](https://github.com/MIRA-science/schema/blob/main/mira.yaml)). Visual: [`Schema diagram.png`](./Schema%20diagram.png).

### 4.1 Node types

| Node | Definition | Notes |
|---|---|---|
| **Question** | A scientific unknown we want to make known. | a.k.a. research goal / research unknown. |
| **Claim** | An atomic, generalized assertion that (proposes to) answer a Question. | a.k.a. hypothesis. |
| **Evidence** | A specific empirical observation from one application of a research method. | **Single node type — see [§5 R3](#5-rules--constraints-normative).** Covers both *own experimental results* and *observations drawn from the literature*. (The earlier "Result vs. Evidence" split is **collapsed**.) Sub-fields: observation statement, data artifact (pointer), context. |
| **Study** | A research activity/experiment that produced a data artifact. | a.k.a. experiment / investigation / event. Modeled as `prov:Activity`. Carries artifacts (data, software) **by reference**. |
| **Protocol** | The method/approach a Study uses to generate Evidence. | a.k.a. method. Modeled as `prov:Activity`. |
| **Request** | A requested-but-not-yet-existing experiment/analysis. | Fields: *motivation*, *skill required*. The collaboration primitive — see [§6](#6-the-request-lifecycle). |

### 4.2 Edge types

```
Claim       --addresses-->        Question
Evidence    --supports / opposes--> Claim        # Evidence is an "Argument"
Study       --grounds-->          Evidence       # inverse: is_grounded_in
Study       --follows-->          Protocol       # the canvas/diagram labels this edge "uses"; the LinkML slot is `follows`
Request     --request_for-->      Study          # the study the request asks to be done
Request     --request_target-->   Claim          # the claim the requested study would illuminate
```

Evidence additionally carries (from the DG core): `observationStatement` (→ Claim), `observationOriginActivity` (→ Study/Activity), `observationBase` (→ the data Entity), `sourceDocument` (→ a source document, for literature-derived Evidence).

### 4.3 Serialization & interop
- **Local authoring format:** markdown pages in a discourse graph (Obsidian / Roam / MyST).
- **Interchange format:** **JSON-LD / RDF.** Chosen because (a) it lets nodes reference external URLs (S3, GitHub) as first-class pointers, (b) a node decomposes into triples so individual fields (author, date, content) are themselves linkable — making the format **extensible** (add fields/metadata later without breaking consumers) and enabling future `owl:sameAs`-style merges.
- The transport must be **schema-agnostic**: see [§5 R6](#5-rules--constraints-normative).

---

## 5. Rules & constraints (normative)

> Engineers and agents: treat these as acceptance constraints. Cite them in PRs (e.g. "satisfies R2").

- **R1 — P2P, no central authority.** Sharing MUST be modeled as bilateral relationships each org controls. Do NOT introduce a central gatekeeper, broker-of-record, or single permission authority.
- **R2 — Pointers, not payloads.** Graphs are excellent for *which data produced which conclusion* and terrible for *storing data*. Nodes MUST reference data/code/media by pointer (git repo + commit + path; S3/HTTP URI; or a freeform markdown link). Raw data MUST NOT be embedded or transferred by this system.
- **R3 — One `Evidence` node type.** Do NOT model a separate "Result" type. `Evidence` covers both own-experiment observations and literature observations; distinguish provenance via fields/edges (`is_grounded_in` a Study vs. `sourceDocument`), not via a new node type.
- **R4 — "Send" = a small, self-describing subgraph.** The default shareable unit is **`Evidence` + its grounding `Study` + that Study's `Protocol`** (plus pointers). Each node MUST be **interpretable on its own** (a header/summary so a recipient can read it cold — "like the methods section of a paper"), with deeper context reachable by traversal. Summary up front for the PI; traversal to Study/Protocol/data for the grad student.
- **R5 — Permissions first, start closed.** Permissioning MUST be designed in from the start (it is far harder to close an open system than to open a closed one). Support **node-level** privacy (e.g. a personal-thoughts node stays private) and **subgraph-level** sharing, with a **per-project default share**. Honor **progressive disclosure**: sharing a node forces a decision about whether to share its attached relations — and a shared relation can *imply the existence of* an un-shared node. The system MUST NOT leak the existence/metadata of un-shared nodes through dangling references.
- **R6 — Schema-agnostic transport.** The sharing/transport layer MUST be decoupled from the schema: changing the MIRA schema MUST NOT break sharing. Build to the serialization contract ([§4.3](#43-serialization--interop)), not to hard-coded node shapes.
- **R7 — JSON-LD on the wire; RDF as the model.** Use JSON-LD as the working interchange format (treatable as plain JSON, expandable to RDF triples). Do NOT require collaborators to operate a triple store or hand-author RDF.
- **R8 — Human-readable vs. machine-readable.** Encode something in the hard schema only when a *tool* must understand it. Prose/markdown is fine for human-only context. Don't over-formalize fields that no tool consumes.
- **R9 — Connect to domain ontologies, don't absorb them.** Provenance of *how data was made* (PROV-O) and domain specifics (e.g. SBOL for synthetic-biology DNA constructs) live in **adjacent/lower layers** that MIRA **points to**, not inside the discourse-graph reasoning layer. (Discourse graphs answer "what question / what claim"; PROV-O answers "what software/protocol produced this artifact.")
- **R10 — Preserve intent; no fully-automatic capture.** Authoring automation ([§7](#7-secondary-track-authoring--automation)) MUST keep the human's explicit declaration of relations in the loop (auto-suggest → user confirms). End-to-end automation that strips intent is a non-goal — the act of distinguishing observation/claim/evidence is part of the scientific value.

---

## 6. The Request lifecycle

A `Request` is the **collaboration primitive**: a placeholder for something that **does not exist yet** but is needed for the research. It is distinct from a comment/annotation (which describes something that *does* exist).

1. **Create** — the consumer creates a `Request` (fields: *motivation*, *skill required*), with `request_target` → the `Claim` it would illuminate (and optionally `request_for` → the `Study` it asks to be done).
2. **Discover** — collaborators (identified or not-yet-identified) can see open Requests addressed to them / their project and get **notified**.
3. **Claim** — a lab member with the matching skill **claims** the request (kinds: experiment, analysis, lit-analysis).
4. **Fulfil** — claiming, when done, **generates** a new `Study` → `Evidence`; the new node **points back at** the Request.
5. **Notify** — the requester gets a **notification of updates**; the resulting subgraph flows back via the normal results path ([§1](#1-tldr--the-user-story)).

> Decided: keep `Request` a distinct type meaning *"this doesn't exist yet — a prompt to extend the graph in a desired direction."* Do **not** merge it into a generic comment/annotation node. (Permission-requests and data-requests are *different* uses and are not what `Request` models — see [§10](#10-open-questions--not-decided).)

---

## 7. Secondary track: authoring & automation

Goal: make capturing nodes feel **frictionless / self-explanatory / passive** so adoption doesn't stall on click-overhead. Approach (intent-preserving, per [§5 R10](#5-rules--constraints-normative)):

- Point a tool at an existing repo; crawl to detect structure (e.g. a Python script reads a CSV; Snakemake/Nextflow steps) and **propose** nodes/edges.
- Feed gathered context to an **LLM that asks the user questions** (typed or spoken) and constructs nodes from the answers — instead of the user working a mental checklist.
- On share, **prompt** the user to add a summary/context or record a short walkthrough video, and to **format-in-schema** ("here's what I generated; ask me how to present it").

Build this so it **exports nodes/edges in the shared serialization** ([§4.3](#43-serialization--interop)) — i.e. the two tracks stay compatible. This was flagged as easy to forget; make it explicit.

---

## 8. Architecture / stack

Disambiguate the layers (do not "tape them together" implicitly):

| Layer | What | Notes |
|---|---|---|
| **Interface** | Discourse Graph plugin in **Obsidian** (and Roam/MyST for other labs) | Where each lab actually authors nodes. |
| **Schema** | **MIRA** (this story's vocabulary) | Being adopted by Discourse Graphs. |
| **Infrastructure** | **Supabase** | Current Discourse Graphs backend / database storage. |
| **Transport / mediation** | **KOI-net** (`koi-obsidian-plugin`) | BlockScience/Metagov/RMIT *Knowledge Organization Infrastructure* network protocol. Decentralized, bilateral; has an existing Discourse Graphs integration. Uses **RIDs** (Reference Identifiers = pointers to referents) — aligns with [R2](#5-rules--constraints-normative). **Note:** KOI's permission model is currently weak — permissioning ([R5](#5-rules--constraints-normative)) likely needs work *on top of* it. |
| **Domain / provenance ontologies** | **PROV-O** (provenance), **SBOL** (synbio) | *Connected, not absorbed* ([R9](#5-rules--constraints-normative)). |

### The source / transport / destination matrix (transcribed from the canvas)

| | **Sean / source** | **Transport (middle)** | **Kate / destination** |
|---|---|---|---|
| **location** | local HD | `export edges` → **KOI-net** → ; DG Supabase | (imported into her graph) |
| **format** | markdown in discourse graph | **JSON-LD, RDF** | markdown in discourse graph |
| **feels like** | frictionless · self-explanatory · passive | — | — |
| **features** | share subgraph · offer to share · connected/related nodes · prompt to add context/summary · prompt to format in schema | draw relation to node from other lab · manage permissions · disambiguate / merge pages | notification of updates · auto-summary · import connected nodes · **claim a request** (expt / analysis / lit-analysis) · add commentary · **send a request** (expt / analysis) |
| **schema** | subgraph permissioning · default share for the project | **MIRA schema + Request** · SBOL ontology (synbio) · PROV-O (how data was generated) | |

---

## 9. Prototype scope & acceptance criteria

**Canonical test case:** Sean's HeLa stress-granule result (two stress-granule proteins tracked by fluorescence microscopy; one intensity rises, the other rises later). It exists today as a real subgraph in Sean's Obsidian DG; he has the Python scripts and a 5 TB drive on hand. (Data is **unpublished** — usable for prototyping at the workshop, not for redistribution.)

**v1 is done when:**
1. **Results path** — Sean shares a subgraph (`Evidence` + grounding `Study` + `Protocol`, each carrying a data/code **pointer**) from his Obsidian DG; it travels over KOI-net as JSON-LD; **Kate's lab imports it** into their own graph and can **traverse back** from the Evidence to the Study, Protocol, and the data pointer.
2. **Requests path** — Kate creates a `Request` (an experiment/analysis that doesn't exist yet) targeting a Claim; Sean's lab **sees** it, can **claim** it, and points a new node at it; Kate gets a **notification**.
3. **PoC of the hard mechanics** — **permissioning**, **push / pull**, and **make-edge** (draw a relation from one lab's node to the other lab's node) across the two graphs.

**Task ladder (from the canvas):** paper prototypes → mock interfaces & user flows → export functionality → PoC of *permission + push/pull/make-edge*.

---

## 10. Open questions — NOT decided

> Do not implement these as settled. Surface them in design discussions.

- **Exact "send" closure.** [§5 R4](#5-rules--constraints-normative) fixes the default bundle (Evidence + Study + Protocol). Still open: how much *additional* connected context auto-travels, how the recipient pulls a referenced-but-not-included node (e.g. a Protocol two hops away), and how this varies by recipient role.
- **Permission granularity model.** Node vs. subgraph vs. project, and how progressive disclosure is enforced in practice (especially KOI's weak native permissions).
- **Conflict / merge semantics.** When two labs evolve overlapping graphs independently: the proposed approach is to mark both claims deprecated, mint a merged claim, **union the edges**, and keep deprecated nodes greyed-out for attribution; plus RDF `owl:sameAs`. **Deferred** — the model must *allow* it; don't build it in v1. Also unresolved: two labs adding *different field values* (e.g. a second author) to a shared node.
- **Pointer resolution for local data.** S3/GitHub pointers resolve cleanly across labs; **local-disk** pointers do not. How does a recipient resolve a pointer to data that lives only on a producer's array?
- **Provenance boundary.** How much pipeline provenance (PROV-O / Snakemake / Nextflow) lives *in* the graph vs. in an external tool the graph points to. ([R9](#5-rules--constraints-normative) sets the direction; the exact line is unsettled.)
- **Request, other readings.** Permission-requests ("a PI requests her postdoc be granted access") and data-requests ("send me that dataset") came up but are **not** what `Request` models ([§6](#6-the-request-lifecycle)). Whether/how to support them is open.
- **Kate's tool** (Obsidian vs. Roam) and general cross-tool validation (Obsidian ↔ Roam ↔ MyST) via the JSON-LD bridge.
- **Consortium model.** A hub-and-spoke arrangement with a central PI gatekeeper triggering bilateral sharing at funding/IP milestones is realistic at scale, but **explicitly out of scope** for the P2P v1; recorded here so it isn't lost.

---

## 11. References

- Canvas: [`user-story-tldraw.png`](./user-story-tldraw.png) · Schema diagram: [`Schema diagram.png`](./Schema%20diagram.png)
- Source discussion: verbatim Granola transcript (2026-06-08) — held locally, not committed (covers unpublished data)
- Schema source repo: <https://github.com/MIRA-science/schema> — [`mira.yaml`](https://github.com/MIRA-science/schema/blob/main/mira.yaml) · [`mira.ttl`](https://github.com/MIRA-science/schema/blob/main/mira.ttl) · [`mira.jsonld`](https://github.com/MIRA-science/schema/blob/main/mira.jsonld)
- KOI / KOI-net: <https://github.com/BlockScience/koi> · <https://blog.block.science/architecting-knowledge-organization-infrastructure/>
- MIRA: <https://www.mira.science> · Discourse Graphs: <https://discoursegraphs.com>
