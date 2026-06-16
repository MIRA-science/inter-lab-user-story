# Dashboard mockups v0.4 — real data + UI refinements (2026-06-15)

**Status:** spec delta. Supersedes the *placeholder science* used through v0.3.
**Scope:** screens 06–11 + the index. No changes to `tokens.css` / `components.css`
(all v0.4 CSS is screen-local).
**Companion docs:** [v0.3 trainee-feedback spec](ux-spec-update-2026-06-11-trainee-feedback.md),
[trainee research synthesis](ux-research-synthesis-2026-06-11-trainee.md). The *rationale* for
"no nudge / meeting-anchored" lives in those docs and the v0.3 synthesis — **not** in the
rendered mockups (see §2.4).

---

## 0 · What changed v0.3 → v0.4

Two things, on the same six screens:

1. **Real data.** Every screen was rebuilt on the lab's real, prepared discourse-graph
   nodes (the `discourse-graph/` folder), replacing the invented G3BP1/PABP1
   *recruitment-order* placeholder. The spine is now the lab's actual finding:
   **stress-granule composition changes over time.**
2. **Four UI refinements** from a walkthrough of the v0.3 mockups (06 share targets,
   07 subgraph + request panel, 09 grab handles, 11 de-justification).

---

## 1 · The node model (from real data, lightly extended)

Source nodes are the five tracked files in `discourse-graph/` (already public, committed
in `ad8cd46`). Node-type **morph** applied for the discourse-graph schema:
`Experiment → Study`, `Result → Evidence` (this matches the JSON-LD: the `Experiment`
schema is `subClassOf mira:Study`, `Result` is `subClassOf mira:Evidence`).

| Role | Node | Source | Notes |
|---|---|---|---|
| **Study** (was EXP) | "Live Lattice-SIM imaging of granule formation at staged times" | `019ea6d6-d619` | Protein 1–GFP + Protein 2–mKate in HeLa, ZEISS Elyra 7 |
| **Evidence** (was RES) | "Stress granules change composition over time" | `019ea6d6-d61a` | Pearson r(P1,P2): **negative → positive over ~10–15 min** |
| **Protocol** ×3 | HeLa culture · Lattice-SIM imaging · **Segmentation & composition** | `…3a61 / …a227 / …1361` | Study `follows` each; Evidence `is_grounded_in` Study |

**Generated for illustration** (clearly marked *illustrative / manufactured / preliminary*
in-screen, per the existing honesty pill):

- **Q1** (anchor Question): *"Do the two granule proteins condense together, or separately and then mix?"*
- **Q2** (follow-up Question): *"Are early granules two merging droplets, or one droplet with sub-compartments?"* — taken directly from the real Result's open second key-point.
- **C1** (Claim): *"Components first condense separately, then mix as the granule matures."*
- **S2** (Study): *"Architecture test of early granules (FRAP)"* — addresses Q2; the "on the agenda / planned" follow-up.
- **E2** (Evidence, preliminary): *"Early granules look like two adjacent condensates."*
- **R1** (Request, Kate → Sean): *"Push the time-course below 5 min"* — the early arsenite-vs-osmotic divergence.

**The load-bearing methods detail** (the payoff a reproducer traverses to, screen 08) is no
longer a single "threshold" — it is the **segmentation choice**: segment on the **per-pixel
SUM of both channels** (not one), thresholded just above background. That choice defines
every granule, and so every co-localization number.

**Anonymization preserved.** The real data redacts the two proteins to `PROTEIN 1`/`PROTEIN 2`;
the mockups keep that redaction (shown as *Protein 1–GFP* / *Protein 2–mKate*). No real protein
names were introduced. Methods (Lattice SIM, sodium arsenite, 3D Suite, regionprops) and the
inter-lab provenance (cells via the Montreal exchange; Khalid maintains the culture) are real
and were already public.

### The result plot
The reused illustrative figure changed from *two intensity-vs-time curves* to **Pearson r vs
time**: a zero reference line, two stress conditions (arsenite, osmotic) that both climb from
negative to positive, **diverging in the first ~10–15 min then converging**. Colour-blind-safe
(coral/blue) with inline labels + legend. Rendered at four sizes across 06/07/08/10.

---

## 2 · Per-screen UI refinements

### 2.1 · 06 — "Share with" is now multi-select (checkboxes)
The right-click *Share with* menu was single-pick rows; it is now **checkbox rows** so a
producer can share with **more than one group at once** (e.g. *Kate's Lab* + *McGill
Consortium*). A live "Share with **N** selected…" button reflects the count. People list
gains an inter-lab face (*Khalid · Montreal*). Atoms: screen-local `.ctx-check` / `.ctx-share-btn`
reusing the shared `.cbx`.

### 2.2 · 07 — Question → subgraph highlight, and a request composer
- **Hover (or focus) a Question card → its subgraph lights up, the rest dims.** Every kanban
  card carries `data-sub`; questions carry `data-q`. JS toggles `.subhit` / `.subdim`; a
  per-question "N linked" count. Keyboard-accessible (focus/blur), reduced-motion-safe.
- **A "make a request" panel now sits *beneath* the kanban** (the empty space the walkthrough
  flagged): a typed-`Request` composer — `request_for` [Study / Protocol detail / Raw data],
  a target chip, a message, and Send → an on-node confirmation (no email). Screen-local
  `.reqpanel` / `.rq-*`.

### 2.3 · 09 — Grab handles on every card
The drag-to-reorder grip was on 2 of ~7 cards; it is now on **every** kanban/candidate card,
and made **persistently visible** (≈0.4 opacity, brightening on hover) so the reorder
affordance reads on each card, not just on hover. (Shared `.kcard__grip`; one screen-local
opacity override.)

### 2.4 · 11 — The ping/nudge *justification* is removed from the surface
Per the note that "these explanations can remain in the UX document but shouldn't be in the
mockup itself," screen 11 no longer argues *why there is no nudge*:
- Header eyebrow/h2/lead are now plain descriptions ("…what's slated for discussion"); the
  *"I'd really hate to receive [a ping]"* quote and "no ping ever arrives" framing are gone.
- The **"Why there's no nudge" side rail is replaced** by a functional rail: *This sync*
  intro · *Change key* legend · *Followers* (opt-in, owner-granted) · *Accessibility*.
- Footer drops "0 pings sent · ever" / "not a push."

The rationale itself is unchanged and still authoritative — it lives in the v0.3
trainee-feedback spec and synthesis. The product **behaviour** (meeting-anchored agenda,
self-set deadlines, opt-in Follow, staleness folded not pushed) is unchanged; only the
in-mockup *editorializing* was removed.

---

## 3 · Acceptance criteria (v0.4)

1. No screen references the old recruitment-order placeholder (G3BP1/PABP1/"assembly order"); the spine is composition-over-time. ✅ (grep-clean)
2. `Experiment→Study`, `Result→Evidence` morph reflected in every node badge. ✅
3. Protein redaction preserved (`Protein 1`/`Protein 2`); no real protein names. ✅
4. 06: ≥2 share targets selectable at once; count updates live. ✅
5. 07: hovering/focusing a Question dims non-subgraph cards; a request composer sits below the kanban. ✅
6. 09: every card has a visible grab handle. ✅
7. 11: no ping/nudge justification in rendered text (header, rail, or footer). ✅
8. Previews 06–11 + index regenerated at 2× from the live render. ✅
9. `tokens.css` / `components.css` unchanged; no regressions to 01–05. ✅

---

## 4 · Round-2 refinements (same day, after a walkthrough of v0.4)

A second pass on the live v0.4 mockups. The 08 work follows
[impeccable.style](https://impeccable.style/docs/) — its **distill** ("collapse
multiple card styles into one; every element justifies its existence") and **quieter**
("≤2 intentional colours; use *one* of border/background/shadow; remove auto-play")
guidance.

**07 — the dashboard got its interactions:**
- **Subgraph highlight now fires on *any* node type**, not just Questions. Hover/focus
  any card → the cards sharing its `data-sub` stay lit, the rest dim. (Model changed to
  "dim-all-then-light-the-hits.")
- **Table view removed** — it was empty. The view switch is now Graph · Kanban.
- **Click a card → the right-rail inspector swaps to that node.** Seven per-node panels
  (e1/e2/q1/q2/s1/s2/cand), each reusing the `.ecard` atom — no new card style (distill).
  Default selection is the composition Evidence.
- **The audience presets now do something.** Results / Experiments in progress /
  Questions & requests filter the board (dim non-matching cards), relabel the board head,
  and spotlight the request composer on "Questions & requests."

**08 — intentional card usage (impeccable pass):** the **discourse node is the single
primary card**. Each remaining colour now *means* something rather than decorating:
the protocol-wash marks the one segmentation **payoff**; a request-ink **left accent**
marks the request-back card (same "left border = node type" language as the chain nodes).
The explanatory `.why` aside and the gated-stacks note were **quieted to plain notes**
(no wash), the framing prompt lost its border and its auto-animating arrow, and the payoff
node's double shadow became a single ring. Net: from five competing coloured boxes to a
clear hierarchy.

**10 — removed the card-anatomy strip.** The region-map callout was design documentation;
it belongs in this spec, not in the rendered mockup. (Same principle as the 11 v0.4 change.)

**11 — the diff rows are clickable.** Each row opens its node (→ `08`, the node page),
with a chevron affordance + hover state; rows that already own a control (the "set next
step" backlog row) are left alone.

### Round-2 acceptance
10. Hovering a **Study** (not just a Question) lights its subgraph. ✅
11. Clicking a kanban card swaps the inspector to that node. ✅
12. Presets relabel the board and dim non-matching cards; no Table tab. ✅
13. 08 renders with the node as the only card pattern; `.why` / access note are plain; ≤2 competing washes. ✅
14. 10 shows no anatomy strip. ✅
15. 11 rows navigate (keyboard + click); the control-bearing row is exempt. ✅

---

## 5 · Not in scope (carried forward)
Real top-to-bottom Graph view (filter-by-creator, node→subgraph drill-in); native mobile/iOS
layouts; a standalone dyslexia mockup; a `personas.md`. Unchanged from the v0.3 backlog.
