# Grounding pack — Inter-lab exchange of results & requests

> Single source of truth for every agent producing an artifact in this folder.
> Distilled from `../AGENTS.md` (the spec), `../context/transcript-2026-06-08.md`
> (verbatim workshop transcript), the MIRA schema (`../../../schema/mira.yaml`), and the
> Discourse Graphs team Roam graph (`Inter-graph Use Cases`, `[[ISS]] - Document
> initial inter-graph use cases`, `Initiative/Inter-graph functionality`,
> `MIRA workshop 2026`). **Do not contradict `AGENTS.md`.** Where the transcript and
> AGENTS.md disagree, AGENTS.md wins.

---

## 1. The one-line story

A data **producer** (Sean, Vogel Lab) shares a small, self-describing **subgraph** — an
`Evidence` result plus the `Study` and `Protocol` that produced it, carrying **pointers**
to data/code/video (never raw data) — with a **consumer** (Kate's lab), at a granularity
and permission level Sean controls. The consumer imports it, traverses back to the data,
gets notified of updates, and can fire a **`Request`** (an experiment/analysis that doesn't
exist yet) back the other way, which a collaborator claims and fulfils.

Two first-class directions:
- **Results** flow producer → consumer (Sean → Kate).
- **Requests** flow consumer → producer (Kate → Sean).

## 2. Personas

| Persona | Role | Tool | Needs |
|---|---|---|---|
| **Sean Moore** | Grad student / postdoc, Vogel Lab (cell biology) | **Obsidian + Discourse Graph plugin** | Produce the result; *control* what & how much he shares; keep private thoughts private; "5-minutes-on-my-laptop" frictionless authoring. |
| **Kate** | PI of a collaborating lab | **Obsidian or Roam (TBD)** | "Everything in one place, any time"; trust a result from a one-figure summary; get notified of updates; issue requests. |
| **Kate's grad student** | bench scientist in Kate's lab | Obsidian/Roam | Traverse from the result to Study/Protocol/data; maybe re-analyze; needs the methods-level detail. |
| **Anton** | Synthetic biology collaborator | **MyST**, data on **S3**, **GenBank** files | Cross-domain interop; champions a domain ontology layer (**SBOL**). |
| **A lab WITHOUT a DG app** (key added scenario) | e.g. a clinician, a reviewer, a prospective collaborator, or just Kate before she installs anything | **A web browser** | View the shared node(s) at a **URL as a web page**, understand them cold, follow pointers, request access or import later. |

**Recipient-dependent depth (the pivotal insight):** a **PI** usually needs only the summary
`Evidence` (≈ one figure panel / one slide) to trust and act. A **grad student** wants to
traverse to `Study`, `Protocol`, and data pointers, and only needs raw data to *re-analyze*.
One shared subgraph must serve both: **summary up front, traversal on demand.**

## 3. The worked example (use this concrete content in mockups — mark as illustrative)

> Real canonical test case from the workshop; specific protein names below are plausible
> stand-ins (G3BP1 / PABP1 are textbook stress-granule markers). Data is **unpublished** —
> label any figure "illustrative / unpublished — workshop prototype."

- **Question** — *"How are proteins recruited into stress granules as the granules assemble?"*
- **Claim** — *"In stressed HeLa cells, G3BP1 is recruited to stress granules before PABP1 (sequential, not simultaneous, assembly)."*
- **Evidence** *(supports the Claim)* — *"Under arsenite stress, G3BP1 fluorescence intensity rose first; PABP1 intensity rose ~40 s later."* The summary figure: one line (G3BP1) climbs early, a second line (PABP1) stays flat then climbs later.
- **Study** *(grounds the Evidence)* — *"Live lattice light-sheet imaging of arsenite-stressed HeLa cells, tracking G3BP1 & PABP1 intensity at high spatial resolution as granules form."*
- **Protocol** *(the Study follows)* — *"Arsenite stress induction + LLSM acquisition + intensity tracking with literature-standard SG segmentation threshold."* Carries the "what's in the dish" context table (cell line, perturbation, dose, batch) Anton asked for.
- **Pointers** (R2 — pointers, not payloads):
  - `GIT` — `github.com/vogel-lab/sg-assembly @ a9f3e21 · /analysis/track_intensity.py`
  - `S3 / LOCAL` — raw LLSM stack ≈ **80 GB**, on the lab's 25 TB array (local-disk pointer — resolution is an open question); processed CSV is the shareable artifact.
  - `VIDEO` — a 3-min Loom walkthrough Sean records on share.
- **Private node, NOT shared** (privacy granularity): *"Sean's thoughts — is the PABP1 lag real, or a photobleaching artifact? don't trust it yet."*
- **Request example (Kate → Sean)** — *"Need: repeat the assembly assay with a PABP1 phospho-null mutant to test whether recruitment timing is phosphorylation-dependent."* skill required: *live-cell LLSM*; `request_target` → the sequential-recruitment Claim.

## 4. Schema (MIRA, extends Discourse Graphs core)

Nodes: **Question · Claim · Evidence · Study · Protocol · Request** (+ Source for literature).
Edges:
```
Claim     --addresses-->        Question
Evidence  --supports/opposes--> Claim
Study     --grounds-->          Evidence     (inverse: is_grounded_in)
Study     --follows-->          Protocol     (canvas labels this "uses")
Request   --request_for-->      Study
Request   --request_target-->   Claim
```
- **Evidence is ONE type** (covers own results AND literature observations) — never split "Result" from "Evidence" (R3).
- **Interchange = JSON-LD / RDF**; local authoring = markdown in a discourse graph (R7).
- Connect to domain/provenance ontologies (PROV-O for "how data was made", SBOL for synbio), don't absorb them (R9).

## 5. The normative rules (cite these; mockups & docs must not violate them)

- **R1** P2P, no central authority — each lab mediates its own bilateral sharing.
- **R2** Pointers, not payloads — reference data/code/video by git+commit+path / S3·HTTP URI / markdown link; never embed/transfer raw data.
- **R3** One `Evidence` node type.
- **R4** "Send" = a small self-describing subgraph (`Evidence` + grounding `Study` + its `Protocol` + pointers). Each node interpretable on its own (a header/summary "like a paper's methods section"); deeper context by traversal. **Summary up front for the PI; traversal for the grad student.**
- **R5** Permissions first, start closed. Node-level privacy + subgraph-level sharing + a per-project default share. **Progressive disclosure**: sharing a node forces a decision about its relations; a shared relation can *imply the existence of* an un-shared node; the system MUST NOT leak existence/metadata of un-shared nodes through dangling references.
- **R6** Schema-agnostic transport — changing the schema must not break sharing.
- **R7** JSON-LD on the wire; RDF as the model; don't make collaborators run a triple store.
- **R8** Human-readable vs machine-readable — only formalize a field when a *tool* must read it.
- **R9** Connect to domain ontologies, don't absorb them.
- **R10** Preserve intent; no fully-automatic capture (auto-suggest → user confirms).

## 6. The Request lifecycle (R / §6)

Create (motivation + skill required, `request_target` → Claim) → Discover (collaborators see/are notified) → Claim (a member with the matching skill claims it: experiment / analysis / lit-analysis) → Fulfil (generates a new Study → Evidence that points back at the Request) → Notify (requester gets an update; the result flows back via the normal results path).

`Request` = "this doesn't exist yet — a prompt to extend the graph in a desired direction." NOT a generic comment, NOT a permission-request, NOT a data-request.

## 7. Architecture / stack (for context; mockups stay at the UX layer)

Interface = Discourse Graph plugin in Obsidian (Roam/MyST elsewhere) · Schema = MIRA ·
Infra = Supabase · Transport/mediation = **KOI-net** (`koi-obsidian-plugin`, BlockScience/Metagov/RMIT;
uses **RIDs** = Reference IDs / pointers; **permission model currently weak — needs work on top**) ·
Domain ontologies = PROV-O, SBOL (connected, not absorbed).

The existing **Obsidian DG plugin** already: lets you define node types with a **colour**, share a node out into a KOI network that mediates where data may flow (incl. into another DG instance or another platform), uses **tldraw** for canvas, hotkey menu (⌘+\), context menus, sidebar widgets.

## 8. The three reception modes (these structure the mockups & the user story)

The Roam graph bins sharing into **levels of shared context** (from the `[[ISS]]` page):
*established collaborator · seeding another graph from a trusted collaborator · literally the
same person in two graphs · prospective collaborator · everyone in the world*. We collapse these
into three concrete UX modes the producer chooses between when sharing:

1. **Import into a DG app** (high shared context) — Kate's lab runs Obsidian/Roam DG; the subgraph travels over KOI as JSON-LD and is imported into her graph; she can make-edge from her own nodes to Sean's, and gets update notifications. *(Mockup 03.)*
2. **View on the web** (low / no shared tooling — THE ADDED SCENARIO) — Kate's lab has **no DG app**. The node(s) render as a **web page at a URL** (DG "Share discourse nodes on the Web through the database"; Semble: "each DG node should be displayable as a Web page with a URL"; v0 = markdown rendered as HTML). Summary up front, traversable subgraph, pointers, video, and CTAs to *request access* or *import later if you adopt a DG tool*. *(Mockup 04.)*
3. **Request back** (collaboration primitive) — the consumer issues a `Request`; the producer's lab sees, claims, and fulfils it. *(Mockup 05.)*

## 9. Open questions (AGENTS.md §10 + Roam "considerations") — surface, don't "solve"

- Exact "send" closure: how much connected context auto-travels; how a recipient pulls a referenced-but-not-included node (Protocol two hops away); varies by role.
- Permission granularity: node vs subgraph vs project; how progressive disclosure is enforced atop KOI's weak permissions; "easily request access" for associated pages.
- Dangling references: when exporting a node, do we carry its relations as a dangling indicator of missing resources (a *signal*), or hide them until both sides import? (MAP prefers hide; Matt leans signal.)
- Conflict / merge: two graphs evolve overlapping nodes → mark both deprecated, mint a merged node, union the edges, keep deprecated greyed for attribution; plus `owl:sameAs`. **Deferred** — model must allow, don't build in v1. Also: two labs adding different field values (e.g. a second author).
- Pointer resolution for **local-disk** data (S3/GitHub resolve cleanly across labs; local does not).
- Provenance boundary: how much pipeline provenance (PROV-O/Snakemake/Nextflow) lives in the graph vs an external tool it points to.
- Versioning: when you refer to an external resource you want to keep the version you referred to.
- Kate's tool (Obsidian vs Roam) and cross-tool validation (Obsidian ↔ Roam ↔ MyST) via the JSON-LD bridge.
- Consortium / central-PI gatekeeper model (Farida's milestone-gated bilateral unlocking) — realistic at scale but **explicitly out of scope** for the P2P v1.

## 10. Recipient-side & relationship stories already gathered (from the `[[ISS]]` page)

"As a recipient, see who shared this with me (provenance)" · "be notified when an imported node
updates" · "as a cited author, see who's using my work" · "see all nodes I've shared & who imported
them" · "revoke access" · "disconnect from a graph" · "search across all graphs I can access" ·
"find collaborators on similar questions" · "know if an import failed & why" · "restore a previous
version of a synced node."

## 11. Design palette + type (mockups must use the shared CSS, not reinvent)

Use `tokens.css` + `components.css`. Node-type colours are fixed:
Question = amber/gold · Claim = green · Evidence = coral/salmon · Study = blue ·
Protocol = violet/lavender · Request = indigo · Source = teal.
Fonts: **Fraunces** (display), **Hanken Grotesk** (body/UI), **JetBrains Mono** (pointers/JSON-LD).
Aesthetic: warm-paper "lab-notebook / editorial," minimal-scientific, faint dotted grid, soft layered shadows. Light theme by default; `.theme-obsidian` class flips to a dark Obsidian-like pane.

## 12. Provenance of claims (for citation in the written docs)

- Spec: `../AGENTS.md` §1–§11 (esp. §3 personas, §5 rules R1–R10, §6 Request, §9 acceptance, §10 open Qs).
- Transcript: `../context/transcript-2026-06-08.md` (e.g. summary-vs-traversal for PI/grad student; 80 GB on the 25 TB array; "pointers not payloads"; "permissioning must be thought about at the start … harder to close an open system"; progressive disclosure / Ellie-koi ethnographic case; "node interpretable on its own … like a paper's methods section"; "everything in one place, any time" + walkthrough video; Request = "doesn't exist yet … prompt to extend the graph").
- Roam (dg-team): `Inter-graph Use Cases`, `[[ISS]] - Document initial inter-graph use cases` (recipient/relationship/discovery/error stories; considerations; levels of shared context), `Initiative/Inter-graph functionality` ("each DG node displayable as a Web page with a URL"; "markdown rendered as HTML is ok for v0"; "we need a URL"), `MIRA workshop 2026`.
