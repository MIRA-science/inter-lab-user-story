# Dashboard mockups — glance-and-trust correction + verbatim grounding (2026-06-15)

> 🗂️ **Historical delta — folded into the current spec.** The single, up-to-date specification is
> **[`ux-spec.md`](./ux-spec.md)**; build and maintain the mockups from it. This correction is kept only as
> the record of how it was reached. Its conclusions (no "Endorse"; study/protocol expander; verbatim-on-
> demand) are **current** and live in [`ux-spec.md §5.1, §7.1`](./ux-spec.md#5-ux-principles-normative-for-design). *(Consolidated 2026-06-15.)*

**Status:** spec delta. Companion to the [v0.4 real-data + UI doc](ux-spec-update-2026-06-15-real-data-and-ui.md); both dated Jun 15.
**Scope:** screen 07 (the inspector's PI region) and screen 08 (the study/protocol detail a reader clicks into), plus a correction to [`_grounding-dashboard.md`](_grounding-dashboard.md) §3.
**Origin:** author's clarification of the Jun-8 "glance-and-trust" quote (see §4) — the previous reading produced an **Endorse** affordance that does not belong.

---

## 1 · The correction — "glance-and-trust" is passive, not an action

The Jun-8 source quote (Sean, speaking as the PI):

> "As the PI, often that will be sufficient. I'll be like, okay, I see you have this new information, like I trust you. And then the grad student would want to know how you did it."

Through v0.1–v0.4 this was read as a PI *action*: an **Endorse** button on the Evidence card
(`07-shared-dashboard.html`, the `.trust` block — a click that flipped the label to "✓ Endorsed").
**That is a misreading.** The author's intent:

- The PI **trusts that the experiment was done well** and **does not need to see the study's details** —
  *unless the result looks unexpected*, in which case they (or their grad student) drill in.
- There is therefore **no action for the PI to take** on a shared result. No endorse, no approve, no sign-off.
- The **only affordance this region needs** is the **option to open the grounding study/protocol** — ideally an
  in-place **collapse/open** in the same sidebar, so the reader goes summary → detail without losing context.

**Decision:** remove the Endorse affordance entirely; replace it with an easy, prominent **expand/collapse of the
study details** in the inspector. (This also matches the v0.4 §2.4 principle — in-mockup editorializing/quotes
belong in the spec, not the rendered screen.)

## 2 · Screen 07 — the inspector's PI region

**Remove** from the default Evidence inspector panel (`data-node="e1"`):
- the `.trust` block — the quote, the **Endorse** `<button>`, and the now-unused `.trust` CSS.
  (The "For the grad student → how was it done?" link folds into the study-details expander and the existing
  "Open full study details →" footer link.)

**Promote** the existing `<details class="ecard__method">` from a low-key "Method & context" to the **primary
affordance of this region**: a clearly-labelled, obviously-expandable **"How it was done — study & protocol"**
collapse/open, sitting where the endorse block was, with a direct path to the verbatim grounding
(**"Read the full study & protocol →"** → screen 08).

Net: the PI glances and is done; opening the study is one obvious, in-place click — and no action is implied
that isn't real.

## 3 · Verbatim study & protocol content must be reachable (07 → 08, and any node detail)

**Principle:** the rendered cards are *summaries*. The **human-authored node text is the ground truth that every
summary derives from**, and it must be available when a reader clicks into a Study or Protocol — not paraphrased
away. Today screen 08 shows only derived summaries (a 2-sentence study blurb; a 4-row "segmentation choice"
table). The verbatim content already exists in `discourse-graph/*.md` and must be surfaced, summary-first /
**verbatim-on-demand** (a per-node collapsible, opened by a plain human label — "Sean's notes" / "Experiment notes" / "The full protocol" — not a cryptic "Read verbatim"):

| Node | Verbatim source (`discourse-graph/`) | What must be reachable |
|---|---|---|
| Evidence / Result | `RES - Stress Granules Change Composition Over Time.md` | the three **Key Points** — negative→positive correlation; the two-droplets-vs-one-sub-compartmented-droplet ambiguity; the significant early difference between the two stresses |
| Study / Experiment | `EXP - SIM Imaging Stress Granules at Different Times during Formation.md` | the dated **Progress & Notes** (2025-08-23 Montreal cells / Khalid maintaining culture; 2025-10-14 pipeline + the two segmentation design choices) and the two **Hypotheses** |
| Protocol ×3 | `PRO-High resolution stress granule formation imaging (Lattice SIM).md`, `PRO-Stress Granule Segmentation and Composition Mesasurement.md`, `PRO-Hela Cell Culture.md` | the **numbered steps** and the **Imaging / Processing parameter tables**; the culture protocol's honest "TODO: get growth info from Khalid" stub |

Anonymization preserved: the sources already redact to `PROTEIN 1` / `PROTEIN 2`; keep that, introduce no real
protein names. Inter-lab provenance (cells via the Montreal exchange; Khalid maintains the culture) is real and
already public.

## 4 · Acceptance criteria

1. No "Endorse"/approve affordance on any Evidence card or node (07 or elsewhere) — grep-clean for `Endorse`.
2. 07: the study-details expander is present, prominent, opens/closes in the sidebar, and links to the verbatim detail.
3. 08: each of Evidence, Study, Protocol exposes its **verbatim** node text on demand — the segmentation
   reasoning, the dated lab-notebook notes, the two hypotheses, and the imaging/processing parameter tables all
   appear verbatim.
4. Protein redaction preserved; no real protein names introduced.
5. Summary-first glance path intact (verbatim is opt-in, not forced open).

## 5 · Provenance

- **"Collaboration between labs experiment"** — Jun 8, 2026 (Granola). Source of the glance-and-trust quote: the
  PI trusts the summary; the grad student wants the how. (`MIRA-transcripts/lab-to-lab/2026-06-08-collaboration-between-labs.transcript.md`, line 83.)
- **Author clarification** — Jun 15, 2026: glance-and-trust is passive; the endorse button was a misreading; the
  region needs an open-the-study affordance, and verbatim study/protocol content must ground the summaries.
