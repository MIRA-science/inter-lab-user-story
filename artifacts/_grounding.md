# Grounding pack — 🥉 Low shared context: push result to a public repository or database

> **Note:** this pack grounds the **public extreme** (QBI → the world). The repo's **central** use case —
> the **Sean → Kate dashboard** — is grounded in [`_grounding-dashboard.md`](./_grounding-dashboard.md).

> A **variant of the inter-lab user story** for the *lowest* rung of the shared-context ladder.
> Where the inter-lab story (`../lab-to-lab/`) is **P2P, Sean → a known Kate**, this
> variant is **producer → the public / anyone in the world**, with little-to-no shared context and
> often **no shared tooling** on the receiving end.
>
> **Sources** (do not contradict): the Roam dg-team graph — the bronze use-case node
> `🥉Low shared context: push result to a public repository or database` (block `pOEtpyGbH`,
> embedded under `[[ISS]] - Document initial inter-graph use cases`, uid `FOQKen3Dd`), the
> `UserPilot/Quantum biology institute` pilot page, `[[Inter-graph Use Cases]] to design MVP around`
> (uid `qYphLOmYp`), `Initiative/Inter-graph functionality` (uid `vE2hKuUq0`), `[[FLO]] - Push node
> to shared database` (uid `E8_V-gWIt`) — plus the canonical schema (`../../schema/mira.yaml`), the
> MyST↔DG interop spec (`../../myst-dg-interop/context/discourse-graphs-myst-spec.md`), and two canvas
> sketches (`../low-context-user-story.png`, `../../myst-dg-interop/MyST-DG user story.png`). Where this file and the
> inter-lab `AGENTS.md` rules (R1–R10) interact, the rules still hold; this variant only changes the
> *recipient* and the *destination*, not the data model or the pointers-not-payloads discipline.

---

## 0. One-line story (the variant)

A data **producer** (a researcher in their personal Obsidian discourse graph — concretely Brian &
Morgan at the **Quantum Biology Institute**) wants to **publish a result to the open world**: the
`Evidence` (claim + key figure) with *just enough lineage* — the analysis that produced it, the lab-
notebook entry that describes it, and a pointer to the data — as a small, **independently
addressable** bundle that lands in a **public repository or database** (a Jupyter Book
micropublication / desci node / nanopub, or the DG web database) and is reachable at a **URL** by
**anyone**, including readers who have **no discourse-graph tooling at all**.

There is **no specific consumer** and **no established relationship**: the reader is a stranger, a
reviewer, a future collaborator, or **the producer's own future self** citing the result from a new
notebook. The consumer→producer "Request back" current of the inter-lab story is **weak or absent
here** — the dominant flow is one-directional, **producer → public** (cf. `[[FLO]] - Push node to
shared database` (`E8_V-gWIt`): "export your result, choose to send it to a nanopub site / consortium
/ reviewer").

---

## 1. Where this sits — the shared-context ladder

The Roam `[[ISS]]` page bins inter-graph sharing into **levels of shared context** (block `U_3yH8WCx`),
and `[[Inter-graph Use Cases]] to design MVP around` turns those into tiers. This use case is the
**bronze / lowest** tier:

| Tier | Use case (Roam title) | Recipient | Shared tooling |
|---|---|---|---|
| 🥇 High | push evidence bundles **to a shared graph** (`PP1mMVO09`) | established collaborator, working relationship | both run a DG |
| 🥇 High | push **between your own graphs/platforms** (`WoZqynjBQ`) | the *same person* in two graphs | yes |
| 🥈 Medium-high | push **between graphs and platforms** (`4UXkCArxd`) | shared interests; may/may not communicate | partial |
| 🥉 **Low** | **push result to a public repository or database** (`pOEtpyGbH`) | **everyone in the world** | **often none** |

The inter-lab user story (`../lab-to-lab/`) covers the **gold/silver** end (import into a
DG app; its "Scenario B — view on the web" already reaches toward bronze). **This variant is the
bronze end's first-class write-up.** Key consequence: with no shared context, **the bundle must
stand entirely on its own**, and trust comes from *visible provenance + reproducibility*, not from a
prior relationship.

---

## 2. The USER STORY

### 2.1 The cast

The inter-lab story stars Sean (Vogel) → Kate. This variant keeps the **producer archetype** but
swaps the recipient for *the public*. The canonical pilot makes it concrete:

| Role | Who | Tool | In the story |
|---|---|---|---|
| **Producer (PI/lab)** | **Morgan** (Quantum Biology Institute) | Obsidian + DG plugin → Jupyter Book | Owns the result; wants it published as a FAIR micropublication; "would love the pub to be **interactive**, perhaps like distill/idyll" (`W13C7qjJ7`). |
| **Producer (bench)** | **Brian** (QBI) | Obsidian + DG plugin | Actually authored the evidence bundle; will make the next one for figure 3 (`zCdAid-Iw`). |
| **Reader — has no DG app** | a reviewer, a clinician, a prospective collaborator, or **Kate before she installs anything** | a web browser | Lands on the published figure; wants to drill from figure → code → notebook → data, "in one space, in one spot" (`XSOMnSUyq`). |
| **Reader — future self / citing author** | any researcher | their own notebook (MyST/Obsidian/Roam) | "I want to **link/cite specific results** in my own notebooks" (`TAXQ_j36-`); wants the version they cited *frozen*. |

> **Naming note** (per the inter-lab convention): Brian & Morgan are QBI *scientists* and are
> first-class actors. The facilitation/infrastructure people around the pilot — Joel (DG team),
> Marc-Antoine (schema), Elli (connector), and the user (Matt) — are **roles/components, not
> user-story actors**.

The **pivotal insight is the same** as the inter-lab story but aimed at strangers: a reader wants
*the big idea up front* and *traversal on demand* — except here we cannot assume the reader knows the
producer, so each layer must be self-explanatory.

### 2.2 The worked example (the bronze analog of Sean's stress-granule)

> Real canonical test case from the QBI pilot. **Magnetosensitivity of microbial life** — part of
> "multi-omics of magnetosensitivity in microbial model organisms," led by Michael Montague (J. Craig
> Venter Institute); QBI public roadmap at quantumbiology.eco. Mark figures *illustrative ·
> unpublished — workshop prototype*.

- **Claim / model** — the *radical-pair quantum model* of magnetosensitivity (multiple EVDs support it).
- **Evidence (EVD)** — *"The sign of the mean calculated magnetic field effect (MFE) for
  **MagLOV2-expressing E. coli** flipped from positive to negative as magnetic field strength
  increased at low fields."* (`fY8iScuZG`)
- **Key figure / data artifact** — mean MFE at 0.5, 1.0, 1.5, 2.0, 2.5 mT (1 experiment per
  condition, **74 colonies each**; SEM with fit error, cycle and biological variation propagated)
  (`D8z_73L0N`); plus a representative single-colony trace, positive MFE at 1.0 mT vs negative at
  2.5 mT (`5F-5TSQfb`).
- **"Experiment" = 5 imaging sessions on 5 days at 5 field strengths** (`cDBsiE_BH`) — note the
  unit-of-experiment subtlety.
- **Analysis (ANA)** — the `2_curated_datasets_analyses` subfolder for that figure: plot, code to
  make the plot, data to make the plot.
- **Lab notebook (ELN / "EXP")** — physical procedures across the 5 sessions + the **caveats / "juicy
  gossip"** (e.g. "we realized we'd done these with the field turned on for the first section…").
- **Data (Dataset)** — raw TIFF stacks (`NDTiffStack_rois.zip`) and pipeline outputs per run; **how/
  whether to archive this is explicitly unresolved** ("punt dataset to later", `pBBuT3NjB`).

### 2.3 The reader's progressive-disclosure ladder (this tier's defining UX)

Unlike the inter-lab story's `Evidence + Study + Protocol`, the bronze pilot frames the bundle as a
**reader-facing ladder** with *publication* node types — **EVD → ANA → ELN → Data** — captured as a
"what the reader needs at each level" table (`PwzU8PLGb` / `FuNnInMAP`):

| Level | What the reader wants | Quote from session |
|---|---|---|
| **EVD** | the claim + the key figure | "I hover on the figure panel" (`nrn78cLD_`) |
| **ANA** | reproducibility — can I rerun this plot? | "if people want to reproduce the figure, or actually look at the code, they want this" (`c5VfhOdyg`) |
| **ELN** | the "juicy gossip": how the experiment *really* went | "all the juicy gossip about how the experiment really went down, and the associated caveats" (`rI4on4WgF`) |
| **Data** | just the spreadsheet *for this graph*, not 50 CSVs | "maybe they want a spreadsheet of the data just in that graph" (`6EN2QNsZh`) |

The three user-story beats (all `refs`-tagged on the bronze node):
1. **Publish** — "As a researcher, I want to **publish my result alongside the associated plot,
   analysis scripts, and point to the relevant lab-notebook entry**" (`f66QSB2K7`).
2. **Read cold** — "As a reader, when I view a plot/figure, I want to **access the methods, example
   data, ELN entry as additional context**" — "jump directly to the README… here's all the code"
   (`3x26_Z7fH`, `p5Fuj1djy`).
3. **Compose a narrative** — "As a researcher, I want to **post individual results as I go along and
   then compile them into a longer narrative**" (`aktn-uDCU`).

### 2.4 The "specs instead of papers" narrative workflow

A distinctive arc for this tier (`MwqKrxxoq`): **individual bundles are published first** → **claims
emerge across bundles** (the "MFE sign flips" claim isn't inside any one EVD; it sits above them) →
**bundle them once the story is clear** → **AI drafts the narrative** (point Claude at the collection
+ spec) → **human edits** → **publication compiles** (a Jupyter Book that *points to / contains* the
still-independently-addressable bundles). Joel's framing: the publication is "**a rendered view of
structured data** — the evidence bundles ARE the publication, the narrative is just one rendering"
(`3HQWydFb3`); Morgan: make it interactive (distill/idyll). In the QBI pilot the write-up was drafted
in **Overleaf with Claude** from Clarice's template (`maDyM5aXY`).

### 2.5 The two destination flavors (both are "public repository or database")

- **(a) The DG web database / shared web interface.** Push DG nodes to a database that renders each
  node as a **web page at a URL** (canvas `../low-context-user-story.png`: "**KOI DG nodes to
  dashboard/kanban on web interface**", MIRA database, public URL, permission/account). This is the
  `[[ISS]] - Share discourse nodes on the Web through the database` track — "we need a URL", "markdown
  rendered as HTML is ok for v0", v1 adds the composite/bundle (`Initiative/Inter-graph functionality`).
- **(b) External public repos / publishing.** Obsidian → **Jupyter Book** micropublication / **desci
  node** object / **nanopub** site (`[[ART]] - Cosmik`, `[[ART]] - experiment.com`), optionally a
  **PREreview** request (`[[FLO]] - Push node to shared database`). The QBI pilot is flavor (b).

---

## 3. USER CONSTRAINTS

What the humans need, expect, or won't tolerate (rooted in behavior/experience, not infrastructure):

- **Don't break the 5-minute frictionless workflow.** Authoring stays in Obsidian; publishing must
  ride the *existing* workflow. The pilot's emergent pattern: a **"document/pub" container that acts
  like a README** — a single linkable thing that points to a whole package of files — felt natural to
  Brian & Morgan "in their normal workflow" (`UlM6_ljlH`, `9zS-UA2gH`).
- **Big idea up front, lineage on demand — for a stranger.** Same as inter-lab R4, but the reader
  has no prior trust, so every level (§2.3) must be interpretable cold.
- **Reproducibility is a first-class reader need** (the ANA level): a reader must be able to *rerun
  the plot*, not just see it.
- **Honesty / caveats belong in the open** (the ELN level): the "juicy gossip" and caveats are
  *wanted*, not hidden — a notable contrast with polished papers.
- **Scoped data, not data dumps**: "a spreadsheet of the data *just in that graph*, not 50 CSVs."
- **Publish-as-you-go, then bundle**: results are posted individually and only later compiled into a
  narrative; bundles must remain **independently addressable** after compilation.
- **Trust through provenance**: with no relationship, the reader trusts via visible authorship/source
  on the page (cf. inter-lab "see who shared this with me").
- **Cite/link specific results** from one's own notebook, and **keep the cited version frozen**
  ("we've frozen it; it will be this way for as long as GitHub exists" — versioning, `JdWqM-G1p`).
- **"Even public needs permissions."** Readers should be able to **easily request access** to
  deeper/gated nodes or **full permission** (`jeZDvzfjZ`, `I3-vZ9iSw`); a dashboard/kanban + account
  is on the canvas tasks (`../low-context-user-story.png`).
- **Make it feel like a publication, ideally interactive** (distill/idyll), not a raw data dump.
- **Bundle creation today is clunky** — the pilot flagged streamlining the evidence-bundle flow as an
  `#iss-candidate` (`-NRuxOLhQ`); "really, Brian did it with guidance from Joel."

---

## 4. TECH CONSTRAINTS

What the systems, formats, schema, and infrastructure impose or block:

- **Pointers, not payloads (R2), at the extreme.** Raw data (TIFF stacks, ~large) lives in **a third
  system entirely** — a university drive/archive, "maybe access-gated" (`7kGaXJJer`). Local-disk
  pointer resolution across labs is still **open**.
- **Links must resolve across *system boundaries*** (`ccg0stkWb`). The pilot's architecture is three
  heterogeneous stores, and relations point to **URLs, not records** in the next store:
  ```
  ATProto / KOI            Jupyter Book                 Raw data store
  EVD record           →   ANA chapter (URL)        →   Analysis folder / TIFFs
  (published,              ELN chapter (URL)            (large, maybe access-gated)
   addressable hub)        (published, readable)
  ```
  The **EVD is the hub record** on ATProto (figure + relations); ANA/ELN are **Jupyter Book chapter
  URLs**; data is a third system (`W6uHRiA48`–`7kGaXJJer`).
- **Addressability is the v0 deliverable.** "We need a URL" (`tobVQ7-FZ`); each DG node displayable as
  a **web page with a URL** (Semble); node packaged as **JSON, URIs as RID or URL** (KOI); schema
  readable as **RDF** (LOD/nanopub) and **ATProto lexicon** (`Initiative/Inter-graph functionality`).
  **v0 = markdown rendered as HTML**; **v1 = the composite/bundle**.
- **⚠ Schema gap — the bronze vocabulary is not (yet) the canonical schema.** This is the single
  biggest tension to surface:
  - `../../schema/mira.yaml` defines only **Question · Claim · Evidence · Study · Request · Protocol**,
    with relations **`grounds`/`is_grounded_in`** (Study↔Evidence) and **`follows`** (Study→Protocol).
    There is **no `Analysis`/ANA, no `ELN`/lab-notebook, no `Data` node type**.
  - The bronze pilot uses **EVD / ANA / ELN / Data** with relations **`produces`** (ANA→EVD),
    **`describes`** (ELN→EVD), **`interpretedFrom`** (EVD→data artifact), **`derivedFrom`** (EVD→EXP)
    — *none* of which are MIRA slots. The team even noted the directions are UX-driven shortcuts:
    "ANA `produces` EVD… i knowww sorry Marc-Antoine, easier from a UX pov to point to the EVD"
    (`DE7ap2_r4`); "ELN should really `describes` ANA" (`NSrrTS6fr`); with an inference rule to
    recover the intended graph (`_wSRrZbOK`).
  - The **publish target supports even fewer types**: the MyST/Jupyter-Book spec
    (`../../myst-dg-interop/...`) is **Phase-1 = `claim` + `evidence` + `figure` only**, relations
    `supports/opposes/informs/grounds`; **custom node types are explicitly out of scope**. So
    EVD/ANA/ELN/Data must be **mapped/flattened** to publish — lossiness and mapping are open.
  - ⇒ **Schema-agnostic transport (R6) matters here most.** The mapping between bronze EVD/ANA/ELN/Data,
    canonical Evidence/Study/Protocol, and the MyST claim/evidence/figure is **unsettled** and is the
    core data-model work for this tier (`Project/Inter-graph data model`, `ENG-1543`).
- **Relations: source-of-truth is migrating.** Today relations live in **Obsidian YAML**
  (`interpretedFrom`, `derivedFrom`); after `Project/Reify relations in Obsidian` they move to
  **`nodes.json` / `relations.json`** (`9ZHKGabKV`, `OJzTEIynR`). **IDs, not labels, are the stable
  handle** — "what matters is the ID of the thing… you can add them to the body of the Jupyter Book"
  (`edsyvZqfn`). Dragging YAML→canvas already caused an accidental relation-key overwrite (`suz3OUtlI`).
- **The key figure needs to become a linkable data artifact with an ID** — not yet a clean UX
  ("wasn't obvious when/how to make the figure itself a linkable thing… doesn't make sense to make a
  README for a single key figure"; hooks into `[[FLO]] - Set/customize key figure for node`,
  `4eTWArOx7`).
- **KOI is "composing records that reference other records,"** not just "share a node" (`LtHKDNv97`);
  this is the **composite/nested evidence-bundle** ("desci node object" that archives the figure +
  code + data + ELN link).
- **Stack** (for context; UX layer stays above it): Obsidian DG plugin → **Supabase** → **KOI** (RIDs);
  desci nodes; **ATProto**; nanopub (Cosmik, experiment.com); **Jupyter Book / MyST**; PREreview.
  **KOI's native permissions are weak** — public-but-gated access needs work on top.
- **AI in the loop**: draft the narrative from the bundle collection (Claude), LaTeX assistance,
  and a **publish-time modal/interview** to formalize & contextualize (`[[FLO]] - Push node to shared
  database`: "maybe even a conversation with a chatbot to collect contextual/narrative info").

---

## 5. Open questions carried into design (surface, don't "solve")

- **Schema mapping**: how do EVD/ANA/ELN/Data reconcile with MIRA Evidence/Study/Protocol and with the
  MyST claim/evidence/figure subset? What is lost on publish? (the central one)
- **Dataset archiving**: how/whether to archive & share the raw data — *unresolved by the pilot itself*.
- **Figure-as-artifact**: how a sub-panel becomes an addressable node with an ID.
- **Dangling references**: when publishing one node, carry its relations as a *signal* of missing
  resources, or *hide* them? (Matt leans signal; Holehouse hides — `NP7h6eqbi`/`davTGxtg4`.)
- **Versioning**: freeze the version the reader/citer referred to.
- **"Even public" permissions**: request-access / accounts atop KOI's weak permissions.
- **Pub vs micropub** naming, and whether the publication is a document or "a rendered view of data."
- **Cross-tool validation**: Obsidian ↔ Jupyter Book/MyST ↔ Roam ↔ ATProto via the JSON-LD/JSON bridge.

---

## 6. How this differs from the inter-lab (gold/silver) story — quick contrast

| | Inter-lab story (🥇/🥈) | This variant (🥉) |
|---|---|---|
| **Recipient** | a *known* Kate's lab | *anyone in the world* / future self |
| **Shared context** | established collaboration | little/none |
| **Shared tooling** | both run a DG | often none (a browser) |
| **Destination** | KOI → Kate's graph (import) | public repo/DB: Jupyter Book, desci node, nanopub, DG web DB |
| **Bundle vocabulary** | Evidence + Study + Protocol (`grounds`, `follows`) | EVD + ANA + ELN + Data (`produces`, `describes`, `interpretedFrom`) |
| **Trust basis** | the relationship + provenance | visible provenance + reproducibility (no relationship) |
| **Reverse current** | first-class `Request` back | weak/absent; flow is one-directional |
| **Worked example** | HeLa stress-granule (G3BP1/PABP1) | MagLOV2 magnetic field effect (QBI) |
| **Pivotal new mechanic** | cross-graph make-edge + permissions | cross-*system-boundary* link resolution + addressable bundle |

---

## 7. Provenance (Roam dg-team UIDs + files)

- **Bronze node**: `pOEtpyGbH` ("🥉Low shared context: push result to a public repository or
  database"), under `[[ISS]] - Document initial inter-graph use cases` (`FOQKen3Dd`; the TODO
  pointer is `vnuXmM8D2`). Reader-ladder tables `PwzU8PLGb`/`FuNnInMAP`; story beats `f66QSB2K7`,
  `3x26_Z7fH`, `aktn-uDCU`, `TAXQ_j36-`; narrative workflow `MwqKrxxoq`; "specs not papers" `TlIem9BKI`.
- **Pilot**: `UserPilot/Quantum biology institute` — evidence-bundle win `Jik1z-xgT`; FAIR-ification
  plan `Yq30MWqRi`; observation `fY8iScuZG`; figure `D8z_73L0N`/`5F-5TSQfb`; experiment-unit `cDBsiE_BH`;
  write-up `maDyM5aXY`; publish `XJqd4huAy` (`ys7KXitoe`, `fMN0se_Vq`, `edsyvZqfn`); resources `8UDDPcGE_`.
- **Tiers**: `[[Inter-graph Use Cases]] to design MVP around` (`qYphLOmYp`) — `PP1mMVO09`, `WoZqynjBQ`,
  `4UXkCArxd`. Levels-of-context binning `U_3yH8WCx`. Considerations `n_Xm2FFJE`.
- **Flow**: `[[FLO]] - Push node to shared database` (`E8_V-gWIt`).
- **Web/DB & addressability**: `Initiative/Inter-graph functionality` (`vE2hKuUq0`) — `[[ISS]] - Share
  discourse nodes on the Web through the database` (`Hig8BV1OQ`); v0 notes `tobVQ7-FZ`/`CI_BvHtBT`.
- **Schema**: `../../schema/mira.yaml` (Question/Claim/Evidence/Study/Request/Protocol; `grounds`,
  `follows`, `request_for`, `request_target`); `../../schema/discoursegraphs.yaml` (`dg_Evidence` slots).
- **MyST/Jupyter Book**: `../../myst-dg-interop/context/discourse-graphs-myst-spec.md` (Phase-1
  claim/evidence/figure only).
- **Canvases**: `../low-context-user-story.png` (web-interface destination), `../../myst-dg-interop/MyST-DG user story.png`
  (MyST/nucleus variant).
- **Inter-lab parent story** (for the variant contract & rules R1–R10): `../lab-to-lab/AGENTS.md`,
  `../lab-to-lab/artifacts/ux-user-story.md`, `../lab-to-lab/artifacts/_grounding.md`.

---

*MIRA × Discourse Graphs · inter-graph exchange · 🥉 low-shared-context variant · context pack draft v0.1*
