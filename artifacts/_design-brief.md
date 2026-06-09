# Design brief — MIRA "publish to a public repository or database" mockups

> Read with [`_grounding.md`](./_grounding.md) and [`../AGENTS.md`](../AGENTS.md). Every mockup is a
> **single self-contained `.html` file** in `artifacts/mockups/` that links the two shared
> stylesheets and adds only screen-specific CSS. This repo **reuses the inter-lab design system
> verbatim** (`tokens.css` + `components.css`, copied from `../lab-to-lab/`), plus a
> small extension block at the bottom of `components.css` for this repo's casts and destinations (two
> blocks: QBI, then the Sean → Kate dashboard). Built per Claude's **frontend-design** sensibility:
> distinctive, production-grade, never generic.

> **This repo now carries two worked examples on one design system.** The **central** one is the
> **Sean → Kate dashboard** — pushing a selected subgraph to a *read-only shared web interface*
> (screens **06–09**, grounded in [`_grounding-dashboard.md`](./_grounding-dashboard.md) /
> [`ux-user-story-dashboard.md`](./ux-user-story-dashboard.md)). The retained one is **QBI MagLOV2 →
> the world** — the fully-public extreme (screens **01–05**, [`_grounding.md`](./_grounding.md)). Both
> ride the same machinery and the same R1–R12 rules; build all nine as **one product family.**

## Non-negotiables

1. **Link the shared system, don't reinvent it:**
   ```html
   <link rel="stylesheet" href="tokens.css">
   <link rel="stylesheet" href="components.css">
   ```
   Reuse `.node`, `.ntype`, `.btn`, `.pointer`, `.edge`, `.edge-rail`, `.panel`, `.toast`, `.avatar`,
   `.steps`, `.share-level`, `.screen`, `.winbar`, `.frame-caption`, `.reveal .d1…d8`, etc. Add a
   `<style>` block only for layout/composition unique to the screen. Do **not** import other fonts or
   restate the palette.
2. **Node-type colours are fixed** (Question gold · Claim green · Evidence coral · Study blue ·
   Protocol violet · Request indigo · Source teal) — apply via component classes.
3. **This repo's extension classes** (already in `components.css`, use them; don't redefine):
   - Avatars: `.avatar.brian` (B), `.avatar.morgan` (M), `.avatar.qbi` (lab), `.avatar.world` (a
     stranger reader).
   - Pointer kinds: `.pointer .kind.data` (curated dataset / CSV), `.kind.doi` (citable handle),
     `.kind.koi` (KOI RID / desci record) — in addition to the existing `git`, `s3`, `local`, `video`.
   - Share levels: `.share-level.world` (public to everyone) and `.share-level.gated` (public page,
     restricted resource). Existing `.private` / `.lab` / `.public` still apply.
   - `.cite` (a "cite this · version frozen" chip, with `.frozen`) and `.reuse` (a "cited by N" signal).
   - **Dashboard block (Sean → Kate):** avatars `.avatar.sean` (S) · `.avatar.kate` (K) ·
     `.avatar.grad` (Kate's student) · `.avatar.vogel`/`.avatar.lab` (labs) — *already defined in the
     base kit, reuse them.* Plus `.share-level.consortium` (password-protected, 20+ labs),
     `.node.candidate` + `.ntype.candidate` (the provisional/informal node), `.request` (the
     request-back chip, with `.open`), `.viewswitch` (Graph·Kanban·Table segmented control), `.preset`
     (audience-preset chip, with `.active`), and `.dash-search` (the "everything in one place /
     searchable" bar). Kanban/Table/Graph *layouts* are screen-specific — put them in the screen's
     `<style>`; use these atoms for the shared pieces.
4. **Use the worked example verbatim** from `_grounding.md` §2.2 — the **QBI MagLOV2 magnetic-field
   effect**. Label any plot **"illustrative · unpublished — workshop prototype."** Never invent
   contradicting science. The canonical copy:
   - **Claim / model** — *the radical-pair quantum model of magnetosensitivity.*
   - **Evidence** — *"In MagLOV2-expressing E. coli, the sign of the mean magnetic-field effect (MFE)
     flipped from positive to negative as magnetic field strength increased at low fields."*
   - **Key figure** — mean MFE at **0.5, 1.0, 1.5, 2.0, 2.5 mT** (1 experiment each, **74 colonies
     each**; SEM with fit + cycle + biological variation propagated). The curve is **positive at low
     field, crosses zero, negative by ~2.5 mT.** (A representative single-colony trace: +MFE at 1.0 mT,
     −MFE at 2.5 mT.)
   - **Study** — *"Bacterioscope live fluorescence imaging of MagLOV2-expressing E. coli colonies
     across 5 magnetic field strengths"* (1 "experiment" = 5 imaging sessions on 5 days; Twinleaf
     magnetometer-calibrated field).
   - **Protocol** — *"MagLOV2 expression in E. coli; bacterioscope acquisition under applied field;
     per-colony MFE calculation."*
   - **Pointers (R2):** `git` → `quantum-bio/mfe-analysis @ 7c1ae04 · /mfe_fit_analysis/` ·
     `data` → curated `mfe_by_field.csv` (the spreadsheet *for this figure*, not 50 CSVs) ·
     `local` → raw NDTiff stacks ≈ large, institute drive (**request access** — archival TBD) ·
     `video` → a short walkthrough · `doi`/`koi` → the minted public handle once published.
   - **Provenance:** *Brian & Morgan · Quantum Biology Institute · CC-BY.* Part of "multi-omics of
     magnetosensitivity in microbial model organisms."
5. **Honour the rules visually** — pointers not payloads (R2: show git/data/local/video/doi chips,
   never a raw-data download); summary-up-front-then-traverse (R4); **permissions even when public**
   (R5: the raw-stack pointer reads *request access*; any withheld working note appears as
   "1 note withheld", never leaked); **addressable at a URL** (R6: `.winbar` address bar / a citable
   handle); **trust via provenance** (R8: byline with lab + license); **freeze the cited version**
   (R9: a `.cite` chip with a pinned version).
6. **Caption every screen.** Top of `<body>`, a `.frame-caption` with an `.eyebrow`
   (e.g. `MOCKUP 03 · PUBLIC WEB VIEW`), an `<h2>`, and one sentence naming the rule/insight it
   embodies. Use the eyebrow tier marker **🥉 low shared context** where natural.
7. **Animate the page load** with staggered `.reveal .d1…d8` and at least one signature motion moment
   (an edge drawing, a toast sliding in, a version pin snapping, bundles snapping into a contents
   list). CSS-only. `prefers-reduced-motion` is already handled in `components.css`.
8. **No external JS frameworks.** A tiny inline vanilla-JS interaction is fine, but the screen must
   look complete and correct with JS disabled (static-first).
9. **Self-contained & openable** by double-clicking (relative CSS hrefs, no build step, no network
   beyond Google Fonts). Center content, target ~1080px max; responsive down to ~720px is a plus.

## Aesthetic direction (commit to it)

Warm-paper **lab-notebook meets a credible open-science publication**. Minimal-scientific, confident,
editorial. Generous negative space, a faint dotted grid (already on `body`), soft layered shadows,
hairline rules. Fraunces for headings, Hanken Grotesk for UI, JetBrains Mono for anything
machine-facing (pointers, JSON-LD, URLs, RIDs, DOIs, version hashes). The feeling: *"a result, with
just enough of its lineage, standing on its own at a public URL — trustworthy to a stranger."*

Two chrome modes:
- **In-app (Obsidian)** screens use `.theme-obsidian` on a wrapper (dark editor pane, faux ribbon /
  file list); the DG nodes sit inside. *(Used by mockup 01.)*
- **Public web / standalone** screens use the light paper theme and the `.winbar` with an address bar
  / a journal-like masthead. *(Used by 03, 04, 05.)*

## Cross-screen consistency

- Same people, same colours: **Brian = `.avatar.brian` (blue-teal), Morgan = `.avatar.morgan`
  (green-blue), QBI lab = `.avatar.qbi`, a stranger reader = `.avatar.world`.**
- Same node titles/wording across screens (copy from §4 above) so the five mockups read as one
  continuous story.
- Edge labels use the exact relation names: `addresses`, `supports`, `grounds`, `follows`.
- Footer line on each screen (tiny, muted):
  `MIRA × Discourse Graphs · publish to a public repository · draft v0.1`.

## The five screens

| # | File | What it shows | Chrome | Rules |
|---|---|---|---|---|
| 01 | `01-publish.html` | **Producer publishes to the public.** Brian selects the `Evidence` bundle in Obsidian and opens a **Publish** dialog: pick destination(s) (DG web DB · Jupyter Book · desci/nanopub · request PREreview), set each node **public vs. request-access-gated** (start closed; raw stack gated), add summary + methods table + walkthrough + "format-in-schema". A private working note is **withheld without leaking**. | Obsidian (dark) | R1, R2, R4, R5, R12 |
| 02 | `02-evidence-node.html` | **The Evidence node, read cold.** Summary figure (the MFE sign-flip) first, observation statement, "what's in the dish" methods table, **pointer chips**; framed as **published · addressable · citable**. The summary↔traversal duality (PI glance vs. reproducer drill). | light card / panel | R2, R3, R4, R8 |
| 03 | `03-public-web-view.html` | **The defining screen — published at a public URL, no DG app.** A credible scholarly page at a URL: summary-first, **traversable** `Claim ← Evidence ← Study → Protocol`, pointer chips, provenance byline (QBI · CC-BY), and a conversion sidebar: **cite this (frozen version)**, **request access** to the gated raw data, **import if you run a DG**. *(This is the in-repo reference screen — model the others on it.)* | public web (`.winbar`) | R4, R5, R6, R7, R8, R9 |
| 04 | `04-micropublication.html` | **"Specs instead of papers" — compile bundles into a narrative.** Several independently-addressable `Evidence` bundles compiled into one **Jupyter Book micropublication**; a **Claim/model emerges above** them (radical-pair model); the narrative is **AI-drafted, human-edited**; each subfigure panel **links back to its addressable bundle**, which stays live. | public web / book | R7, R11 |
| 05 | `05-public-database.html` | **The public database / dashboard + reuse.** A gallery/kanban of published DG nodes on the **web interface**, each with a **public URL**, provenance, and a **"cited by N"** reuse signal; plus a reader **citing a specific result** into their own notebook with a **frozen version pin**, and the author seeing **who is using their work**. | public web (`.winbar`) | R1, R6, R8, R9 |

## The dashboard worked example (CENTRAL) — Sean → Kate

Use this **verbatim** across screens 06–09 (from [`_grounding-dashboard.md`](./_grounding-dashboard.md)
§2.2–2.3). Caption every plot **"illustrative · unpublished — workshop prototype."** Never invent
contradicting science. The transcript names only "two proteins"; we use **G3BP1 / PABP1** for
cross-repo consistency with the inter-lab story.

- **Cast** — **Sean** (`.avatar.sean`, Vogel Lab, producer) · **Kate** (`.avatar.kate`, PI) · **a grad
  student in Kate's lab** (`.avatar.grad`, reproducer). Same colours everywhere.
- **Claim / result** — *G3BP1 is recruited to forming stress granules **before** PABP1* (an assembly-
  order result).
- **Evidence — key figure** — two **intensity-vs-time** curves in stressed HeLa cells: **G3BP1 climbs
  earlier, PABP1 lags**, as granules form (order reverses as they dissolve).
- **Study** (the producers say *"the experiment"*) — *"live fluorescence microscopy of stressed HeLa
  cells, two-channel, tracking per-granule intensity over time."*
- **Protocol** — *HeLa culture → stress induction → live imaging → **segmentation (threshold)** →
  per-granule intensity quantification.* **The segmentation threshold is the load-bearing methods
  detail** the grad student traverses to.
- **Pointers (R2)** — `git` → `vogel-lab/sg-imaging @ a3f9c21 · /segmentation/` · `data` → curated
  `granule_intensity_by_time.csv` (intensities *for this figure*) · `local` → raw NDTiff stacks **≈ 80
  GB on the lab's 25 TB array** (**request access**) · `video` → a short **walkthrough** (*"no Zoom
  call"*).
- **Private, withheld** — a **"Sean's working notes"** node: present in the vault, **withheld** on the
  dashboard as *"1 note withheld"* — never leaked (R5).
- **Provenance byline** — *Sean · Vogel Lab · shared with Kate's lab.*
- **Shared tokens (use identical strings on every screen):** dashboard at **`dashboard.mira.science/vogel-kate`**;
  the Evidence node permalink **`mira.science/n/sg-recruit-order-a3f9c21`**; commit **`a3f9c21`**;
  project **"Stress-granule assembly order" (Vogel × Kate consortium)**.
- **The dashboard** — a **read-only** web view at a URL; **public OR password-protected (consortium,
  20+ labs)**; views **Graph · Kanban · Table**; **preset views** *Results* / *Experiments-in-progress
  (funder)* / *Questions & requests (public/recruiting)*; **colorblind-safe**, legible for *"a
  60-year-old-plus professor."*

**Honour the new mechanics visually:** the dashboard is **read-only** (no edit affordances — it's a
viewing surface); **two currents** (results out **and** a `Request` back, the latter using the indigo
`Request` colour + `.request` chip); **public vs. password-protected** (`.share-level.world` vs.
`.share-level.consortium`); **subset-of-the-vault** (Sean shares *part* of his graph); **shared =
visible, not editable**; **candidate** nodes (`.node.candidate`) may ride along.

### Cast & chrome for the dashboard screens
- **In-app (Obsidian)** for **06** — `.theme-obsidian` wrapper, Sean selecting & permissioning his
  subgraph, push-to-dashboard toast/motion.
- **Public/standalone web** for **07, 08, 09** — light paper theme, `.winbar` with
  `dashboard.mira.science/vogel-kate`, a dashboard masthead, the `.viewswitch` and `.preset` controls.

## The dashboard screens (06–09)

| # | File | What it shows | Chrome | Rules |
|---|---|---|---|---|
| 06 | `06-share-to-dashboard.html` | **Sean shares a *selected subgraph* to the dashboard.** In Obsidian he picks the `Evidence`; the plugin proposes the bundle (`Evidence`+`Study`+`Protocol`+pointers) and **offers connected nodes**; he sets **per-node visibility** (raw stacks **request-access-gated**; **"working notes" withheld, not leaked**), chooses **public vs. password-protected (consortium)**, lets a **candidate** node ride along, and **pushes via KOI** (shared = *visible, not editable*). Signature motion: nodes syncing to the dashboard / a publish toast. | Obsidian (dark) | R1, R2, R4, R5, R12 |
| 07 | `07-shared-dashboard.html` | **The shared read-only dashboard — the headline screen.** A web page at `dashboard.mira.science/vogel-kate`: the **`.viewswitch` (Graph · Kanban · Table)**, **`.preset` audience views** (Results / Experiments-in-progress / Questions & requests), a **`.dash-search`** bar, node **cards** (summary + provenance byline + **public/consortium badge** + cited/updated), and **Kate's glance-and-trust** of the G3BP1-before-PABP1 result. Accessibility cues (colorblind-safe, legible). Signature motion: cards populating / the view morphing (kanban columns sliding in). | public web (`.winbar`) | R6, R8 |
| 08 | `08-traverse-and-request.html` | **A grad student traverses to the threshold, then requests back.** Open Sean's `Evidence` on the dashboard → the two-protein plot → traverse `Evidence ←grounds← Study →follows→ Protocol` to the **segmentation threshold** node → follow `git`/`data` **pointer chips** + the **video** walkthrough → hit **request-access** on the 80 GB stacks → **send a `Request`** (`.request`, indigo) for a follow-up experiment/analysis (`request_for`). Signature motion: the traversal rail drawing down to the threshold, or the `Request` sending. | public web (`.winbar`) | R2, R4, R5, R7 (+ Request) |
| 09 | `09-consortium-view.html` | **Kate runs it as a consortium + a recruiting page.** One **project umbrella**, **multiple collaborators** (Sean + others) under selective visibility; a **password-protected** consortium board vs. a **public** lab-website view; **preset views per audience** (a **funder** seeing experiments-in-progress; the **public** seeing open questions/requests for **recruiting**); unpublished one-off results made **discoverable** instead of orphaned. Signature motion: the audience preset switching what's visible / a lock toggling public↔consortium. | public web (`.winbar`) | R5, R6, R8 |

## What "great" looks like

Not a wireframe — a screen you could hand to an engineer and a user. Real copy, real pointers, real
permission states (public / gated / withheld), real typography hierarchy, purposeful colour, one
memorable motion moment. Restraint and precision over decoration. The five screens should read as one
product and as a clear sibling of the inter-lab mockups.
