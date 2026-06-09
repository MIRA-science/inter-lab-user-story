# User-research plan — Inter-lab exchange of results & requests

> Companion to [`_grounding.md`](./_grounding.md) and [`_design-brief.md`](./_design-brief.md).
> Validates and refines the user story in [`../AGENTS.md`](../AGENTS.md) (Draft v0.1).
> Mockups referenced by relative path live in [`./mockups/`](./mockups/): `01` share / subgraph-selection
> dialog · `02` Evidence node · `03` import into a DG app · `04` view on the web · `05` Request
> lifecycle (per `_grounding.md` §8 reception modes and the [`_design-brief.md`](./_design-brief.md) screen set).
> Data in every session is **unpublished** — use the illustrative stress-granule example
> (`_grounding.md` §3), labelled "illustrative · unpublished — workshop prototype."

---

## 1. Research goals & the core question

**Core question we are really testing:**
> **What level of context / permission / tooling is *sufficient* for each mode of sharing?**

Sharing is not one act. A producer (Sean) chooses among three reception modes (`_grounding.md` §8): **import into a DG app** (high shared context), **view on the web** (no shared tooling — the added scenario), and **request back** (the collaboration primitive). Each mode places a different bar on three sliders — how much *context* travels, how much *permission* must be set, how much *tooling* the recipient runs. This research finds, per mode, the **minimum sufficient** point on each slider.

**Decisions this research informs:**

| # | Decision | Tied to |
|---|---|---|
| D1 | The **default "send" closure** — how much connected context auto-travels, and how a recipient pulls a referenced-but-not-included node (e.g. a Protocol two hops away). | AGENTS.md §10 (send closure); R4 |
| D2 | The **permission granularity model** the UI should expose — node vs. subgraph vs. per-project default — and whether users can operate it without leaking un-shared nodes. | AGENTS.md §10 (permission granularity); R5 |
| D3 | **Dangling references: signal vs. hide.** When a shared relation implies an un-shared node, do we show a "1 related node not shared" indicator (signal) or hide it entirely (Matt leans signal; MAP prefers hide)? | AGENTS.md §10; Roam "considerations"; R5 |
| D4 | Whether the **web view (mockup 04)** lets a no-DG recipient understand a result *cold*, trust it, follow pointers, and convert to access-request / later-import. | AGENTS.md §9; `_grounding.md` §8 mode 2 |
| D5 | Whether the **Request lifecycle (mockup 05)** is understood as "doesn't-exist-yet" (not a comment / data-request / permission-request) and whether create→claim→fulfil→notify is legible. | AGENTS.md §6, §10 |
| D6 | **Authoring friction** — does share-time prompting (add summary / record walkthrough / format-in-schema) help or stall adoption ("click-overhead")? | AGENTS.md §7; R10 |
| D7 | The **levels-of-shared-context binning** — does Sean's mental model match the five Roam levels, and do they collapse cleanly to three UX modes? | Roam `[[ISS]]` levels; `_grounding.md` §8 |
| D8 | Out-of-scope-but-watch: **local-disk pointer resolution**, **versioning**, **dedup / merge** — surface user expectations so v1 doesn't foreclose them. | AGENTS.md §10 (deferred) |

Out of scope for this study (mirrors the spec): the consortium / central-PI gatekeeper model (AGENTS.md §2, §10) and building conflict-merge (AGENTS.md §2) — we *probe expectations* on the latter but do not validate an implementation.

---

## 2. What we ALREADY know (don't re-litigate; design around these)

These are settled by spec or transcript. Each research question below either *confirms in passing* or *builds on* one of these — but we do not re-open them in interviews.

| # | Question | Answer (settled) | Source |
|---|---|---|---|
| K1 | Does a PI need the same depth as a grad student? | **No.** PI needs the summary `Evidence` (≈ one figure / one slide) to trust & act; the grad student traverses to Study / Protocol / data and only needs raw data to *re-analyze*. One subgraph serves both: **summary up front, traversal on demand.** | AGENTS.md §3 (recipient-dependent depth); transcript Spkr A: *"as the PI… the summary plot… will be sufficient… the grad student would want to know how you did it… or do a different analysis on your data"* (lines 65–69); R4 |
| K2 | Do we ship data or references? | **Pointers, not payloads.** Reference data/code/video by git+commit+path / S3·HTTP URI / markdown link; never embed or transfer raw data. | AGENTS.md §5 R2; transcript Spkr E: *"discourse graphs are great for understanding which data generated which… but very, very bad at storing data"* (line 97); 80 GB stack on a 25 TB array (line 64) |
| K3 | When in the project should permissions exist? | **Start closed.** Permissioning designed in from the start; harder to close an open system than open a closed one. | AGENTS.md §5 R5; transcript Spkr H: *"permissioning need to be thought about at the start because if you just make the assumption that everything's open, it's much harder to close it rather than starting closed and opening up later"* (line 125) |
| K4 | One Evidence type, or split "Result" vs "Evidence"? | **One `Evidence` type** — covers own results and literature observations; never split. (Sean's earlier "shot myself in the foot" by over-splitting.) | AGENTS.md §5 R3, §4.1; transcript Spkr E: *"separating evidence and results… merge those into… an observation"* (line 40); *"shoot yourself in the foot… too granular… creates a bunch of overhead"* (lines 41–48) |
| K5 | What is a `Request`? | **Something that doesn't exist yet** — a prompt to extend the graph in a desired direction. NOT a generic comment, NOT a permission-request, NOT a data-request. | AGENTS.md §6, §10; transcript Spkr I: *"a prompt to extend the graph in a particular desired direction"* (line 387); Spkr H: *"this doesn't exist yet, but this is something that we need"* (line 381) |
| K6 | Is a web/URL view acceptable for v0? | **Yes.** Each DG node should be displayable as a web page at a URL; markdown rendered as HTML is acceptable for v0. | `_grounding.md` §8 mode 2 & §12; Roam `Initiative/Inter-graph functionality` (*"each DG node displayable as a Web page with a URL"; "markdown rendered as HTML is ok for v0"; "we need a URL"*) |
| K7 | What does Kate's lab want from reception? | **"Everything in one place, any time"** — searchable, revisitable years later; first view may need a walkthrough but without scheduling a Zoom. | AGENTS.md (implicit in §3); transcript Spkr J: *"access everything in one place and at any time… look at it three years from now"* (line 146) |
| K8 | Does a walkthrough video help comprehension? | **Yes, desired** — a short recorded walkthrough so the first read is guided, watchable any time (not a scheduled Zoom); referenced (pointer), not embedded. | AGENTS.md §7; transcript Spkr J: *"maybe they could record a video… the first time we look at a graph, somebody's walking us through it… three years from now… watch it again"* (line 148); Spkr F: *"references to the video… in the description"* (line 154) |
| K9 | Must a shared node stand alone? | **Yes** — each node interpretable on its own with a header/summary, "like the methods section of a paper"; deeper context by traversal. | AGENTS.md §5 R4; transcript Spkr E: *"node should be interpretable on its own… like going to the methods section of a paper"* (lines 167, 173) |
| K10 | Should there be a private node that is NOT shared? | **Yes** — node-level privacy; e.g. "Sean's thoughts" stays private; sharing needs node-level granularity. | AGENTS.md §5 R5; transcript Spkr E: *"Sean's personal thoughts… maybe not share that one, have some kind of granularity… John's innermost secrets"* (line 30) |
| K11 | Is the transport bound to the schema? | **No** — schema-agnostic transport (JSON-LD on the wire); changing the schema must not break sharing. | AGENTS.md §5 R6, R7; transcript Spkr E: *"the solution should probably be somewhat abstract to the actual schema… shouldn't wreck the sharing protocol"* (line 225) |
| K12 | Central gatekeeper or peer-to-peer? | **P2P** — each lab mediates its own bilateral relationship; the consortium/central-PI model is realistic at scale but **out of scope** for v1. | AGENTS.md §2, §5 R1, §10; transcript Spkr H: *"allow each organization to mediate their individual relationship with others, rather than… a centralized approach"* (line 134); Farida's consortium model (line 128) |

> **Design implication:** sessions should *start* from these as givens (e.g. show the summary-up-front Evidence in mockup 02 as already decided) and spend their minutes on §3's open questions.

---

## 3. What we DON'T know — questions for users

Grouped by theme. Each row: the open question · the method · the artifact it probes. Methods: **moderated concept test** (talk-aloud on a mockup), **interview**, **card sort**, **diary study**, **survey**. Ties to AGENTS.md §10 and the Roam "considerations" (send-closure, dangling-reference signal-vs-hide, local-disk pointers, access-request, versioning, dedup, levels-of-shared-context binning) are called out.

### Theme A — Sharing model & permissions (D1, D2, D3, D7)

| Open question | Method | Probes |
|---|---|---|
| **Send closure:** when Sean shares the `Evidence`, what does he *expect* to travel with it? Just Evidence+Study+Protocol (R4 default), or more? Does the default match his intent? (AGENTS.md §10 send-closure) | Concept test (talk-aloud) | [`./mockups/01-share-subgraph.html`](./mockups/01-share-subgraph.html) |
| **Pulling a not-included node:** a recipient wants a Protocol two hops away that wasn't in the bundle. How do they expect to get it — auto-pull, request access, ask Sean? (AGENTS.md §10; Roam "easily request access") | Concept test + interview | `01`, `03` (the import side) |
| **Permission granularity:** is node-level + subgraph-level + per-project default the right set of controls, or too many / too few? Can Sean set them without confusion? (AGENTS.md §10 permission granularity) | Concept test (talk-aloud) | `01` (permission states on the share dialog) |
| **Dangling reference — signal vs hide:** shown a shared relation that points at an un-shared node, do users *want* to see "1 related node not shared" (signal, Matt) or see nothing (hide, MAP)? Which feels safe? Which feels honest? (AGENTS.md §10; Roam "considerations") | Concept test, A/B two variants of the same screen | `01` and `03` (render the un-shared "Sean's thoughts" both as a redacted line and as fully hidden; compare) |
| **Levels-of-shared-context binning:** do the five Roam levels (established collaborator / seeding a graph / same person two graphs / prospective collaborator / everyone in the world) match users' mental model, and do they collapse cleanly to the 3 UX modes? (Roam `[[ISS]]` levels) | **Card sort** (see §4) | The five level cards vs. the 3 modes (`03`/`04`/`05`) |
| **Per-project default share:** Kate's "umbrella" project with several collaborators who share asymmetrically (some share back, some don't, some private) — does a per-project default + per-collaborator override express this? (transcript Spkr J, line 116) | Interview + concept test | `01` (project-default control) |

### Theme B — Reception modes, incl. the web view (D1, D4)

| Open question | Method | Probes |
|---|---|---|
| **Import comprehension:** after import, can the grad student traverse Evidence → Study → Protocol → pointer and find the segmentation threshold? Is "summary up front, traversal on demand" legible? (K1, R4) | Concept test (task) | [`./mockups/03-kate-import.html`](./mockups/03-kate-import.html), [`./mockups/02-evidence-node.html`](./mockups/02-evidence-node.html) |
| **Web view — cold comprehension (THE added scenario):** a recipient with **no DG app** opens the URL. Can they understand the result cold, see it's unpublished, follow the git/S3/video pointers, and grasp the subgraph without traversal training? (D4; `_grounding.md` §8 mode 2) | Concept test with the **no-DG-app collaborator** | [`./mockups/04-web-view.html`](./mockups/04-web-view.html) |
| **Web view — trust:** does a one-figure summary on a web page earn enough trust for a PI to act, the way it would in a slide? What's missing (the "what's in the dish" context table)? (K7, K9; transcript Spkr D line 159) | Concept test + trust rating | `04`, `02` |
| **Web view — conversion CTAs:** are "request access" and "import later if you adopt a DG tool" understood and compelling? Does the web view feel like a dead end or an on-ramp? | Concept test (talk-aloud, observe CTA use) | `04` |
| **Make-edge after import:** does Kate's lab understand they can draw an edge from *their own* node to Sean's imported node, and that updates will notify them? (AGENTS.md §9 make-edge; Roam "be notified when an imported node updates") | Concept test (task) | `03` |
| **Notification expectations:** what update events do recipients want notified (new Evidence on a claimed Request, a revised figure, a deprecated node)? (Roam recipient stories) | Interview | `03`, `05` |
| **Walkthrough video placement:** embedded vs. pointer; auto-play vs. on-demand; does it reduce the "first read needs a human" gap (K8)? | Concept test | `02`/`04` (video chip) |

### Theme C — Request lifecycle (D5)

| Open question | Method | Probes |
|---|---|---|
| **Request semantics:** shown a `Request`, do users read it as "doesn't exist yet" rather than a comment, a data-request, or a permission-request? (K5; AGENTS.md §6, §10 "other readings") | Concept test (talk-aloud) | [`./mockups/05-request-flow.html`](./mockups/05-request-flow.html) |
| **Create → claim:** is "skill required" + `request_target` → Claim enough for a collaborator to decide to **claim** it, and is "claim" understood as committing to do the work? (AGENTS.md §6) | Concept test (task) | `05` |
| **Fulfil → notify:** when the claim produces a new Study → Evidence pointing back at the Request, is the loop back to the requester clear? (AGENTS.md §6 step 4–5) | Concept test (talk-aloud) | `05`, `03` |
| **Confounded readings (probe, don't build):** do users *expect* the same node to also carry permission-requests ("grant my postdoc access") or data-requests? (transcript Farida line 369; Spkr J line 370; AGENTS.md §10) | Interview | `05` |
| **Discovery of as-yet-unidentified collaborators:** how should an open Request surface to a lab that *might* be able to claim it? (AGENTS.md §6 step 2) | Interview | `05` |

### Theme D — Authoring friction & adoption (D6)

| Open question | Method | Probes |
|---|---|---|
| **Share-time prompts:** do "add a summary / record a walkthrough / format-in-schema" prompts feel helpful or like overhead at the moment of sharing? (AGENTS.md §7; K8) | Concept test (talk-aloud, friction notes) | `01` (the share flow) |
| **Click-overhead / adoption fatigue:** Matt's lab adopters "petered off" because tagging is extra work — what is the friction budget before a producer abandons it? (transcript Spkr J line 178; Spkr E line 192) | Diary study (§4) + interview | Real sharing events |
| **Intent-preserving capture:** would an LLM-asks-you-questions flow (R10) feel like it *keeps* the scientist's intent, or like automation that "loses intent"? (transcript Spkr A line 188; Spkr C line 190; Spkr I line 194) | Interview + lightweight prototype walk-through | (authoring track — not a numbered mockup) |
| **5-minutes-on-my-laptop:** does the share flow preserve the "open my laptop, 5 minutes, it's made" feel that makes the tool usable at all? (transcript Spkr E line 50) | Concept test (time-on-task) | `01` |

### Theme E — Cross-tool & conflict-merge future (D8 — surface, don't solve)

| Open question | Method | Probes |
|---|---|---|
| **Local-disk pointer resolution:** when a pointer targets data only on Sean's lab array (not S3/GitHub), what does a recipient *expect* to happen — request, broken link, "ask Sean"? (AGENTS.md §10 local-disk; transcript line 64) | Interview | `02`/`04` (LOCAL pointer chip) |
| **Versioning:** when referring to an external resource, do users expect the version they referred to to be pinned (git commit, S3 version)? (AGENTS.md §10 versioning; transcript line 100, 345 "we've frozen it") | Interview | `02` (pointer with commit) |
| **Dedup / merge across graphs:** if two labs evolve the same Claim, what do users expect — deprecate-both-mint-merged, union edges, `owl:sameAs`? (AGENTS.md §10; transcript lines 274–279) | Interview (storyboard, not a built flow) | Storyboard derived from `03` |
| **Cross-tool fidelity:** Obsidian ↔ Roam ↔ MyST via the JSON-LD bridge — does Anton's MyST/S3/GenBank world round-trip without surprises? (AGENTS.md §10 Kate's tool; transcript Spkr D lines 114, 347) | Concept test with **Anton** | `03`, `04` (rendered from a MyST source) |

---

## 4. Study design

### Recruiting (n ≈ 7 + extras; map directly to the personas in `_grounding.md` §2)

| Code | Who | Tool | Role in study | Why essential |
|---|---|---|---|---|
| P-PROD | **Sean** (Vogel lab grad/postdoc) | Obsidian + DG plugin | The producer; runs the share/permission/request flows. | Owns the canonical case (AGENTS.md §9); tests D1, D2, D6. |
| P-PI | **Kate** (PI of the collaborating lab) | Obsidian or Roam (TBD) | The summary-level consumer. | Tests K1/K7 (summary-up-front, trust), D4 web-view trust. |
| P-GRAD | **A grad student in Kate's lab** | Obsidian/Roam | The traversal-level consumer. | Tests K1 traversal, find-the-segmentation-threshold, make-edge (D1, D4). |
| P-SYNBIO | **Anton** (synthetic-biology, MyST) | MyST · S3 · GenBank | Cross-domain / cross-tool collaborator. | Tests Theme E cross-tool fidelity, local-vs-S3 pointers, SBOL/PROV-O boundary. |
| P-NOAPP | **A collaborator with NO DG app** (clinician, reviewer, or prospective collaborator — e.g. Kate *before* she installs anything) | A web browser only | The web-view cold-read tester. | **Crucial** — the only valid test of mockup `04` (D4). Recruit ≥1, ideally 2 (one domain-near, one domain-far). |
| P-PI2 / P-GRAD2 | A second PI + grad pair from a *different* collaborating lab (stretch) | Obsidian/Roam | Replication of the PI/grad split. | Guards against over-fitting to Kate's lab; supports the card sort. |

Screen for: active discourse-graph or lab-notebook use (or willingness), real ongoing inter-lab collaboration, and — for P-NOAPP — genuinely *no* DG tooling. Consent must cover unpublished data (see §7).

### Plan

1. **~5–7 moderated concept-test sessions (60–75 min each), remote or in-person, talk-aloud** using mockups `01`–`05`. Per session: 2–3 min orientation (the story, the unpublished-data caveat), the per-mockup tasks (§5), then 10–15 min of open interview on the relevant §3 open questions. Counterbalance mockup order across participants except always show `02` (Evidence node) before `03`/`04` so the node is understood before reception. Route the **dangling-reference signal-vs-hide** A/B (D3) so each participant sees one variant first.
2. **Card sort (15–20 min, inside or adjacent to the session) on "levels of shared context"** (D7). Cards = the five Roam levels: *established collaborator · seeding another graph from a trusted collaborator · literally the same person in two graphs · prospective collaborator · everyone in the world*. Task 1 (open sort): group by "how much do you share / how much do you trust." Task 2 (closed sort): map each level onto the three UX modes (`03` import / `04` web / `05` request) and flag any that don't fit. Reveals whether the 5→3 collapse holds and where a fourth mode is wanted.
3. **Lightweight diary study (2–3 weeks, P-PROD + P-GRAD + ≥1 other producer) of real sharing events.** For each real "I shared / received a result or request" moment, a 3-field micro-entry: *what I shared, with whom, what I had to decide/click, what felt like friction*. Captures the §3 Theme D adoption/friction signals (D6) and real send-closure choices (D1) in the wild, where the lab told Matt adopters "peter off" (transcript line 178). Pair with a 20-min exit interview.
4. **Optional short survey (n = 10–20, the broader DG/MIRA user pool)** to size the friction budget (D6) and the relative demand for reception modes (D4/D7) beyond the deep sessions — quant backstop, not a substitute for the talk-alouds.

---

## 5. Per-mockup usability tasks

Run as talk-aloud; record time-on-task and friction points. Concrete content from `_grounding.md` §3 (G3BP1/PABP1 stress-granule example).

**Mockup [`01` — share / subgraph-selection dialog](./mockups/01-share-subgraph.html)** (producer flow; tests D1, D2, D3, D6)
- "Using mockup 01, **share this result with Kate's lab but keep your private note ('is the PABP1 lag real or a photobleaching artifact?') private** — talk aloud as you go." *(R5 node-level privacy, K10.)*
- "Show me everything you think will travel to Kate when you hit Share." *(send closure, D1.)*
- "Set this so Kate's whole lab can see it without you managing each student." *(per-project default, transcript line 24.)*
- "Here's a relation pointing at a node you didn't share — what should Kate see here?" *(dangling-reference signal-vs-hide, D3.)*
- Note every prompt (summary / record walkthrough / format-in-schema) and whether it felt helpful or like overhead. *(D6.)*

**Mockup [`02` — Evidence node](./mockups/02-evidence-node.html)** (the shareable unit; tests K1, K9, R2)
- "Read this node cold and tell me, in one sentence, what was found." *(K9 stand-alone interpretability.)*
- "You're the PI — is this enough to trust and act on? What's missing?" *(K1, K7.)*
- "Find where you'd get the code that made this figure, and the raw data." *(R2 pointers — git/S3/LOCAL/video chips; confirm no raw-data download.)*
- "Where would you check what cell line / dose / batch was in the dish?" *(the "what's in the dish" context table, transcript line 159.)*

**Mockup [`03` — import into a DG app](./mockups/03-kate-import.html)** (high-context reception; tests D1, K1, D4-adjacent, make-edge)
- "Import this into your graph, then **traverse from the result to find the segmentation threshold Sean used.**" *(K1 traversal, the transcript's literal example, line 28.)*
- "Draw a relation from one of *your* nodes to Sean's imported Evidence." *(make-edge, AGENTS.md §9.)*
- "A Protocol you need wasn't included. Get it." *(pull a not-included node, D1.)*
- "Six months from now Sean revises the figure — how would you find out?" *(notifications, Roam recipient stories.)*

**Mockup [`04` — view on the web](./mockups/04-web-view.html)** (the added no-DG scenario; tests D4 — run with **P-NOAPP**)
- "You have no special software — open this link and tell me what Sean found, and whether you believe it." *(cold comprehension + trust, D4.)*
- "Is this published? How do you know?" *(unpublished labelling.)*
- "Follow this result back to the data and the walkthrough video." *(pointers + video on the web, K8.)*
- "You'd like deeper access / to pull this into your own tool later — what do you do?" *(request-access & import-later CTAs.)*

**Mockup [`05` — Request lifecycle](./mockups/05-request-flow.html)** (collaboration primitive; tests D5)
- "Kate needs an experiment that doesn't exist yet — **create a Request for the PABP1 phospho-null mutant assay** and point it at the right Claim." *(create; `_grounding.md` §3 request example.)*
- "You're in Sean's lab and you have live-cell LLSM skills — what do you do with this Request?" *(claim; skill-required match.)*
- "In your own words, what *is* this thing? Is it a comment, a data-request, or something else?" *(K5 semantics; AGENTS.md §10 other readings.)*
- "The work is done — show me how the result gets back to Kate." *(fulfil → notify loop.)*

---

## 6. Signals & metrics

Adapted from MAP's evaluation ideas surfaced at the MIRA workshop (serendipity, cross-referencing, feedback-seeking on preliminary ideas, adoption of inter-graph features), plus web-view comprehension/trust and producer-flow friction.

**Adoption & collaboration (longitudinal — diary + follow-up; MAP-style):**
- **Serendipitous discovery** — count of "I found a useful result/Request I wasn't looking for" events per participant per week (diary-logged).
- **Cross-referencing of others' results** — number of make-edges drawn from a lab's own nodes to imported (other-lab) nodes; trend over the diary window.
- **Feedback-seeking on preliminary ideas** — count of preliminary/unpublished results shared *before* publication, and Requests issued on still-open Claims (proxy for willingness to expose early work).
- **Adoption of inter-graph features** — % of sessions where import / make-edge / Request / web-share are used unprompted; week-over-week retention vs. the "peters off" baseline (transcript line 178).

**Web-view comprehension & trust (mockup 04, P-NOAPP):**
- **Cold-comprehension accuracy** — can the participant state the finding correctly with no traversal training (pass/fail + paraphrase quality).
- **Trust rating** — 1–7 "would you act on this from the web page alone?" + qualitative reasons; gap vs. the same question after seeing the in-app node.
- **Pointer-follow success** — % who locate code / data / video pointers unaided.
- **Conversion intent** — % who use (or say they'd use) request-access / import-later CTAs; whether the web view reads as on-ramp vs. dead end.
- **Unpublished-status recognition** — % who correctly identify the data as unpublished.

**Producer flow — time-to-share & friction (mockups 01/02; concept test + diary):**
- **Time-to-share** — seconds from "I want to share this" to a correctly-scoped Share, with the private node kept private. (Anchor to the "5-minutes-on-my-laptop" expectation, transcript line 50.)
- **Privacy-correctness** — % of share tasks where the un-shared node did *not* leak (existence or content) — a direct R5 / D3 check.
- **Friction count** — number of clicks / decisions / hesitations per share; self-rated overhead of each share-time prompt (helpful ↔ annoying).
- **Send-closure match** — % of cases where what actually travelled matched what the producer expected (D1).

**Request legibility (mockup 05):**
- **Semantic-match rate** — % who describe a Request as "doesn't exist yet" (not comment / data-request / permission-request) — direct K5 / D5 check.
- **Claim-decidability** — % who can decide whether to claim from "skill required" + target alone.

---

## 7. Risks & ethics

- **Unpublished data.** The canonical case is real, unpublished stress-granule work (AGENTS.md §9, §11; transcript line 394: *"maybe we can't publish it outside of here because it's unpublished data"*). Mitigations: explicit consent from Sean/Vogel lab before any session; every figure labelled "illustrative · unpublished — workshop prototype"; **no raw data** ever in a session (R2 makes this natural — only pointers); recordings stored access-controlled and deleted after analysis; the verbatim transcript stays local and uncommitted (AGENTS.md §0, §11).
- **Permission leakage in testing the very feature about permissions.** The dangling-reference A/B (D3) and the share tasks could themselves expose an un-shared node ("Sean's thoughts") to an observer. Mitigation: use only the fabricated private-note text from `_grounding.md` §3, never Sean's real private notes; treat any observed leak in the mockup as a finding, not a breach; debrief P-NOAPP that web-view content is a prototype, not a real shared result.
- **Adoption fatigue / click-overhead.** The study must not *cause* the fatigue it measures: keep diary entries to 3 fields, sessions ≤75 min, and avoid leading questions that imply more tagging is virtuous. Watch for social-desirability bias — participants over-praising a tool a respected PI champions.
- **Cross-lab confidentiality & consent asymmetry.** Recruiting a real grad student from Kate's lab plus Kate herself can create power dynamics (a student may not freely criticize); interview them separately and anonymize. Anton's GenBank/synbio constructs may have their own sharing constraints — confirm before using any real artifact.
- **No-DG-app recipient expectations.** P-NOAPP may assume the web page grants more access than it does (D4 conversion); be careful not to over-promise import/access during the session, and clarify it is a prototype.
- **Generalizability.** A single collaboration (Vogel ↔ Kate) risks over-fitting; the second PI/grad pair and the survey (§4) are the guardrails. Findings are formative for v1, not a summative validation of the full consortium-scale story (which is out of scope per AGENTS.md §2, §10).

---

*MIRA × Discourse Graphs · inter-lab exchange · user-research plan · draft v0.1*
