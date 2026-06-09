# A result, with just enough of its lineage, moving between two labs

### The inter-lab exchange of results & requests — MIRA × Discourse Graphs, draft v0.1

> **Premise.** Sean (Vogel Lab) has a stress-granule result living as markdown discourse-graph
> nodes in his personal Obsidian, on his laptop, made in "five minutes" with the plugin. Kate's
> lab wants it. **As a producer, Sean wants to share a small, self-describing subgraph — the
> `Evidence`, plus the `Study` and `Protocol` that produced it, carrying *pointers* to data/code/video,
> never the 80 GB raw stack — at a granularity and permission level he controls.** Kate's lab imports
> it, trusts it from one figure, and traces back to the methods when they want to. **And the exchange
> runs both ways: as a consumer, Kate fires a `Request` back — an experiment that does not exist yet —
> which Sean's lab claims and fulfils, and the new result flows home along the same path.** Two
> first-class directions: results flow producer→consumer; requests flow consumer→producer
> (AGENTS.md §1; _grounding.md §1).

---

## 1. The cast

Five people stand around one result. The pivotal insight that shapes every screen is that they
need *different depths* of the same shared subgraph — "the big idea up front" for a PI, "follow
these nodes back" for a grad student (transcript Speakers E & A; AGENTS.md §3; _grounding.md §2).

#### Sean Moore — the producer
- **Role:** grad student / postdoc, Vogel Lab (cell biology). Owns the HeLa stress-granule result.
- **Tool:** Obsidian + Discourse Graph plugin, everything local on his laptop and a 25 TB lab array.
- **Wants:** to share the result *and* control what & how much goes out; keep private thoughts
  private; never lose the "open my laptop and in five minutes I made this" frictionlessness.
- **Defining quote:** *"The big idea up front… with busy principal investigators it's good to
  summarize — yes, we did the experiment, here's the result that matters. If they're interested,
  they can follow these nodes back."* (transcript, Speaker E)

#### Kate — the PI consumer
- **Role:** PI of a collaborating lab. The decision-maker; time-poor; thinking about many things
  at once.
- **Tool:** Obsidian *or* Roam — **TBD** (an open question, not a decided fact; AGENTS.md §10).
- **Wants:** "everything in one place, at any time"; to trust a result from a single figure panel;
  update notifications; and the ability to issue requests.
- **Defining quote:** *"Access everything in one place and at any time… if it's searchable and we
  can look at it three years from now, that would be the ideal scenario."* (transcript, Speaker J)

#### Kate's grad student — the bench consumer
- **Role:** the scientist who will actually use, check, or re-run the result at the bench.
- **Tool:** Kate's lab graph (Obsidian/Roam).
- **Wants:** to traverse from the `Evidence` to the `Study`, the `Protocol`, and the data pointers —
  the methods-level detail — and only the raw data if they intend to *re-analyze* to show robustness.
- **Defining quote:** *"What segmentation threshold did Sean use? The thresholding can change all
  the measurements… you'd go to the image-analysis node."* (transcript, Speaker E, voicing the
  grad student's question; AGENTS.md §3)

#### Anton — the synbio / MyST collaborator
- **Role:** synthetic-biology collaborator on a separate but adjacent project; the cross-domain
  conscience of the group.
- **Tool:** **MyST** Markdown; data on **S3** as CSVs; design files as **GenBank**. *"The schema
  should work across both"* (transcript, Speaker D).
- **Wants:** that a figure never travels alone — *"tell me what's in the dish or what's in the
  well"* — and that MIRA *connects to* domain ontologies (SBOL for DNA constructs) rather than
  absorbing them (R9).
- **Defining quote:** *"I always find the figure on its own to be a little bit tough to rely on…
  having a detailed table next to the figure that does more than a narrative connection."*
  (transcript, Speaker D)

#### A collaborator with NO DG app — the web-view persona
- **Role:** a clinician, a reviewer, a prospective collaborator — or simply Kate herself before
  she installs anything. The added scenario.
- **Tool:** **a web browser.** No Obsidian, no Roam, no plugin.
- **Wants:** to open the shared node(s) at a **URL as a web page**, understand them cold, follow
  the pointers, and *then* decide whether to request access or adopt a DG tool.
- **Defining quote (the design test):** *"There needs to be some way we can refer to things that
  don't exist within the graph"* (transcript, Speaker H) — extended here to: the graph must be
  *receivable* by someone who has no graph at all. (Roam: *"Share discourse nodes on the Web through
  the database"*; *"each DG node should be displayable as a Web page with a URL"*; *"markdown
  rendered as HTML is ok for v0"* — `Initiative/Inter-graph functionality`.)

---

## 2. The spine — results one way, requests the other

The story has one backbone and two currents running along it.

- **Results flow producer → consumer.** Sean → Kate. A subgraph of `Evidence` + grounding `Study`
  + that Study's `Protocol`, with pointers to data/code/video. First-class. (AGENTS.md §1; R4)
- **Requests flow consumer → producer.** Kate → Sean. A `Request` node — "we need this experiment
  /analysis, and it doesn't exist yet" — that a collaborator claims and points a new `Study`/`Evidence`
  at. **Equally first-class**, not an afterthought (transcript Speaker I: *"a request sort of invites
  collaboration… a prompt to extend the graph in a particular desired direction"*; AGENTS.md §6).

**The schema, compactly** (MIRA, extending the Discourse Graphs core; AGENTS.md §4; _grounding.md §4).
Six node types carry the story (plus `Source` for literature):

> **Question · Claim · Evidence · Study · Protocol · Request**

```
Claim     --addresses-->          Question
Evidence  --supports / opposes--> Claim          # Evidence is an Argument
Study     --grounds-->            Evidence        # inverse: is_grounded_in
Study     --follows-->            Protocol        # the canvas labels this edge "uses"
Request   --request_for-->        Study           # the study the request asks to be done
Request   --request_target-->     Claim           # the claim the requested study would illuminate
```

Two rules ride on this spine and must never be violated:
- **One `Evidence` type** — it covers Sean's own microscopy observation *and* an observation drawn
  from the literature; provenance is a field/edge (`is_grounded_in` a Study vs. `sourceDocument`),
  not a new node type. Sean himself learned this the hard way: *"separating evidence and results…
  I shot myself in the foot pretty hard"* (transcript Speaker E; **R3**).
- **Pointers, not payloads** — every data/code/media reference is a git+commit+path, an S3/HTTP URI,
  or a freeform markdown link. The 80 GB raw stack is never embedded or transferred (transcript
  Speaker E: *"discourse graphs are great for understanding which data generated which parts of the
  graph, but very, very bad at storing data"*; **R2**).

---

## 3. The three reception modes

When Sean shares, the question is *who is on the other end and what do they have to receive it
with?* The Roam `[[ISS]]` page bins this into "levels of shared context" — established collaborator,
seeding another graph, the same person in two graphs, a prospective collaborator, everyone in the
world. We collapse those into **three concrete UX modes** (_grounding.md §8). Each is told below as
a step-by-step journey and pointed at the mockup that depicts it.

### Scenario A — Import into a DG app (high shared context)
*Kate's lab runs a Discourse Graph. The subgraph travels and becomes live nodes in her graph.*
→ **mockups/01-share-subgraph.html · mockups/02-evidence-node.html · mockups/03-kate-import.html**

The worked content throughout: Sean's `Evidence` — *"Under arsenite stress, G3BP1 fluorescence
intensity rose first; PABP1 intensity rose ~40 s later"* — grounded in a live lattice light-sheet
`Study`, following an arsenite-stress + LLSM `Protocol` (_grounding.md §3).

1. **Sean selects the unit and hits share** (mockup 01). In Obsidian he selects the `Evidence` node
   and chooses *Share*. The plugin assembles the default self-describing subgraph — `Evidence` +
   grounding `Study` + that Study's `Protocol` + pointers — because each node must be interpretable
   on its own, "like the methods section of a paper" (R4; transcript Speaker E).
2. **He makes the privacy decision** (mockup 01). Sharing a node forces a decision about its
   relations. Sean shares the three-node spine and its pointers but **withholds** his private node —
   *"Sean's thoughts — is the PABP1 lag real, or a photobleaching artifact? don't trust it yet."*
   That node is never transmitted, and its existence is not leaked through a dangling reference
   (R5; transcript Speaker E: *"Sean's innermost secrets with respect to this result"*; Speaker H's
   progressive-disclosure / Ellie-koi ethnographic case).
3. **He adds context for the recipient** (mockup 01). On share, the plugin prompts him to add a
   one-line summary, the "what's in the dish" methods table Anton asked for (cell line, perturbation,
   dose, batch), and — at Kate's request — a **3-minute Loom walkthrough** so her lab can be walked
   through it the first time, "but at any time, without scheduling a Zoom" (transcript Speakers J & A).
4. **The subgraph travels as JSON-LD over KOI-net.** Markdown is the local authoring format; the
   wire format is JSON-LD/RDF, which lets each node reference external URLs as first-class pointers
   and stay extensible (R7; AGENTS.md §4.3). KOI mediates *where* the data is allowed to flow
   (transcript Speaker H).
5. **Kate's lab imports it** (mockup 03). The three nodes land in Kate's graph in MIRA colours,
   tagged with provenance ("shared by Sean Moore · Vogel Lab"). A `prefers-summary` view shows the
   `Evidence` figure first.
6. **Her grad student traverses** (mockup 02 → 03). From the imported `Evidence` they click `grounds`
   to the `Study`, then `follows` to the `Protocol`, read the segmentation threshold, and resolve the
   `GIT` pointer to `github.com/vogel-lab/sg-assembly @ a9f3e21 · /analysis/track_intensity.py`.
   Two clicks to the protocol, exactly as Sean predicted (transcript Speaker E).
7. **Kate make-edges across the boundary** (mockup 03). She draws a `supports` edge **from her own
   Claim to Sean's imported Evidence** — the cross-graph make-edge that is one of the three hard
   mechanics (AGENTS.md §9.3). The graphs are now connected without being identical (transcript
   Speaker H: *"the parts that connect to each other are able to integrate"*).
8. **She subscribes to updates.** If Sean later revises the threshold, Kate's imported node flags
   that its source updated (_grounding.md §10: "be notified when an imported node updates").

### Scenario B — View on the web, with NO DG app (the added scenario)
*Kate's lab has no Discourse Graph tooling at all. The node renders as a web page at a URL.*
→ **mockups/02-evidence-node.html · mockups/04-web-view.html**

This is the scenario that proves the exchange does not require both labs to have adopted the same
tool first. **A lab without DG tooling can still receive, read, trust, and act.**

1. **Sean shares to a person, not a graph.** Sean shares the same self-describing subgraph, but the
   recipient — a clinician, a reviewer, or Kate before she installs anything — has no DG app to
   import into. KOI mediates the share to a **published web rendering** rather than into a graph
   (transcript Speaker H: KOI can mediate flow "into another discourse-graph instance or a completely
   different platform").
2. **He sends a URL.** The shared `Evidence` node (and its traversable neighbours) is **displayable
   as a web page with a URL** — the explicit Roam requirement: *"Share discourse nodes on the Web
   through the database"*, *"each DG node should be displayable as a Web page with a URL"*, *"we need
   a URL"* (Roam `Initiative/Inter-graph functionality`). For v0 this is simply **markdown rendered
   as HTML** — *"markdown rendered as HTML is ok for v0"* (same page) — which is feasible precisely
   because the local authoring format already *is* markdown (transcript Speaker E: "the current
   implementation of discourse graphs is just a markdown file").
3. **The recipient reads it cold.** The page leads with the summary `Evidence` figure (G3BP1 climbs
   early; PABP1 lags ~40 s), captioned *illustrative · unpublished — workshop prototype*, the
   methods/"what's in the dish" table, and the embedded walkthrough video. They understand the result
   without any tooling, satisfying R4's "interpretable on its own" at the extreme.
4. **They trust it.** Provenance is visible on the page — shared by Sean Moore, Vogel Lab — so the
   reader knows who is standing behind the figure (_grounding.md §10: "see who shared this with me").
5. **They follow pointers.** The `GIT`, `S3 / LOCAL`, and `VIDEO` chips are live links — pointers,
   not payloads — so a reader can fetch the processed CSV or inspect the analysis script without the
   page ever hosting the 80 GB stack (R2). (The **local-disk** pointer to the raw stack is shown as a
   reference but its cross-lab resolution is an open question — see §8.)
6. **They act, on a ramp.** The page carries two calls to action: **request access** to deeper or
   restricted nodes ("easily request access for associated pages", _grounding.md §9), and **import
   later** if they adopt a DG tool — at which point they re-enter Scenario A with no loss. The web
   view is a *front door*, not a dead end.

### Scenario C — Request back (the collaboration primitive)
*Kate needs an experiment that does not exist yet. She fires a `Request`; Sean's lab claims and fulfils it.*
→ **mockups/05-request-flow.html**

This is the consumer→producer current, walking the full lifecycle (AGENTS.md §6; _grounding.md §6).

1. **Create.** Looking at Sean's imported sequential-recruitment `Claim`, Kate creates a `Request`:
   *"Need: repeat the assembly assay with a PABP1 phospho-null mutant to test whether recruitment
   timing is phosphorylation-dependent."* Fields: **motivation** + **skill required: live-cell LLSM**;
   `request_target` → the sequential-recruitment `Claim` (optionally `request_for` → a new `Study`).
   This is "this doesn't exist yet — a prompt to extend the graph in a desired direction," **not** a
   comment, **not** a permission-request, **not** a data-request (transcript Speakers I & H).
2. **Discover.** The open `Request` surfaces to collaborators who could fulfil it — including
   "as-yet-unidentified" ones — and they are notified (transcript Speaker I; Speaker H: "for other
   laboratories to potentially see and respond to").
3. **Claim.** A Vogel Lab member with the matching skill **claims** the Request. The claim records
   its kind — *experiment* (here), or *analysis*, or *lit-analysis* (AGENTS.md §6.3).
4. **Fulfil.** Doing the work **generates** a new `Study` → `Evidence` (the phospho-null run), and
   that new `Evidence` **points back at** the Request, closing the loop in the graph.
5. **Notify.** Kate gets a **notification of updates**, and the resulting subgraph flows back to her
   along the *normal results path* — i.e. it re-enters Scenario A. The two directions meet
   (transcript Speaker J: *"whenever something is done a notification pops up… that would be super
   nice"*).

---

## 4. Two journey maps

### The PRODUCER journey — Sean
*author → decide to share → set permissions / progressive disclosure → add summary + video → send.*

| Stage | What Sean does | Feels like / rule |
|---|---|---|
| **Author** | Opens his laptop and, in ~5 minutes, captures `Question → Claim → Evidence → Study → Protocol` as markdown DG nodes. | "Frictionless · self-explanatory · passive" — usability over formality (transcript Speaker E; AGENTS.md §8 matrix). |
| **Decide to share** | Selects the `Evidence` as the share unit; the plugin proposes the default subgraph (`Evidence` + `Study` + `Protocol` + pointers). | Self-describing subgraph; each node readable cold (R4). |
| **Set permissions / progressive disclosure** | Confirms the three-node spine is shared; **withholds** the private "is the lag a photobleaching artifact?" node; chooses the share level (this collaborator / project default / web). | Permissions first, start closed; node-level privacy + subgraph share + per-project default; no leaking un-shared nodes via dangling refs (R5; transcript Speakers H & E). |
| **Add summary + video** | Prompted to add a one-line summary, the "what's in the dish" methods table, and a 3-min Loom walkthrough; optionally "format-in-schema." | Context so the recipient understands it the first time, re-watchable any time (transcript Speakers J, A, D; AGENTS.md §7). |
| **Send** | Exports as JSON-LD; KOI-net mediates the flow to Kate's graph (Scenario A) or to a published URL (Scenario B). | Pointers not payloads on the wire; schema-agnostic transport (R2, R6, R7). |

### The CONSUMER journey — Kate's lab (this is R4 made concrete: it forks)
*The same shared subgraph serves two readers at two depths.*

**Branch 1 — Kate the PI: trust from the summary.**
1. Opens the imported node (or the web page) and sees the summary `Evidence` figure — "the
   equivalent of one PowerPoint slide / one channel of a publication figure" (transcript Speaker A).
2. Reads the one-line observation and, if it's her first look, watches the 3-minute walkthrough.
3. **Trusts and acts** — *"okay, I see you have this new information, I trust you"* (transcript
   Speaker A). She does not need to traverse. She moves to issuing a `Request` (Scenario C).

**Branch 2 — Kate's grad student: traverse to Study / Protocol / data, maybe re-analyze.**
1. From the `Evidence`, follows `grounds` → `Study` (live LLSM of arsenite-stressed HeLa).
2. Follows `follows` → `Protocol`; reads the segmentation threshold and the methods table.
3. Resolves the `GIT` pointer to the exact commit and script; opens the processed CSV.
4. **Only if re-analyzing for robustness** do they need the raw data — and even then it is reached
   by pointer to the lab array, never shipped (transcript Speakers A & E; R2). They might re-run the
   analysis a different way to confirm the lag is real, not a photobleaching artifact.

> **Why the fork matters:** one shared subgraph, two satisfied readers — *summary up front for the
> PI; traversal on demand for the grad student* (R4; AGENTS.md §3; _grounding.md §2). Build only one
> share; serve both depths.

---

## 5. How the mockups map to the story

| Mockup file | What it shows | Rule / insight it embodies |
|---|---|---|
| `mockups/01-share-subgraph.html` | Sean selecting the `Evidence` subgraph and the **share / permission** dialog; the private "Sean's thoughts" node greyed-out and withheld; prompts for summary + video. | R5 (permissions first, progressive disclosure, no leak); R4 (default subgraph); AGENTS.md §7 (add context on share). |
| `mockups/02-evidence-node.html` | The `Evidence` node itself — summary figure first, observation statement, methods table, and `GIT / S3·LOCAL / VIDEO` **pointer chips**. Shared by both Scenario A and B. | R4 ("interpretable on its own"); R2 (pointers not payloads); R3 (one Evidence type). |
| `mockups/03-kate-import.html` | Kate's lab **importing** the subgraph; provenance tag; traversal `Evidence → Study → Protocol`; the **cross-graph make-edge** (her Claim `supports` his Evidence) and update notification. | AGENTS.md §9 (results path: import + traverse + make-edge); R1 (bilateral); _grounding.md §10 (update notice). |
| `mockups/04-web-view.html` | The same node as a **web page at a URL** for a lab with no DG app — `.winbar` address bar, summary-first, video, pointers, and *request-access* / *import-later* CTAs. | Roam "Share discourse nodes on the Web"; "markdown rendered as HTML ok for v0"; R4 at the extreme; the added scenario. |
| `mockups/05-request-flow.html` | Kate creating a `Request` (`request_target` → Claim; skill: live-cell LLSM) and its **lifecycle**: create → discover → claim → fulfil → notify. | AGENTS.md §6 (Request lifecycle); the consumer→producer direction; "doesn't exist yet." |

---

## 6. Acceptance — "done when" (adapted from AGENTS.md §9)

The prototype is done when all of the following hold for the worked stress-granule case:

1. **Results path.** Sean shares the subgraph (`Evidence` + grounding `Study` + `Protocol`, each
   carrying a data/code **pointer**) from his Obsidian DG; it travels over KOI-net as JSON-LD;
   **Kate's lab imports it** and can **traverse back** from the `Evidence` to the `Study`, the
   `Protocol`, and the data pointer. (AGENTS.md §9.1)
2. **Requests path.** Kate creates a `Request` targeting the sequential-recruitment `Claim`; Sean's
   lab **sees** it, can **claim** it, and points a new `Study`/`Evidence` at it; Kate gets a
   **notification**. (AGENTS.md §9.2; §6)
3. **PoC of the hard mechanics.** Demonstrate **permissioning** (the private node never transmitted;
   no dangling-reference leak), **push / pull**, and **make-edge** across the two graphs (Kate's Claim
   → Sean's Evidence). (AGENTS.md §9.3)
4. **Web-view path (added).** A recipient with **no DG app** can open the shared `Evidence` at a
   **URL as a web page**, read it cold (summary figure + methods table + video), follow the pointers,
   and use a *request-access* / *import-later* CTA — with the un-shared node still invisible.
   (Roam `Initiative/Inter-graph functionality`; R4; R5)

---

## 7. Out of scope (v1)

Explicitly **not** built in this prototype, recorded so they aren't mistaken for omissions:

- **Hosting or transferring raw data.** The 80 GB LLSM stacks live on the lab's 25 TB array / S3;
  the system only ever **references** them (R2; AGENTS.md §2; transcript Speakers E, C, H).
- **Conflict resolution / graph merge.** The deprecate-both, mint-a-merged-node, union-the-edges,
  keep-greyed-for-attribution approach (plus `owl:sameAs`) is **deferred** — the model must *allow*
  it later; do not implement it now (AGENTS.md §2, §10; transcript Speakers E, J, H).
- **Consortium / central-PI gatekeeper.** Farida's hub-and-spoke model — a central PI triggering
  bilateral sharing at funding/IP milestones — is realistic at scale but **out of scope** for the P2P
  v1, which keeps each lab mediating its own bilateral relationships (R1; AGENTS.md §2, §10;
  transcript Speaker B's consortium description vs. Speaker H's per-org mediation).

## 8. Open questions carried into design

Surfaced, **not solved** (AGENTS.md §10; _grounding.md §9; Roam "considerations"):

- **"Send" closure.** How much *additional* connected context auto-travels, and how a recipient pulls
  a referenced-but-not-included node (e.g. a `Protocol` two hops away that Kate's grad student needs
  but Sean didn't include) — and how this varies by recipient role (transcript Speaker E's "draw a
  circle around the nodes I point to… can I import that third node even if it doesn't touch my graph?").
- **Dangling references: signal vs. hide.** When exporting a node, do we carry its relations as a
  *dangling indicator* of missing resources (a signal), or *hide* them until both sides import? (MAP
  prefers hide; Matt leans signal.) (_grounding.md §9; transcript Speaker H on progressive disclosure.)
- **Local-disk pointer resolution.** S3/GitHub pointers resolve cleanly across labs; a **local-disk**
  pointer to Sean's 25 TB array does not. How does Kate's lab resolve it? (AGENTS.md §10; transcript
  Speakers E & D — "your microscopy data stays on a local hard drive… fundamentally pointers.")
- **Versioning.** When a node refers to an external resource, the referrer wants the *version they
  referred to* preserved (the frozen commit, not whatever the repo becomes later) (_grounding.md §9;
  transcript Speaker E: "we've frozen it; it will be this way for as long as GitHub exists").
- **Permission granularity & progressive-disclosure enforcement** atop KOI's *weak* native permissions
  — node vs. subgraph vs. project, and "easily request access" for associated pages (R5; AGENTS.md
  §8, §10; transcript: "Does KOI have permissions? — Kind of, not very strong").
- **Provenance boundary.** How much pipeline provenance (PROV-O / Snakemake / Nextflow) lives *in* the
  graph vs. an external tool it points to — R9 sets the direction, the exact line is unsettled
  (transcript Speaker F: "is discourse graph the right way to handle it? That's a different question").
- **Kate's tool is TBD.** Obsidian vs. Roam, and cross-tool validation (Obsidian ↔ Roam ↔ MyST) via
  the JSON-LD bridge, remains open (AGENTS.md §3, §10).

---

*MIRA × Discourse Graphs · inter-lab exchange · draft v0.1*
