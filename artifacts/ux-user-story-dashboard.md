# A result your collaborators can read without your tooling — on a shared dashboard

> 🗂️ **Superseded for day-to-day work by [`ux-spec.md`](./ux-spec.md)** — the single current UX spec, which
> folds in this narrative plus the dated `ux-spec-update-*` deltas and matches the mockups as built.
> Retained here for its narrative. **Note the details below predate the v0.4 rebuild:** the worked science
> is now *cluster composition over time* (not "Component A-before-Component B assembly order"); there are **11**
> screens (06–11), not 9; "glance-and-trust" carries **no Endorse**; the PI **nudge was removed**. *(Consolidated 2026-06-15.)*

### Push results to a shared web interface — the Sean → Kate **dashboard** · MIRA × Discourse Graphs · the central use case · draft v0.1

> **Premise.** Sean, a PhD student in the **Vogel Lab**, has a cluster result living as markdown
> discourse-graph nodes in his Obsidian vault: in samples under perturbation, two cluster components
> are recruited in a fixed order — **Component A climbs before Component B** as clusters assemble. He wants **Kate's
> lab** — a collaborating PI and her students — to **see it, trust it, and dig into it on their own
> time**: not by installing a discourse-graph app, not on a scheduled Zoom call, and **without** him
> shipping his 80 GB of raw images or exposing his private notes. **As a producer, Sean publishes a
> *selected subgraph* — the `Evidence` and the `Study`/`Protocol` behind it, carrying *pointers* to the
> analysis and the curated CSV — to a *read-only shared web dashboard*, public or password-protected.**
> **As readers, Kate glances at the summary and trusts it; her grad student traverses back to the
> segmentation threshold and pulls the CSV; and either can send a *Request* back for a follow-up.** Two
> currents: results out, requests back (AGENTS.md §1; _grounding-dashboard.md §2).

This is the **central use case** of the repo. Its public-facing extreme — *Brian → the whole world*, no
relationship at all — is told in [`ux-user-story.md`](./ux-user-story.md) and stays exactly as built.
The through-line that unites them: **the destination is a shared *web interface*, readable without any
discourse-graph app** — sliding from a *consortium dashboard* (Sean → Kate, gated, with a request-back)
to a *fully public* page (Brian → world). Same machinery (KOI → MIRA JSON-LD → web rendering), same
rules (pointers, summary-first, permissions-without-leaks, provenance, freeze, a URL).

> **The surface here is a read-only dashboard** Kate's lab simply *views in a browser* — no discourse-graph
> app to install, the result *"legible to people with zero discourse-graphs background"* (Dashboard
> collaboration ideas, Jun 9).

---

## 1. The cast

Four people stand around one shared dashboard. The pivotal insight is the one that recurs across every
tier of this work — readers need *different depths* of the same bundle — but here the readers are a
**known PI and her lab**, so trust starts warmer and the questions get more specific (AGENTS.md §3;
_grounding-dashboard.md §2.1).

#### Sean — the producer (bench)
- **Role:** PhD student in the **Vogel Lab** (quantitative imaging); does quantitative time-lapse imaging.
- **Tool:** Obsidian + Discourse Graph plugin — a new experiment node takes *"10 seconds"*; raw images
  sit on a **25 TB lab array**, never in the graph.
- **Wants:** to share the *result* with Kate's lab while keeping control — *"some kind of granularity"*
  so his **personal thoughts stay private** and his 80 GB of raw stacks stay put.
- **Defining moment:** he throws the bundle together live and walks Kate's group through *"the
  collaboration between Kate's lab and my lab, Vogel Lab"* — sample, perturbation, two components forming and
  dissolving (Jun 8).

#### Kate — the consumer (PI, and consortium lead)
- **Role:** PI of a collaborating cluster lab; runs **multiple collaborators across multiple
  projects** — *"if I can do this with Sean, then I can do it with all my collaborators."*
- **Tool:** **a web browser.** No Obsidian, no plugin.
- **Wants:** *"access everything in one place and at any time… searchable… and we can look at it three
  years from now."* For the result itself, the **summary is usually enough**.
- **Defining quote:** *"as the PI, often that will be sufficient… I trust you."*

#### A grad student in Kate's lab — the reproducer
- **Role:** the person who will actually build on Sean's result.
- **Tool:** the dashboard.
- **Wants:** *"to know how you did it"* — concretely, *"what segmentation threshold did Sean use,
  because the thresholding can change all the measurements,"* so they traverse to the **image-analysis**
  node and the **CSV**.
- **Defining moment:** following the edge from the figure back to the method, then **sending a Request**
  for a follow-up experiment.

#### A public / funder visitor — the dashboard-as-website reader
- **Role:** a lab-website visitor, a prospective student (recruiting), or a funder checking progress.
- **Tool:** the public dashboard.
- **Wants:** a **preset view** pointed at them — open **questions/requests** for recruiting, or
  **experiments-in-progress** *"for the funder seeing progress"* — and a **plain-language** summary
  legible to *"a 60-year-old-plus professor."*

> **Naming note.** Sean and Kate (and Kate's student) are *scientists* and first-class actors. KOI, the
> sync plugin, the schema, and the dashboard build are **components** ([§8 of AGENTS.md](../AGENTS.md#8-architecture--stack)),
> not actors. A second platform/lab ("…and it scales to Anton's tool") is an extension, not the spine.

---

## 2. The spine — two currents, onto a shared read-only surface

The QSI/world story has **one** current (producer → public). This tier has **two**, exactly as the
canvas draws them — *Sean's notebook → Kate's lab → a shared web interface*, with a `Request for study`
node sitting in Kate's column (_grounding-dashboard.md §2.4):

- **Results out.** Sean publishes a *selected subgraph* — *"the results / evidence and associated
  [experiments]"* — to the dashboard. It is a **subset chosen from his vault**, not the whole graph.
- **Requests back.** Kate's lab can **claim an open request** or **send a Request** for a new
  experiment/analysis. *"We just have to create a request type in discourse graph."* On the recipient
  side: *"notification of updates… seeing the summary… importing connected nodes."*

**The schema, compactly** (MIRA — the same grammar as every other tier; AGENTS.md §4). At this tier the
**`Request` node is first-class** (it is peripheral in the QSI case):

> **Question · Claim · Evidence · Study · Protocol · `Request`**

```
Claim     --addresses-->          Question
Evidence  --supports / opposes--> Claim
Study     --grounds-->            Evidence      # the producers call the Study the "experiment"
Study     --follows-->            Protocol       # e.g. the segmentation threshold lives here
Request   --request_for / request_target-->      # Kate's lab → a follow-up Study / Analysis  (the reverse current)
```

Two rules ride this spine and are never violated:
- **Pointers, not payloads** — the analysis is a git+commit pointer, the data a curated-CSV pointer, the
  80 GB stacks a *request-access* pointer to the lab array; *"no raw data in here whatsoever"* (R2).
- **Read-only, and private stays private** — the dashboard renders a *published subset*; *"data marked
  private must not become public when routed through KOI"*; sharing one node never leaks a connected
  private one (R5).

---

## 3. The moments

When Sean shares, the question is *"will my collaborators understand it without me in the room, and can
I share it without losing control of my data and my half-formed thoughts?"* The story has five moments,
each pointed at the mockup that depicts it (AGENTS.md §6; _grounding-dashboard.md §2-3).

The worked content throughout: Sean's `Evidence` — *Component A is recruited to forming clusters
before Component B* — grounded in a live-imaging `Study` of perturbed samples, following a preparation →
perturbation → imaging → **segmentation** `Protocol`, captioned *illustrative · unpublished — workshop
prototype*.

### Moment A — Sean shares a *selected subgraph* to the dashboard
*He turns part of his private vault into a published, read-only view — on his terms.*
→ **mockups/06-share-to-dashboard.html**

1. **He selects the subgraph, not the graph** (mockup 06). In Obsidian he picks the `Evidence`; the
   plugin proposes the self-describing bundle — `Evidence` + grounding `Study` + `Protocol` + pointers —
   and *offers to include connected nodes*.
2. **He sets visibility per node** (R5). The bundle and the analysis/CSV pointers go to the dashboard;
   the **80 GB raw stacks stay request-access-gated**; his **"personal thoughts" node is withheld** —
   present in his vault, invisible (and un-leaked) on the dashboard.
3. **He chooses the audience:** **password-protected** (Kate's consortium) now, **public** later
   (the lab website). *"Easier to open a closed system than to close an open one."*
4. **Candidate is allowed.** A still-informal **candidate** node can ride along, **shared as-is** —
   *"somebody else can formalize it or just see it as it is."*
5. **He pushes.** KOI — *"a specific expressway,"* not the open internet — carries the MIRA JSON-LD to
   the dashboard; shared nodes arrive **visible but not editable.**

### Moment B — Kate opens the dashboard and trusts the result
*The PI gets the big idea in one glance, in a browser, with no tooling.*
→ **mockups/07-shared-dashboard.html**

1. **It's a web page at a URL** (R6) — *"a read-only web view of discourse-graph data."* Kate opens it;
   nothing to install.
2. **She picks a view.** **Graph** (*"move the nodes around… understand what questions the group is
   trying to answer"*), **Kanban**, or **Table** (*"left is the question, the next column the
   experiments, then results"*). A **preset** — *Results centered* — puts Sean's finding front and center.
3. **She glances and trusts** — the summary plot (Component A before Component B) + the provenance byline *Sean ·
   Vogel Lab*. *"As the PI, often that will be sufficient."*
4. **It's legible and durable** — colorblind-safe, readable by *"a 60-year-old-plus professor,"*
   searchable, and still here *"three years from now."*

### Moment C — A grad student traverses to the threshold
*The reproducer asks the specific question and follows the lineage to the answer.*
→ **mockups/08-traverse-and-request.html**

1. **From the figure, back to the method** (R4). *"What segmentation threshold did Sean use?"* — they
   follow `Evidence` ←`grounds`← `Study` →`follows`→ `Protocol`, down to the **image-analysis /
   segmentation** node, *"because the thresholding can change all the measurements."*
2. **They follow the pointers** (R2, R7). The `git` chip opens the analysis at the exact commit; the
   `data` chip opens the curated **CSV**; resolution crosses **system boundaries** (vault → GitHub →
   array).
3. **They watch the walkthrough** — the `video` pointer orients them the first time, *"without a Zoom
   call,"* re-watchable later.
4. **They hit the gate, gracefully** (R5). The 80 GB raw stacks read *request access*; nothing
   un-shared leaks. *Why it matters:* *"three months later there's a bug in your segmentation… you need
   to know everything downstream and recompute it"* — provenance is the point.

### Moment D — The request goes back
*The collaboration's reverse current — the thing the QSI tier doesn't have.*
→ **mockups/08-traverse-and-request.html**

1. **They send a `Request`** for a follow-up — *"a request for [experiment, analysis]"* — attached to
   Sean's `Evidence`/`Study` (`request_for` / `request_target`).
2. **Sean is notified**, sees the summary, and can **claim** it; updates flow back to the dashboard.
   Results out, requests back — on one shared surface.

### Moment E — Kate runs it as a consortium (and a recruiting page)
*One dashboard, many collaborators, several audiences.*
→ **mockups/09-consortium-view.html**

1. **One project umbrella, many collaborators** — *"if I can do this with Sean, I can do it with all my
   collaborators… multiple projects."* Selective visibility per collaborator/project.
2. **Preset views per audience** — *experiments-in-progress* for a funder; *questions & requests* made
   **public** for **recruiting** (*"making requests publicly… is onboarding like recruiting"*).
3. **Unpublished, one-off results become discoverable** rather than orphaned — surfaced on a durable,
   searchable, permissioned surface instead of dying on a hard drive.

---

## 4. Journey maps

### The PRODUCER journey — Sean
*author → select the subgraph → set per-node visibility + audience → push → it renders read-only.*

| Stage | What Sean does | Feels like / rule |
|---|---|---|
| **Author** | Captures `Question → Claim → Evidence → Study → Protocol` in Obsidian; ~10s per experiment. | Frictionless; rides the existing flow (AGENTS.md §7). |
| **Select the subgraph** | Picks the `Evidence`; plugin proposes the bundle + offers connected nodes. | A *subset of the vault*, self-describing (R4). |
| **Set visibility** | Bundle + analysis/CSV → dashboard; raw stacks **gated**; personal note **withheld**. | Permissions without leaks; private stays private (R5). |
| **Choose audience** | **Consortium (password)** now; **public** later. | Start closed; open on purpose. |
| **Push** | KOI carries MIRA JSON-LD; nodes arrive **visible, not editable**. | Pointers not payloads; schema-agnostic transport (R2, R10, R12). |
| **Live** | The subgraph renders **read-only** at a URL; updates propagate. | Addressability is the deliverable (R6). |

### The READER journeys — Kate's lab (R4 made concrete: one bundle, three depths)

**Branch 1 — the PI (Kate).** Opens the dashboard, picks *Results centered*, reads the summary plot +
provenance byline, **trusts it**. No traversal (mockup 07).

**Branch 2 — the reproducer (grad student).** Traverses `Evidence → Study → Protocol` to the
**segmentation threshold**; resolves the `git`/`data` pointers; watches the walkthrough; **requests
access** to the raw stacks only if re-running; **sends a Request** for a follow-up (mockup 08).

**Branch 3 — the public / funder.** Hits the *public* dashboard with an audience **preset** —
**questions/requests** (recruiting) or **experiments-in-progress** (funder) — and a plain-language
summary (mockups 07, 09).

> **Why the fork matters:** publish one subgraph; satisfy the trusting PI, the skeptical student, and
> the outside visitor — *summary for the glancer, lineage for the reproducer, a preset for the
> stranger* (R4). One push; many readers.

---

## 5. How the mockups map to the story

| Mockup file | What it shows | Rule / insight it embodies |
|---|---|---|
| `mockups/06-share-to-dashboard.html` | **Sean shares a selected subgraph** from Obsidian: the proposed bundle, **per-node visibility** (gated raw stacks, **withheld personal note**), **public vs. password-protected** audience, a **candidate** node riding along, push via KOI. | R1, R2, R4, R5, R12; candidate-node baby-step; subset-of-vault. |
| `mockups/07-shared-dashboard.html` | **The shared read-only dashboard** at a URL — **Graph · Kanban · Table** views, **preset views** (results / experiments-in-progress / questions-&-requests), node cards with provenance and a **public/consortium** badge; Kate's **glance-and-trust**. *(The headline screen.)* | R6, R8; read-only viewing surface; accessibility; the dashboard scope. |
| `mockups/08-traverse-and-request.html` | A grad student **traverses to the segmentation threshold**, follows `git`/`data` **pointer chips** and the **video** walkthrough, hits the **request-access** gate on the 80 GB stacks, and **sends a `Request`** back. | R2, R4, R5, R7; the **request-back** current (Request first-class). |
| `mockups/09-consortium-view.html` | **Kate as consortium lead:** one project umbrella, **many collaborators**, **password-protected** vs **public** surfaces, **preset views per audience** (funder / recruiting), and **unpublished results made discoverable** instead of orphaned. | R5, R6, R8; multi-collaborator permissions; public-as-recruiting. |

---

## 6. Acceptance — "done when" (dashboard track; complements AGENTS.md §9)

For the worked Sean → Kate cluster case:
1. **Share path.** Sean publishes a **selected subgraph** (`Evidence` + grounding `Study` + `Protocol`,
   each with a **pointer**) from Obsidian to a **read-only dashboard at a URL**, setting per-node
   visibility (raw stacks gated; personal note withheld, **not leaked**).
2. **Glance path.** Kate opens the dashboard in **a browser, no DG app**, picks a **view/preset**, and
   reads the result cold from summary + provenance.
3. **Traverse path.** A grad student follows the lineage to the **segmentation threshold**, resolves the
   `git`/`data` pointers across system boundaries, and watches the video walkthrough.
4. **Gate path.** The 80 GB raw stacks are **request-access-gated**; access can be requested; **no
   private/un-shared node leaks**.
5. **Request path.** Kate's lab **sends a `Request`** for a follow-up experiment/analysis; Sean is
   notified and can **claim** it; the dashboard reflects it.
6. **Consortium/public path.** The same dashboard runs **password-protected** for the consortium and
   **public** as a lab website, with **audience presets**.

---

## 7. Out of scope (v1)

- **Writing/contributing from the dashboard** — it is **read-only**; authoring stays in the vault.
- **Hosting or transferring raw data** — referenced only; the raw-stack pointer is access-gated.
- **Non-text assets as first-class schema fields** (CSV/image/binary) — **flagged as a gap**, deferred.
- **Conflict resolution / graph merge** when labs diverge — design to allow later; don't build now.
- **Expanding the grammar** to Analysis/ELN/Data node types — deferred, as in the QSI case.

## 8. Open questions carried into design

Surfaced, **not solved** (AGENTS.md §10; _grounding-dashboard.md §5):

- **Candidate-node formalization:** approval/notification when someone else formalizes your shared
  candidate node.
- **Accounts & gating atop weak KOI:** enforcing password-protected consortium views and per-resource
  request-access.
- **Preset-view set:** which presets ship, and whether they are presets or saved layouts.
- **Non-text assets:** how CSVs/images become referenceable schema fields, not embedded content.
- **Provenance standard & invalidation:** transporting provenance to a standard so a downstream
  recompute can be triggered when a method changes.
- **Conflict/merge** semantics across independently-authored graphs.

---

*MIRA × Discourse Graphs · push results to a shared web interface · Sean → Kate **dashboard** (central) · draft v0.1*
