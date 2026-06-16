# Developer handoff — the Sean → Kate dashboard (screens 06–11)

### MIRA × Discourse Graphs · generated from [`ux-spec.md`](./ux-spec.md), verified against the built mockups · current as of 2026-06-15

> **What this is.** The engineering spec-sheet for the **dashboard track** (mockups **06–11**). It
> translates [`ux-spec.md`](./ux-spec.md) into exact measurements, tokens, component props, states,
> breakpoints, animations, edge cases, and accessibility notes — and it was **checked screen-by-screen
> against the built HTML/CSS** so it matches what is actually rendered today (not just the prose).
>
> **Scope:** screens **06–11** (the central Sean → Kate dashboard). The public/QBI track (01–05) is not
> covered here. **Sources of truth, in order:** the **mockup HTML/CSS** (exact values) → this handoff →
> [`ux-spec.md`](./ux-spec.md) (intent) → [`AGENTS.md`](../AGENTS.md) (the normative R1–R13). Where this
> doc and the CSS disagree, the CSS wins — every value below was read from
> [`tokens.css`](./mockups/tokens.css) / [`components.css`](./mockups/components.css) / the screen files.
>
> **Tech stack:** the mockups are **static, single-file HTML** — relative CSS hrefs, no build step, no
> framework, **correct with JS disabled** (a tiny inline vanilla JS adds the interactions). Reproduce that
> contract in any rebuild: **static-first, progressive enhancement, CSS-only motion that honors
> `prefers-reduced-motion`.** See the **[Consistency report (§4)](#4-consistency-report--spec-vs-built)**
> for the short list of residues to fix.

---

## 1. Global foundation (shared by all six screens)

### 1.1 Design tokens
Canonical in [`tokens.css`](./mockups/tokens.css). Reference these tokens, never raw hex.

| Token | Value | Usage |
|---|---|---|
| `--ink` | `#1c2024` | Primary text |
| `--ink-2` | `#3a3f47` | Body text on cards |
| `--muted` | `#6b7280` | Secondary text |
| `--faint` | `#6b6f77` | Tertiary/metadata · **AA-corrected** (don't revert) |
| `--paper` | `#faf8f4` | Page background (warm paper + dotted grid) |
| `--surface` | `#ffffff` | Cards / panels |
| `--surface-2` | `#f4f2ec` | Inset surfaces, code wells, pointer chips |
| `--line` / `--line-2` | `#e7e3d8` / `#ece9e0` | Hairlines |
| `--brand` / `--brand-press` / `--brand-tint` | `#2f3a8c` / `#232c6e` / `#eceefb` | Primary actions, links, active preset, frozen pin |
| **Node — Question** | ink `#8f6210` · wash `#fbf0d4` · edge `#e7cd8f` | gold · AA-corrected ink |
| **Node — Claim** | ink `#2c9a61` · wash `#ddf2e5` · edge `#a6dcbd` | green |
| **Node — Evidence** | ink `#d9573f` · wash `#fbe0da` · edge `#f0b1a3` | coral |
| **Node — Study** | ink `#3877cf` · wash `#dbe9fb` · edge `#a6c6ef` | blue |
| **Node — Protocol** | ink `#8460c5` · wash `#ece2f8` · edge `#c9b3ea` | violet |
| **Node — Request** | ink `#4854bf` · wash `#e2e3f7` · edge `#aeb4eb` | indigo (reverse current) |
| **Node — Source** | ink `#137068` · wash `#d7f0ee` · edge `#98d9d3` | teal · AA-corrected ink |
| `--ok` / `--warn` / `--danger` / `--private` | `#2c9a61` / `#8f6210` / `#d9573f` / `#8a8f98` | status / signal |
| `--r-sm…--r-pill` | `6 / 10 / 16 / 22 / 999px` | radii |
| `--gap` | `16px` (→ `20px` in `.pref-spacious`) | base gutter |
| `--ease` / `--ease-out` | `cubic-bezier(.2,.7,.2,1)` / `(.16,1,.3,1)` | motion |
| `--shadow-1/2/3` | layered (rest / hover / lift) | elevation |
| `--font-display` / `--font-body` / `--font-mono` | Fraunces / Hanken Grotesk / JetBrains Mono | headings·node titles / UI·body / machine-facing |

**Type scale:** `h1 clamp(28px,4vw,44px)`, `h2 24px`, `h3 18px`, body `15px/1.5`. **Mono is for anything
machine-emitted** (pointers, URLs, RIDs, DOIs, hashes, eyebrows, edge labels). **Dark chrome:**
`.theme-obsidian` (producer screen 06 only).

### 1.2 Responsive system (the recurring pattern)
Every reader screen (07–09, 11) uses the same scaffold — **a fluid main column + a fixed-width right rail**:
```
.<screen>-grid { display:grid; grid-template-columns: minmax(0,1fr) <RAIL>px; gap:<G>px; align-items:start; }
```
| Screen | Rail width | Main↔rail gap | Rail collapses below | On collapse |
|---|---|---|---|---|
| 07 | **318px** | 22px | **900px** | rail → `order:-1` (above board), `position:static` |
| 08 | **312px** | 32px | **900px** | rail → `order:2` (below doc), `position:static` |
| 09 | **326px** | 22px | **920px** | rail → `order:-1`, `position:static` |
| 11 | flexible rail; **404px** phone device | 26px | **820px** | `.prep-stage` → `flex-direction:column` |

Secondary breakpoints: **kanban → 1 column at `640px`** (07) / `620px` (09 recruit grid, 08 figure
stacks). `.screen` max-width is **1080px** everywhere except **11 = 980px**. Every screen also carries a
`@media (prefers-reduced-motion: reduce)` block. **Mobile-first caveat (11):** authored desktop-default
with a `max-width:820px` override (not a min-width cascade); the phone frame is a constant 404px column —
see [§4](#4-consistency-report--spec-vs-built) for the sub-44px tap-target fixes.

### 1.3 Shared components (the kit — [`components.css`](./mockups/components.css))
Props are expressed as **modifier classes** (static HTML, no JS framework).

| Component | Variants / states | Key spec | Notes |
|---|---|---|---|
| `.ntype` (type badge) | `.question/.claim/.evidence/.study/.protocol/.request/.source/.candidate` | mono 10.5px/600, `letter-spacing .1em`, uppercase, pad `3px 8px`, pill radius, 7×7px leading dot | colour = node type |
| `.node` (node card) | type modifiers; `.private` (45° hatch, dashed, `opacity .85`); `.candidate` (dashed, faint, `--surface-2`) | `border-left:4px`, radius `--r-md`, pad `14px 16px`, `--shadow-1`→`--shadow-2` hover, transition `.25s --ease` | left-border = type |
| `.ecard` | collapsed / method-open / candidate / with-thread (see §3) | `border-left:4px --evidence-ink`, pad `16px`, `min-width:0` | **the headline component** |
| `.btn` | `.primary` (brand + shadow-1, hover `--brand-press`) · `.ghost` · `.sm` (pad `6px 11px`) | pad `9px 16px`, radius `--r-sm`, 13.5px/600, transition `.18s`; hover `translateY(-1px)` | |
| `.pointer` + `.kind.*` | `git #24292f` · `s3 #d9573f` · `local #8a8f98` · `video #b3358f` · `data #2c9a61` · `doi #2f3a8c` · `koi #4854bf`; `.gated` = dashed `--question-edge` | mono 12px, pad `6px 10px`, `--surface-2` | **chips, never a download** |
| `.share-level` | `.world` / `.gated` / `.consortium` / `.public` / `.lab` / `.private` | pill, pad `5px 11px`, 12px/600 | consortium = request-tint |
| `.edge` | `.supports/.opposes/.grounds/.follows/.uses/.addresses/.request_for/.request_target` | mono 11px/600 | typed relation |
| `.viewswitch` | active button = white + `--shadow-1` | segmented pill, button pad `6px 14px`/12.5px | **Graph · Kanban only** (no Table — §4) |
| `.preset` | `.active` (brand-tint, solid border) | dashed pill, pad `6px 12px`/12px | audience filter |
| `.request` | `.open` (dashed) | request-tint pill, pad `6px 12px` | reverse current |
| `.toast` | — | `border-left:4px --brand`, `--shadow-2` | convert/notify |
| `.avatar` | `.sean/.kate/.grad/.vogel/.lab` (+ QBI set) | 28px disc, fixed gradient per person | one colour/person |
| scaffolding | `.panel(.pad=20px)` · `.divider` · `.tag` · `.frame-caption`+`.eyebrow` · `.screen`(radius `--r-xl`, `--shadow-3`, `overflow:hidden`) · `.winbar`(addr pill) · `.kbd` · `.steps` | — | |

**Dashboard-specific atoms** (also in `components.css`): `.kcard__grip`, `.kcol--drop`, `.kcol__ghost`,
`.relfeed*`, `.a11y-pop`/`.a11y-seg`/`.switch`, `.ctx-menu`/`.group-pill`/`.sv-row`(share-verify),
`.agenda-tag`(`.suggested`), `.due-chip`(`.overdue`), `.follow-btn`(`.is-following`)/`.follow-pill`,
`.diff-since`/`.diff-group`(`.agenda`)/`.diff-row`/`.chg`(`.advanced/.evidence/.comments/.idle`),
`.thread`/`.comment`/`.convert-btn`(`.to-evidence`)/`.promoted-chip`, `.state-note`. **Retired:**
`.nudge-btn` (removed); `.stale-flag` re-homed as a **calm muted** idle marker only — never an alarm.

### 1.4 Motion primitives
| Name | Where | Spec | Reduced-motion |
|---|---|---|---|
| `rise` / `.reveal .d1…d8` | every screen, load | `opacity 0→1` + `translateY(10px→0)`, `.6s --ease-out`; stagger delays `.05s`→`.54s` | `.reveal` animation off (`components.css`) |
| `drawline` (`.pdraw`) | plot curves (06/07/08/10) | `stroke-dashoffset` draw-on, `1.3s --ease-out`, delay ~`.35–.45s` | `stroke-dashoffset:0` |
| `pulse` (`.pulse`) | **continuous** soft ring — request-access btn (08), convert btn (10) | `2.4s ease-in-out infinite` | silenced |
| chevron rotate | `.ecard__method[open]`, `.diff-group[open]`, `.verbatim[open]` | `transform: rotate(90deg)`, `.2s --ease` | silenced |
| method-body reveal | `.ecard__method` open | `rise .3s` | silenced |

CSS-only; one signature moment per screen. **Note:** continuous `.pulse` on 08/10 mildly conflicts with
the "no auto-play" quieter principle — see [§4](#4-consistency-report--spec-vs-built).

### 1.5 Accessibility baseline (and the gaps to close)
**Solid today:** `lang="en"`; node-type colour always paired with a text label (never hue-alone); result
accents are **bold + italic + colour**; decorative SVG `aria-hidden`; plots are `role="img"` with a
descriptive `aria-label`; thread regions are `role="log" aria-live="polite"`; native `<details>/<summary>`
for all disclosures (keyboard-operable, JS-free); AA contrast on text tokens (§1.1).

**Close these in a production build (consistent across screens):**
1. **Kanban grips are decorative** (`aria-hidden`, no `tabindex`/key handler). The spec requires a
   **keyboard reorder path** (focus grip → `Space` lift → `↑/↓` → `Space` drop) — **not yet built** (07, 09).
2. `.viewswitch` (07) and `.audience-lock` (09) use `role="tab"/group` but lack **roving `tabindex` /
   arrow-key** movement and complete `aria-selected`/`aria-controls`.
3. Some toggles set body classes without flipping `aria-pressed` — **11's Serif/Dyslexia buttons** (and
   07's "Compact" spacing stop is functionally inert, == Default).
4. Clickable diff rows (11) rely on the UA focus ring — add an explicit `:focus-visible` outline.

---

## 2. Per-screen handoff

### 06 · Share a selected subgraph — [`06-share-to-dashboard.html`](./mockups/06-share-to-dashboard.html)
**Overview.** Producer (Sean) in **Obsidian (dark, `.theme-obsidian`)** shares a selected subgraph to the
dashboard. Outer light `.winbar` (addr `Obsidian — Vogel Lab vault — SG composition over time.md`) wraps
the dark shell; the floating share **modal resets to light tokens**.

**Layout.** `.obsidian-shell` = flex: `.ribbon` 48px · `.filetree` 234px · `.editor` flex-1 (`min-width:0`,
pad `26px 30px`). Share **modal** `max-width:600px`, radius `--r-lg`, `--shadow-3`; body `max-height:68vh`
scroll. Bundle cards `.bcard` = grid `26px 1fr auto`, `border-left:4px` by type.
**Breakpoints:** `560px` audience options → 1 col; `720px` hide filetree + drop modal max-height; `900px`
hide the sync-rail; 3 reduced-motion blocks.

| Component / state | Spec & behavior |
|---|---|
| **Multi-select "Share with"** (`.ctx-menu` + `.ctx-check` + `.cbx`) | Checkbox rows, **≥2 targets at once**; each `onclick` recomputes `#shareCount` and sets row `aria-checked`. Button: **"Share with N selected…"** (default N=2). Header "Share with — pick one or more". Targets — groups: `.group-pill.consortium` **Kate's Lab**, **McGill Consortium**; `.group-pill.world` **McGill Public**. People: **Kate**, **Khalid · Montreal** (inter-lab face). |
| **Bundle picker** (`.bcard`) | `.cbx` per node with `aria-label` ("Include Evidence node", "Study auto-included", …). Offered/connected cards quiet (`opacity:.72`) until ticked (`syncOffered()` toggles `.on`). Candidate rides along with a "informal · shared as-is" pill. |
| **Per-node visibility** (`.vis-row` + `.share-level`) | shared subgraph + analysis/CSV → **Consortium**; **Raw stacks → Request-access** (`.pointer.gated`, "NDTiff ≈ 80 GB · 25 TB array", **pointer only, not downloaded**). |
| **Withheld note** (`.node.private` + `.privacy-warn`) | "Sean's working notes" shown as **won't be shared**; copy: *"won't leak its existence … no title, no body, no '1 note hidden.'"* (R5). |
| **Audience** (`.aud` radiogroup) | **Public** = "deliberately hosted page" · **Consortium** = "hosted instance · Vogel × Kate · 20+ labs". Explicit copy: *"a local server (localhost:7979) … going public means deliberately hosting it … that act — **not a password** — is what makes it public."* |
| **Mandatory verify sheet** (`.share-verify-overlay`/`.sv-row`) | Opens on push, focuses `#verifyConfirm`. One row per node: `.sv-state.ok` (will share) / `.gate` (request-access) / `.no` (**withheld, shown as withheld**). Confirm → `runSyncAndToast()`. Primary label names count+target: **"Share 4 nodes with Kate's Lab"**. |

**Interactions (inline JS).** `pickAud(el)` (radio), `syncOffered()` (connected-node opt-in),
`runSyncAndToast()` (counts checked bundle nodes, re-triggers the KOI packet-flow animation, reveals the
"Pushed to dashboard · via KOI" toast — **0 delay under reduced-motion**), and the push→verify→confirm
gate. **Motion:** `flow` (packets along an `offset-path` bezier, `1.05s`, staggered `.16s`), `toast-in`
(`.5s`), `drawline` (the two-curve mini Pearson plot), `rise` (verify sheet).
**A11y.** `role="menu"`/`menuitemcheckbox` + synced `aria-checked`; modal `role="dialog"`; audience
`role="radiogroup"`/`radio` + Enter/Space; verify `aria-modal`; plot `aria-label` = the finding; toast
`role="status" aria-live="polite"`. **Consistency: PASS** (science, multi-select, local-vs-hosted,
no-leak, no download all confirmed).

---

### 07 · The shared dashboard — [`07-shared-dashboard.html`](./mockups/07-shared-dashboard.html) · **headline**
**Overview.** The read-only web view at `dashboard.mira.science/vogel-kate` where Kate glances and trusts.
Light + `.winbar`.

**Layout.** `.body-grid` = `minmax(0,1fr) 318px`, gap 22px. Board left; **sticky inspector** right
(`top:18px`). `.kanban` = `repeat(3, minmax(0,1fr))`, gap 14px. `min-width:0` on board/inspector/relfeed/
ecard (overflow guard). **Breakpoints:** `900px` → 1-col, inspector `order:-1` + static; `640px` → kanban
1-col; `760px` → request panel 1-col; reduced-motion kills `.pdraw`.

| Component / state | Spec & behavior |
|---|---|
| **View switch** (`.viewswitch`, `role="tablist"`) | **Exactly two buttons — "Graph" · "Kanban"** (Kanban active default). **No Table.** Graph → `body.show-graph` (hides kanban + request panel, shows `.graphview`). |
| **Audience presets** (`.preset` ×3) | **Results** (active) · **Experiments in progress** · **Questions & requests**. `applyPreset(key)` dims non-matching cards (`.preset-off` = `opacity .26`), relabels `#presetName`, and **spotlights the request composer** (`.reqpanel.preset-hot`) only on *Questions & requests*. |
| **Kanban** (`.kcard`) | Columns **Questions · Experiments · Results**. 7 cards (q1, q2, s1, s2, cand, e1, e2). `border-left:3px` by type; `.sel` = `outline:2px --brand`. Default selected = **e1** (composition Evidence). `.kcol__ghost` "drop here to mark as a result". |
| **Subgraph highlight** | Hover/**focus any card** → `.kanban.subgraph-on` dims all (`opacity .3; saturate .55`), `.subhit` re-lights matches (`mouseenter/focus`→`highlight(sub)`, `mouseleave/blur`→`clearHi`). Map: {q1,s1,cand,e1} share `q1`; {q2,s2,e2} share `q2`. Per-question "N linked" `.sublink`. |
| **Click-to-inspect** | `selectCard(card)` reads `data-node`, toggles `.sel`+`aria-current`, and shows the matching `.insp-panel` (`hidden` on the rest). **Seven per-node panels:** e1·e2·q1·q2·s1·s2·cand — **each reuses `.ecard`** (no new card style). Ignores clicks on `<a>`; Enter/Space supported. |
| **Inspector Evidence card** (`.ecard`) | Plot (Pearson-r SVG, arsenite=evidence-ink, osmotic=study-ink, `≈10–15 min` marker) → summary → caveats → **method `<details>` labelled "How it was done — study & protocol"** (`.ctx-table`, the **SUM-segmentation** note, 4 pointer chips, "Read the full study & protocol →" → 08) → studyfoot "Open full study details →". **No Endorse / no `.trust` block.** `.req-note` "1 open Request from Kate's lab". |
| **Request composer** (`.reqpanel`/`.rq-*`, **beneath the kanban**) | `request_for` segmented [**Study** active · Protocol detail · Raw data] + target chip + textarea (`aria-label`) + **Send → `sendReq07()`** swaps to an on-node confirmation (*"request_for {kind} … on the node now, with your name — no email"*). Hidden in Graph view. |
| **Related-work feed** (`.relfeed`, right rail) | 3 items; each `.relfeed__why` **states the match reason** (`⟡ shared: readout=co-localization · cell line=HeLa`, etc.). |
| **a11y popover** (`.a11y-pop`) | Serif `.switch` (`aria-pressed`, `body.pref-serif`) + Spacing `radiogroup` (Compact/Default/Spacious → `body.pref-spacious`). Persists. |

**Edge/state.** Masthead `.share-level.consortium` "Consortium · 6 labs" + lock; `.ro-pill` read-only;
"illustrative · unpublished" pill on the plot; footer **142 nodes · 6 labs · 1 consortium · 3 open
requests**; gated pointer "request access".
**Consistency: PASS on all functional checks.** ⚠️ **Fix:** the visible caption `<p>` (line ~207) still
reads "switch between Graph · Kanban · **Table**" — stale; should read "Graph · Kanban". (Plus the
header/CSS comments and vestigial `data-q` attrs — cosmetic.) See [§4](#4-consistency-report--spec-vs-built).

---

### 08 · Traverse & request back — [`08-traverse-and-request.html`](./mockups/08-traverse-and-request.html)
**Overview.** A grad student follows the lineage to the method, then sends a Request. Light + `.winbar`
(addr `mira.science/n/sg-composition-019ea6d6`). **Architectural note:** this screen does **not** use
`.ecard` — the lineage is **three separate `.node` cards in a `.chain`**, each with a per-node
`<details class="verbatim">`.

**Layout.** `.doc-grid` = `minmax(0,1fr) 312px`, gap 32px; sticky `.aside` (3 panels). **Breakpoints:**
`900px` → 1-col, rail `order:2` static; `620px` → figure `.plotwrap` stacks (plot above context table),
`.ctx` border flips left→top; 3 reduced-motion blocks.

| Component / state | Spec & behavior |
|---|---|
| **Figure** (`.figure`) | `.plotwrap` = `minmax(0,1.35fr) minmax(0,1fr)` (plot ~57% / `.ctx` table ~43%). `.illus` pill. Curves draw via `.pdraw` (`1.3s`, delay `.4s`). |
| **Framing prompt** (`.ask`) | brand-tint wash; "REPRODUCER" + the question; a down-chevron cue (note: `@keyframes nudge` is **defined but not applied** — dead). |
| **Lineage chain** (`.chain` + `.edge-rail`) | `.node.evidence` ("you are here") → `.edge.grounds` ("grounds ↑", Study→Evidence) → `.node.study` → `.edge.follows` ("follows ↓") → **`.node.protocol.payoff`**. Edge SVGs self-draw (`drawline 1s`, staggered `.d-a .55s`/`.d-b 1.15s`). |
| **The payoff** (`.threshold-detail`, **one protocol-wash**) | `border-left:3px --protocol-ink`, no full border. Title "HeLa culture → stress → Lattice-SIM → **segment on per-pixel SUM** → composition over time"; table row **"Segment on → per-pixel SUM of both"**, "just above background" (no "threshold = 0.18"). |
| **Pointers** | `git` (`segment_stress_granules.ijm`), `data` (`granule_composition_by_time.csv`), **`.pointer.gated` local** ("NDTiff ≈ 80 GB · Elyra 7 · **request access**"); **video** rendered as the player panel, not a chip. |
| **Video walkthrough** (`.video`) | dark poster, 56px round `.play` (`aria-label="Play 2-minute walkthrough"`), magenta `.vidtype` pill. *(Note: JS adds `.played` but there is no `.played` CSS rule — visual no-op; add the rule if a played state is intended.)* |
| **Discussion** (`.thread`, `role="log"`) | 3 comments (Kate↔Sean on why SUM-segment); composer disabled ("Viewing only — sign in to comment"). **Convert** button (`#convReq`, `aria-label`) → **`convertToRequest(this)`**. |
| **Request-back card** (`.req-card`, right rail) | `border-left:3px --request-ink`; `.req-edge` typed `request_for · request_target ↓` → proposed new Study "Finer early time-course (<5 min)"; prefilled body; **Send → reveals `#reqSent`**. `.currents` legend (results out / requests back). |
| **Per-node grounding openers** (`.verbatim` `<details>`) | **Plain human labels** (not "Read verbatim"): Evidence → **"Sean's notes"**, Study → **"Experiment notes"**, Protocol → **"The full protocol"** — each a two-line `<summary>` (`.vlabel`: `b` Fraunces 13.5px + `span` 11.5px muted descriptor) with a `.vicon` doc icon (17px) + `.vchev`. **Restyled to the inset idiom:** `border:0; border-left:3px --line-2; --r-sm; --surface-2` (no dashed border; quieter than the colour-washed payoff). Content per node: Evidence **3 Key Points**; Study **dated Progress & Notes** (2025-08-23, 2025-10-14) + **2 Hypotheses**; Protocol **numbered steps + 2 parameter tables** + "TODO: get growth info from Khalid". `.vsrc` footer keeps "Verbatim · … · protein names redacted". |

**Canonical convert handler (reuse this everywhere).** `convertToRequest(btn)` is a real **`<script>`
function** (not an inline `onclick` with nested quotes): hides the button, inserts a `.promoted-chip`
("→ request sent to Sean"), shows a **`.toast` with a 6 s Undo** (`setTimeout(…, 6000)` removes Undo but
keeps the chip), uses HTML entities for apostrophes. **This is the canonical implementation** — screen 10
should match it (see §4).
**Consistency: PASS** (per-pixel SUM, science, **no leaked handler code**, no Endorse, ≤2 card-level washes
[`.ask` + `.threshold-detail`], request-access not download). ⚠️ Minor: continuous `.pulse` on the
request-access button; dead `nudge` keyframe + `.played` rule.

---

### 09 · Consortium view & recruiting — [`09-consortium-view.html`](./mockups/09-consortium-view.html)
**Overview.** Kate runs one board as a consortium and, **with one toggle**, as a public recruiting page.
Light + `.winbar`; the **address bar rewrites** consortium ↔ public.

**Layout.** `.body-grid` = `minmax(0,1fr) 326px`, gap 22px; sticky `.rail` (Collaborators · Now
discoverable · In plain language). **Breakpoints:** `920px` → 1-col, rail `order:-1`; `640px` kanban
1-col; `620px` recruit grid 1-col; reduced-motion block.

| Component / state | Spec & behavior |
|---|---|
| **Audience toggle** (`.viewswitch.audience-lock`, `role="group"`) | **Consortium** (`.con`, request-ink) ↔ **Public** (`.pub`, claim-ink). `setAudience(mode)` sets `body.is-public`, which (CSS-only) swaps the addr bar (`🔒 dashboard.mira.science/vogel-kate` ↔ `🌐 vogel-kate.mira.science`), the `.share-level` badge (**password-protected** ↔ **public · lab website**), the lab-access line, the build line, and **dims `.con-only` cards** (`opacity .42; pointer-events:none`) showing a "consortium" lock-tag. |
| **Presets** (`.preset` ×3) | **Experiments in progress · funder** (active) · **Open questions & requests · recruiting** · **Results →** (link to 07). `setPreset()` toggles the `.preset-board.active`. |
| **Funder kanban** | Columns **Planned · Running · In analysis** *(note: column is "In analysis"; individual cards may append "· in progress" via `.inprog`)*. **Per-study progress meter** `.kbar` (4px, study-ink fill; widths 12/8/58/44/86/72%). |
| **Grab handles** (`.kcard__grip`) | **On all 8 cards**, **persistently visible** — screen-local override `opacity:.4` (hover/focus `.75`), intentionally overriding the base `opacity:0`. *(Recruiting `.rcard`s correctly have none — read-only gallery.)* **⚠ keep this override** on any CSS consolidation, or grips vanish at rest. |
| **Meeting-anchored mechanics** | `.agenda-tag` **"For next sync · Jun 16"** (calm); owner-set `.due-chip.overdue` **"next step by Jun 6 · 5d over"** (muted, *not* red; title credits Sean); `.follow-btn` toggling Follow↔Following (title: "he granted this"). **No `.nudge-btn`, no `.stale-flag` alarm.** |
| **Collaborators rail** (`.collab`) | per-lab visibility mini-matrix `.mtx` (consortium / public cells). |
| **Now discoverable** (`.feed`) | orphaned one-off results made searchable; "pointers, not payloads — no raw data on the board". |
| **In plain language** (`.plain`) | plain↔technical toggle (`.ptoggle`); colorblind/legibility note. |

**Edge/state.** Footer **318 nodes · ~20 labs · 1 consortium · 5 collaborating labs · 2 open requests**;
recruiting cards use `.recruit-btn` ("express interest" → "✓ Interest noted").
**Consistency: PASS** (no nudge/poke; grip on every kanban card; owner-set agenda/deadline/follow; science;
PI receives-never-initiates). ⚠ A11y: toggle lacks roving tabindex/arrow keys; grips not keyboard-operable.

---

### 10 · The evidence card — embedded method — [`10-evidence-card.html`](./mockups/10-evidence-card.html)
**Overview.** The isolated reference screen for `.ecard` at **all four states**. Light + `.winbar`.

**Layout.** `.gallery` = `repeat(2, minmax(0,1fr))`, gap `28px 24px`, 2×2. **Breakpoint:** `840px` → 1-col.

| State | What's shown |
|---|---|
| **(a) Collapsed** (PI glance) | top → plot → summary → caveats → byline → **method `<details>` closed** → studyfoot. Method summary label: **"Method & context"**. |
| **(b) Method-open** (reproducer) | `<details open>`: Question line + `.ctx-table` (Cell line/Stress/Channels `Protein 1–GFP · Protein 2–mKate`/Imaging/Readout) + highlighted **`tr.threshold-row` "Segmentation = on per-pixel SUM · load-bearing"** (pulses once via `pulse-row`) + 4 pointer chips. |
| **(c) Candidate** (no result) | `.node.candidate` (dashed/faint); **`.no-result-ph` "No result posted yet"**; muted summary; `.trouble-note` (troubleshooting preserved); **"Convert to evidence"** button (`.convert-btn.to-evidence.convert-pulse`); no method/studyfoot. |
| **(d) With-thread** | `.ecard` (abbreviated plot, method collapsed) + a **separate `.thread`** (3 comments + disabled composer + **"Convert to request"**). |

**Interactions.** Candidate→evidence (`#convertToEvidence`) changes the card type live (badge + border →
evidence). **⚠ Convert-to-request here is an inline-`onclick` IIFE with no toast and no Undo** — it only
inserts the `.promoted-chip`. This **diverges from the canonical handler in 08** (`convertToRequest()` +
toast + 6 s Undo). **Fix:** replace 10's inline handler with 08's `<script>` function so the headline
component's reference screen shows the canonical behavior.
**Motion.** `pulse-row` (one-shot threshold highlight, `2.2s`, delay `1.8s`), `.convert-pulse` (continuous),
`drawline`, method chevron rotate.
**A11y.** Per-card `aria-label` ("…state N: …"); plots `role="img"`; `.no-result-ph` `role="img"`; thread
`role="log" aria-live="polite"`; convert buttons have descriptive `aria-label`; native `<details>`.
**Consistency: PASS** on four-states / science / no-Endorse / **no rendered anatomy strip**. ⚠ **Fix:**
(1) the convert-to-request handler (above); (2) orphaned `.anatomy*` CSS (lines ~37–66) + `<title>`/comment
still say "anatomy" though nothing renders — prune. See [§4](#4-consistency-report--spec-vs-built).

---

### 11 · Meeting prep — since last sync — [`11-meeting-prep.html`](./mockups/11-meeting-prep.html)
**Overview.** A **mobile-first** pull surface the student opens to prepare — the calm replacement for the
nudge. Light + `.winbar` (addr `…/vogel-kate/prep`).

**Layout.** `.prep-stage` = flex: `.device` (the phone, **404px**, centered) + sticky `.why` rail.
`.screen` max-width **980px**. **Breakpoint:** `820px` → `flex-direction:column`, rail static.

| Component / state | Spec & behavior |
|---|---|
| **Date + export** (`.diff-since`) | `.date-pick` "since Jun 4 · last sync" + `.mini-btn` Export (both visual-only in the mock). |
| **Diff groups** (`.diff-group` ×3) | **On the agenda** (`3 · marked for this sync`, request-accent, always open) → **What changed** (`5 since Jun 4`) → **`<details>` "Not changed in a while" (`2 · folded`, collapsed by default)**. |
| **Change-verb chips** (`.chg`) | `.evidence` "new evidence" · `.advanced` "advanced" · `.comments` "N comments" · **`.idle` "no update · 19 d"** (muted/mono, **calm, never red**, only inside the folded group). |
| **Clickable rows** | `<script>` sets `role="link"`+`tabindex="0"` and navigates to **08** on click/Enter — **except rows owning a `<button>`** (`if (row.querySelector('button, a')) return;`), which exempts the **`.due-chip` "set next step by ▸"** backlog row. |
| **Right rail** | **This sync** ("Tap any row to open its node") · **Change key** legend · **Followers** (`.follow-pill` Kate; **"Owner-granted — Sean can revoke anytime."**) · **Accessibility** (`Serif`, `Dyslexia-friendly`). |

**Edge/state.** Footer **3 on the agenda · 5 changed since Jun 4 · 1 followed card · owner-granted ·
Read-only**. **No "why there's no nudge" justification anywhere** (header/rail/footer) — the screen shows
the product, not the argument.
**Consistency: PASS** (no nudge-justification; clickable rows + control-row exempt; staleness folded &
calm; followers owner-granted/revocable; science framing). ⚠ **Fix (mobile-first):** enlarge sub-44px tap
targets — `.date-pick`/`.mini-btn` (~30px) and the `.due-chip` button (~20px) → ≥44px. A11y: Serif/Dyslexia
buttons should flip `aria-pressed`; add `:focus-visible` on clickable rows.

---

## 3. The Evidence card (`.ecard`) — component contract
The single headline component (used by 07 inspector ×7 panels and 10 ×4 states; **not** by 08, which uses
the `.chain`). Build it once; vary by state class/attribute.

**Anatomy (order is load-bearing — result first):** `.ecard__top` (`.ntype` + `.share-level`) →
`.ecard__plot` (+ `.pcap` legend + `.illus` pill) → `.ecard__summary` (Fraunces 16.5px; `.accent` =
evidence-ink italic 600) → `.ecard__caveats` (muted bullets) → byline → **`.ecard__method`** (native
`<details>`, summary label **"How it was done — study & protocol"** in 07 / "Method & context" in 10 —
unify on one label) → `.ecard__studyfoot` ("Open full study details →").

| Prop / state | How | Behavior |
|---|---|---|
| `state=collapsed` | `<details>` closed | PI glance; method one click away (JS-free). |
| `state=method-open` | `<details open>` | shows `.ctx-table` + the **SUM-segmentation** load-bearing row (one-shot `pulse-row`) + pointer chips + a link to the full study & protocol (→ 08). |
| `state=candidate` | `.ecard.node.candidate`, no plot | `.no-result-ph` "No result posted yet" + `.trouble-note` + **Convert to evidence**. |
| `state=with-thread` | append `.thread` sibling | comments separate from body; **Convert to request** (canonical handler, §08). |
| (never) | — | **No Endorse / approve / sign-off** in any state. |

`border-left:4px --evidence-ink`, pad 16px, radius `--r-md`, `--shadow-1`→`2` hover, `min-width:0`.
Chevron rotates 90° on open; method-body `rise .3s`; all respect reduced-motion. Focus order: byline →
plot (`role="img"`, alt = the finding) → summary → caveats → method `<summary>` → studyfoot.

---

## 4. Consistency report — spec vs. built
Verdict: **the mockups are consistent with [`ux-spec.md`](./ux-spec.md) on every load-bearing
current-truth fact** — composition-over-time science (no G3BP1/PABP1/recruitment/assembly-order; proteins
redacted to Protein 1/2), **no Endorse**, **no PI nudge** (agenda/due/follow instead), **Graph · Kanban**
(no Table tab), **per-pixel SUM** payoff, multi-select share + withheld-not-leaked, grips on every kanban
card, the rendered card-anatomy strip removed, and no nudge-justification copy. The residues below are
**polish, not load-bearing** — none changes behavior, but fix them so the mockups and spec agree verbatim.

| # | Screen | Issue | Severity | Fix |
|---|---|---|---|---|
| 1 | **07** | Visible caption `<p>` (~line 207) reads "Graph · Kanban · **Table**", but the view switch has no Table tab | **Med** (user-visible text contradicts UI) | change caption to "Graph · Kanban"; also the file header comment + `components.css` `.viewswitch` comment + `index.html` 07 card |
| 2 | **10** | Convert-to-request is an inline `onclick` IIFE with **no toast, no 6 s Undo** — diverges from 08's canonical `convertToRequest()` | **Med** | reuse 08's `<script>` `convertToRequest(btn)` (toast + 6 s Undo) on 10 |
| 3 | **10** | Orphaned `.anatomy*` CSS (~lines 37–66) + `<title>`/top-comment still say "anatomy" though no strip renders | Low | prune the dead CSS + retitle ("Evidence card — states") |
| 4 | **08** | `@keyframes nudge` defined-but-unused; JS toggles `.played` with no matching CSS rule | Low | remove dead keyframe; add `.video.played` rule or drop the toggle |
| 5 | **08 / 10** | Continuous `.pulse` (request-access btn / convert btn) mildly conflicts with the "no auto-play" quieter principle | Low | consider a one-shot or hover-triggered pulse |
| 6 | **all** | **Keyboard reorder for kanban grips not implemented**; viewswitch/audience-lock lack roving-tabindex/arrow keys; 11 Serif/Dyslexia don't flip `aria-pressed`; clickable rows lack `:focus-visible` | **Med** (a11y) | implement the documented keyboard paths (§1.5) |
| 7 | **11** | Sub-44px tap targets (`.date-pick`, `.mini-btn` ~30px; `.due-chip` ~20px) on a mobile-first screen | Low–Med | enlarge hit areas to ≥44px |
| 8 | **07** | "Compact" spacing stop is functionally inert (== Default); vestigial `data-q` attrs | Trivial | wire Compact (tighter line-height/gap) or drop it; remove `data-q` |

*(One non-issue worth guarding: 09's `.kcard__grip { opacity:.4 }` screen-local override is **intentional**
— do not "consolidate" it back to the base `opacity:0`, or grips become invisible at rest.)*

---

## 5. Implementation notes
- **Read-only surface, one write exception.** Viewers never author (R6). The **only** writes are the
  **owner/PI** setting status (kanban drag), priority (reorder), and the owner setting deadline/agenda/
  follow. Gate these to owner/PI; public viewers get a static board (no grips, no drag).
- **Node fields that drive the UI (not new schema types):** `next_step_by` (owner-set date), `agenda_for`
  (meeting ref), `state ∈ {active, parked, done}` (done/parked excluded from staleness), `followers`
  (owner-granted). The backlog and the "since last sync" diff are **generated** from these — don't build a
  parallel to-do store.
- **Pointers, not payloads (R2).** Every data/analysis/raw reference is a chip; raw stacks always read
  **"request access."** Never render a raw-data download.
- **Permissions never leak (R5).** A withheld node shows as withheld (or not at all) — never its title,
  body, or a dangling edge, including through KOI.
- **Convert handlers live in `<script>`** (never inline `onclick` with nested quotes — that bug leaked JS
  as visible text). Toast + 6 s Undo is the canonical convert UX.
- **Static-first.** Each screen must be correct with JS disabled; JS only enhances (highlight, inspect,
  presets, convert, row-nav). No frameworks; CSS-only motion honoring `prefers-reduced-motion`.

---

*MIRA × Discourse Graphs · developer handoff · Sean → Kate dashboard, screens 06–11 · from [`ux-spec.md`](./ux-spec.md), verified against the built mockups · 2026-06-15*
