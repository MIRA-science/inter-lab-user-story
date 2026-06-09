# Grounding pack — the Sean → Kate **dashboard** (push results to a shared web interface)

> **The central use case of this repo.** A producer (**Sean**, a PhD student in the **Vogel Lab**)
> pushes a *selected subgraph* of his discourse graph to a **read-only shared web interface — a
> dashboard / kanban** — that a known collaborator and her lab (**Kate**, a PI) can navigate **without
> running any discourse-graph tooling**. The dashboard can be **public** (a lab website, open
> recruitment) or **password-protected** (a consortium of 20-plus labs). Unlike the fully-public QBI
> case ([`_grounding.md`](./_grounding.md)), this tier has a **second current**: Kate's lab can issue a
> **Request** back (for a follow-up experiment or analysis).
>
> **Sources** (do not contradict). Three Granola sessions, distilled here (raw transcripts stay local,
> per [`.gitignore`](../.gitignore)):
> - **"Collaboration between labs experiment"** — Jun 8, 2026 (the session where Sean *demos* the
>   stress-granule graph live). The worked example comes from here.
> - **"User story clarification session"** — Jun 9, 2026 (interop axes; the KOI **dashboard** track;
>   share-a-selected-subgraph + permissions; the **Request** type; "extract graph elements, don't
>   compose narrative pages").
> - **"Dashboard collaboration ideas"** — Jun 9, 2026 (the definitive **dashboard scope**: read-only,
>   public/password, Graph·Kanban·Table, preset views, accessibility).
>
> Plus the canvas **[`../low-context-user-story.png`](../low-context-user-story.png)** — which *is* this
> story: **Sean's notebook → Kate's lab → a shared web interface ("KOI DG nodes to dashboard/kanban on
> web interface")**, carrying a **`Request for study`** node in Kate's column — and the canonical schema
> (**<https://github.com/MIRA-science/schema>**, `mira.yaml`). Where this file and the repo's
> [`AGENTS.md`](../AGENTS.md) rules interact, the rules hold; this use case changes the **destination**
> (a shared web interface) and **re-activates the Request-back current**, not the data model or the
> pointers-not-payloads discipline.
>
> **Naming note** (repo convention): **Sean and Kate are scientists and are first-class actors.** The
> transport/schema/dashboard engineers around these sessions — KOI, the sync plugin, the schema work,
> the facilitator — are **roles/components** ([`AGENTS.md §8`](../AGENTS.md#8-architecture--stack)),
> **not** user-story actors. Anton (another lab, on a different platform) appears only as the
> *"…and it scales to N labs / other tools"* extension, not the central cast.

---

## 0. One-line story

Sean has a stress-granule result living as markdown discourse-graph nodes in his Obsidian vault. He
wants Kate's lab to **see it, trust it, and dig into it on their own time** — without installing
anything, without a Zoom call, and **without handing over his 80 GB of raw images or his private
notes.** He pushes a **selected subgraph** through KOI to a **shared web dashboard**; Kate (the PI)
**glances at the summary plot and trusts it**; her **grad student traverses back to the segmentation
threshold** and pulls the curated CSV; and when they want more, they **send a Request back** for a
follow-up experiment or analysis. The same dashboard, made public, becomes the lab's website and its
recruiting surface.

---

## 1. Where this sits — the shared-context ladder, and why it's *here*

The repo's other use case (Brian → the world, [`_grounding.md`](./_grounding.md)) is the **🥉 low /
zero-context extreme**: a stranger, no relationship, no tooling, fully public. The Sean → Kate
dashboard sits **one notch up** — there *is* a relationship and a shared project — **but it rides the
exact same machinery** and belongs in the same repo because the **destination is a shared *web
interface* readable by people who are not on the platform.** That is the through-line of this
repository:

> **Push a result to a shared web surface — from a *consortium dashboard* (Sean → Kate, gated) to the
> *fully public* (Brian → world) — readable without any discourse-graph app.**

| | Lab-to-lab push/pull (`./lab-to-lab/`) | **Sean → Kate dashboard (root, central)** | Brian → world (root, retained) |
|---|---|---|---|
| **Destination** | import into Kate's **own DG graph** | a **read-only shared web dashboard** | a **fully public repo / database** |
| **Recipient tooling** | Kate runs a DG | **a browser** (no DG needed) | a browser |
| **Openness** | private P2P | **public OR password-protected (consortium)** | public to everyone |
| **Reverse current** | first-class `Request` into Sean's graph | **`Request` surfaced on the dashboard** | weak / absent |
| **Trust basis** | the relationship | the relationship **+ visible provenance** | visible provenance only |

The dashboard is explicitly the **complement** to in-graph sharing: *"makes that data legible to people
with zero discourse-graphs background"* (Dashboard collaboration ideas, Jun 9). Lab-to-lab sharing
*into* a graph already works via the plugin; the dashboard is the **viewing/discovery surface** on top.

---

## 2. The USER STORY

### 2.1 The cast

| Role | Who | Tool | In the story |
|---|---|---|---|
| **Producer (bench)** | **Sean** — PhD student, **Vogel Lab** (cell biology) | Obsidian + DG plugin; 25 TB raw-data array | Authors the graph; **selects the subgraph to share**; sets permissions; keeps personal notes private. |
| **Consumer (PI)** | **Kate** — PI of a collaborating stress-granule lab | a **web browser** (the dashboard) | **Glances and trusts**; manages **multiple collaborators across multiple projects**; wants everything **in one place, searchable, for years**. |
| **Consumer (reproducer)** | **a grad student in Kate's lab** | the dashboard | **Traverses back to the segmentation threshold**; follows pointers to the CSV; may **request access** / **request a follow-up**. |
| **Viewer (public / funder)** | a lab-website visitor, a recruit, a funder | the public dashboard | Sees a **preset view** — open **questions/requests** (recruiting) or **experiments-in-progress** (funder); needs a *plain-language* summary. |

> **The pivotal insight (same as the inter-lab and QBI stories): one bundle, different depths.** *"The
> big idea up front… with busy principal investigators"* for the glancer; *"what segmentation threshold
> did Sean use… go to the image analysis node"* for the reproducer (Collaboration between labs, Jun 8).

### 2.2 The worked example (verbatim from the Jun 8 demo)

> Real test case Sean built live. **Stress-granule protein recruitment in HeLa cells.** Mark figures
> *illustrative · unpublished — workshop prototype* — Sean: *"we can't publish it outside of here
> because it's unpublished data, but you can experiment."*

- **The system (Sean, Jun 8):** *"the collaboration between Kate's lab and my lab, Vogel Lab, would
  produce… they visited us and we had these HeLa cells and then we cultured them and then exposed them
  to stress and image[d] them… using fluorescence microscopy, we were able to track the intensity of
  these two… proteins within stress granules as they're forming or dissolving at really high… spatial
  resolution."*
- **Claim / result (illustrative):** *the two stress-granule markers are recruited in a fixed order —*
  **G3BP1 rises before PABP1** *as granules assemble* (same protein pair as the sibling inter-lab
  story, for cross-repo consistency; the transcript names only "two proteins").
- **Evidence — key figure:** two intensity-vs-time curves; **G3BP1 climbs earlier, PABP1 lags**, as
  granules form (and the order reverses as they dissolve).
- **Study (the producers call it the "experiment"):** live fluorescence microscopy of stressed HeLa
  cells, two-channel, tracking per-granule intensity over time.
- **Protocol:** HeLa culture → stress induction → live imaging → **segmentation (threshold)** →
  per-granule intensity quantification. *The segmentation threshold is the load-bearing methods detail.*
- **Pointers, not payloads (R2), at the extreme:**
  - `git` → the analysis repo + commit (Speaker C, Jun 8: *"work out the git repo… work out the commit…
    a link to the position in that file… the CSV file that might be in a git repo"*).
  - `data` → the curated **CSV** of measurements *for this figure* (Sean: *"it's the CSVs that get
    passed around and then we share the method that the CSVs [were] generated by"*).
  - `local` → the **80 GB raw image stacks** — **gated** (Sean: *"my image files are like 80 gigabytes…
    we have a 25 terabyte array of hard drives… there's no raw data in here whatsoever"*).
  - `video` → a short **walkthrough** (Kate: *"maybe they could record a video… we don't have to
    schedule a zoom call… and maybe three years from now…"*).
- **Private, withheld (Sean):** *"a node here that's just like Sean's personal thoughts… maybe not
  share that one, have some kind of granularity."* → the "1 note withheld, not leaked" pattern (R5).
- **Provenance:** *Sean · Vogel Lab*, shared with *Kate's lab* under a consortium project.

### 2.3 The dashboard itself (the headline screen — Dashboard collaboration ideas, Jun 9)

The agreed scope, almost verbatim:

- **A read-only web view of discourse-graph data, at a public URL.** *"No writing or contributing from
  the dashboard — it's a viewing/discovery surface."* Data = a **subset chosen from within the user's
  Obsidian vault.**
- **Public OR password-protected.** *"when there's like 20-some labs involved in a research project
  together… they will be able to access the dashboard, but not have everyone in the world see it yet."*
  Public for *"lab websites, open recruitment."*
- **Three views:** a **Graph view** (*"move the nodes around spatially… so you can understand what
  questions the group is trying to answer"*); a **Kanban / Table** (*"left is the question, then the
  next column you have potential experiments… results"*); click any node for full content.
- **Preset views** (*"a few different most common desired views… you press a button"*):
  - **Questions / requests centered** — for public lab-website visitors & recruiting.
  - **Experiments-in-progress centered** — *"for the funder seeing progress."*
  - **Results centered** — for showing completed work.
- **Accessibility is a stated requirement:** *"please make sure it's colorblind friendly"*; legible for
  a *"60-year-old-plus professor whose last tech thing was C"* (EU standard).

### 2.4 The two currents (this is what makes the tier different from QBI)

The QBI/world case is **one-directional** (producer → public). The dashboard tier **re-activates the
Request-back current** the canvas shows (a `Request for study` node in Kate's column):

1. **Results out:** Sean pushes the shareable subgraph — *"the two elements we want to share are the
   results / evidence and associated [experiments]"* (User story clarification, Jun 9).
2. **Requests back:** *"we just have to create a request type in discourse graph"* — Kate's lab can
   **claim** an open request or **send a request for [experiment, analysis]** back to Sean. On the
   recipient side: *"notification of updates… seeing the summary… being able to import connected nodes."*

---

## 3. USER CONSTRAINTS (what the humans need / won't tolerate)

- **"Access everything in one place, at any time, searchable" (Kate, Jun 8):** *"I asked the lab what
  they wanted… access everything in one place and at any time… if it's searchable and we can look at it
  three years from now, that would be the ideal scenario."* The dashboard is a durable index, not a
  meeting artifact.
- **Glance-and-trust for the PI; how-did-you-do-it for the grad student (Jun 8):** *"as the PI, often
  that will be sufficient… I trust you. And then the grad student would want to know how you did it."*
- **Traverse to the method that matters (Jun 8):** *"what segmentation threshold did Sean use… the
  thresholding can change all the measurements… go to the image analysis node."*
- **A video walkthrough beats a Zoom call (Kate, Jun 8):** orient a first-time reader asynchronously;
  re-watchable years later.
- **Selective visibility / progressive disclosure:** some nodes shared, **personal thoughts private**;
  sharing one node must not leak the existence of connected private ones (R5).
- **One project umbrella, many collaborators (Kate, Jun 8):** *"if I can do this with Sean, then I can
  do it with all my collaborators… I have multiple projects where there's other collaborators… share
  under one project umbrella"* — selective visibility per collaborator/project.
- **Public vs. consortium is a deliberate choice:** start gated; open up on purpose (*"easier to open a
  closed system than close an open one"*).
- **Don't break the frictionless authoring flow:** Sean's graph takes *"10 seconds"* to set up a new
  experiment; publishing must ride that, not tax it.
- **Candidate (informal) nodes are the baby step (Dashboard collab, Jun 9):** *"you don't want to think
  while you're having the idea"* — so nodes can start **informal/candidate**, be **shared as-is**, and
  *"somebody else can formalize it or just see it as it is."* (Whether the author must approve a
  formalization is an **open question**.)
- **Accessibility:** colorblind-safe; legible for non-technical, older readers; a plain-language tab.
- **Build on each other's work (Jun 9):** *"the lab-to-lab collaboration between Kate and Sean of being
  able to build on each other's discourse graphs… make the data more searchable and easier to use."*

---

## 4. TECH CONSTRAINTS (what the systems impose / block)

- **The dashboard is READ-ONLY.** A rendering/viewing surface; no authoring or contribution from it.
  Authoring stays in the vault; the dashboard reflects a **published subset**.
- **Pointers, not payloads (R2), at the extreme.** Raw data (~80 GB) lives on a **25 TB lab array** — a
  third system entirely; the graph carries **GitHub + commit + path**, **S3**, and **curated CSV**
  pointers, never the bytes. Non-text assets (CSVs, images, DNA files) are *"currently only embedded as
  content, not exposed as separate schema fields — flagged as a gap"* (Dashboard collab, Jun 9).
- **KOI is the transport, and it is *not* the open internet.** *"The internet… is like air travel all
  around the world. The KOI app is like a specific expressway"* — a structured interoperability layer
  between MIRA-compatible tools across organizations; *"each org manages individual relationships rather
  than centralized permissions."* The dashboard is *"a small application that gets MIRA notes from KOI
  and displays a [notification/visualization] board."*
- **Private must not leak through KOI (R5).** *"Data marked private in Discourse Graph must not become
  public when routed through KOI."* Permissions/accounts are needed (*"they need an account, because
  permissions"*) and **KOI's native permissions are weak** — gating needs work on top.
- **MIRA (JSON-LD) is the interchange; DG is already structured.** Discourse-graph data needs *minimal*
  transformation to render on the dashboard; long-form docs from other tools (e.g. Nucleus/MIST) must
  first be *"chopped into claim/result/request nodes."* **Extract graph elements *from* existing pages —
  don't compose narrative pages from the graph.**
- **The `Request` type is in scope.** A first-class `Request` node (with `request_for` / `request_target`
  edges) carries the request-back current; *"we have subgraph, we have permission"* already.
- **Shared ≠ editable.** *"By default, shared nodes are visible but not editable by the receiving lab."*
- **Provenance must be transportable and support invalidation (Jun 8):** *"you might find out three
  months later that there's a bug in your segmentation… you'll need to know everything downstream of
  that and invalidate that and recompute it… [or] the microscope is too cold."* Auto-generated bullets
  carry *"provenance so you can see who created it"* and when.
- **Automation that preserves intent.** Crawlers can read a repo and *"recognize when your Python script
  reads a CSV file"* (Snakemake/Nextflow integrations) and auto-propose edges — *"but then you lose that
  intent. That's important."* AI proposes **candidate** nodes; a human keeps what's relevant.
- **Stack** (UX sits above this): Obsidian DG plugin → Supabase → **KOI** (RIDs; the cross-org sync
  plugin) → **the dashboard web app** (Graph/Kanban/Table) + onward public destinations. Roam vs.
  Obsidian, data-sovereignty/hosting (EU) are adoption considerations, not user-story actors.

---

## 5. Open questions carried into design (surface, don't "solve")

- **Candidate-node formalization flow:** when someone else formalizes your shared candidate node, do you
  approve / get notified? *"That's an open question."*
- **Non-text assets in the schema:** how CSVs / images / DNA files become **referenceable schema fields**,
  not just embedded content.
- **Accounts & gating atop weak KOI:** how password-protected consortium views and per-resource
  request-access are enforced.
- **Conflict / merge:** when Kate's lab and Sean's lab work independently then need to reconcile graphs.
- **Preset-view set:** which presets ship (results / experiments-in-progress / questions-&-requests …),
  and whether they're truly presets or just saved layouts.
- **Provenance standard:** is the transported provenance to a recognized standard (PROV-O), and how much
  pipeline provenance (Snakemake/Nextflow) lives in the graph vs. a tool it points to.
- **Approval/notification when a request is claimed**, and how updates propagate to subscribers.

---

## 6. Provenance (sources + verified quotes)

- **"Collaboration between labs experiment"** — Jun 8, 2026 (Granola; Sean demos). Worked example
  (HeLa / two stress-granule proteins / forming-dissolving); 80 GB / 25 TB array; CSV-as-shared-unit;
  git+commit pointer; private "personal thoughts" node; glance-and-trust vs. segmentation-threshold;
  video walkthrough; "one place / searchable / three years"; one-project-umbrella / many collaborators;
  invalidate-and-recompute provenance; unpublished-data caveat.
- **"User story clarification session"** — Jun 9, 2026 (Granola). Interop axes (lab/platform/direction);
  **KOI dashboard track** (kanban from KOI notes); share-a-selected-subgraph + permissions UX;
  results+evidence as the shared elements; **the `Request` type**; "extract graph elements, don't
  compose narrative pages"; private-must-not-leak; assets-not-yet-referenceable.
- **"Dashboard collaboration ideas"** — Jun 9, 2026 (Granola). **Dashboard scope:** read-only web view
  at a URL; public OR password-protected (20+ labs); Graph / Kanban / Table; **preset views**
  (questions-requests / experiments-in-progress-for-funders / results); accessibility (colorblind, 60+);
  candidate nodes shareable & formalizable; KOI = "expressway"; data-sovereignty adoption angle.
- **Canvas:** [`../low-context-user-story.png`](../low-context-user-story.png) — Sean's notebook → Kate's
  lab → shared web interface; "KOI DG nodes to dashboard/kanban on web interface"; a `Request for study`
  node; rows: location / format / feels-like / features / schema / tasks (dashboard-kanban,
  permission/account, notifications, public URL).
- **Schema:** <https://github.com/MIRA-science/schema> (`mira.yaml`): Question · Claim · Evidence ·
  Study · **Request** · Protocol; edges `grounds`/`is_grounded_in`, `follows`, **`request_for`**,
  **`request_target`**, `supports`/`opposes`, `addresses`.
- **Sibling & companion:** [`../lab-to-lab/`](../lab-to-lab/) (import-into-a-
  known-graph; same Sean → Kate cast & G3BP1/PABP1 example); [`./_grounding.md`](./_grounding.md) (the
  QBI / fully-public worked example retained in this repo).

---

*MIRA × Discourse Graphs · push results to a shared web interface · Sean → Kate **dashboard** (central) · context pack draft v0.1*
