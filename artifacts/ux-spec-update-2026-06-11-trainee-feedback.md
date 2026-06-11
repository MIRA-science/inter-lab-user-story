# UX spec update — the trainee reverses the nudge; meetings become the checkpoint

### Developer-handoff delta · **draft v0.2 → v0.3** · source: *Graph filtering ideas with Sean* (Jun 11, 2026) · research layer: [`ux-research-synthesis-2026-06-11-trainee.md`](./ux-research-synthesis-2026-06-11-trainee.md)

> **What this is.** A buildable **delta** to the Sean → Kate dashboard, from the first session where the **trainee
> (Sean)** reacted to the *built* 06–10 mockups. It **supersedes** the parts of
> [`ux-spec-update-2026-06-10-dashboard.md`](./ux-spec-update-2026-06-10-dashboard.md) it names (chiefly **§4's PI
> nudge**), and reuses the design system verbatim ([`tokens.css`](./mockups/tokens.css),
> [`components.css`](./mockups/components.css)). Every new atom is an **extension block**, not a reinvention.
>
> **The one-sentence change.** *Stop pinging the student; show them — on their own surface and against the next
> meeting — what's slated to be discussed and what they said they'd finish.* The need (don't let experiments
> silently rot) is unchanged; the **mechanism** flips from a top-down push to a self-directed, meeting-anchored pull.
> Rules **R4** (summary-first), **R5** (no leaks), **R6** (addressable), **R-readonly** carry over from v0.1/v0.2.

---

## 0. What changed at a glance

| # | Area | Change | Status | Anchoring quote (Jun 11) | Affects |
|---|------|--------|--------|--------------------------|---------|
| 1 | **PI nudge** | **Removed.** The supervisor-initiated "no result in 18 d — nudge the student?" poke is cut. | **REVERSED** (was v0.2 §4) | *"I don't like that one… this is something I would really hate to receive if I'm in [do-not-disturb] mode."* | 09; **supersedes** v0.2 §4, accept. #10 |
| 2 | **Meeting-anchored "agenda"** | Nodes carry an **"For next sync"** state; the calm signal is *"these are what we'll discuss,"* not a notification. | **NEW** | *"Which of these are slated for us to talk about in the meeting?"* (Sean affirmed) | 09, 11; §2 |
| 3 | **Self-set deadline + backlog** | Student sets **"Expected next step by [date]"**; on lapse it surfaces on **their own** board, not as a push. | **NEW** | *"Seeing it appear in a list of backlog… feels safer than getting a ping from your supervisor."* | 09, 11; §3 |
| 4 | **Opt-in "Follow card"** | The **student** invites the PI to follow a card; the PI then gets the **same** signal when the self-set date lapses. Student-initiated, tunable. | **NEW** | *"I'm setting myself this deadline and I want Matt to know… he'll also get the notification."* | 09, 11; §3 |
| 5 | **"Since last sync" diff view** | A **meeting-prep** surface: what changed since a date (advanced / new evidence), with **stale nodes folded** beneath. | **NEW screen** | *"Maybe a diff — what has been updated since this date?… a folded section: these haven't changed."* | **new 11**; §2 |
| 6 | **Derive, don't duplicate** | Deadline / agenda / stale are **node fields**; the backlog & diff are **generated** (Obligator/Notion model). | **NEW** | *"I am kind of duplicating my reporting efforts — in the node and then in my Obligator page."* | 09, 11; §3 |
| 7 | **Graph view, for real** | **Top-to-bottom**; **filter-by-creator**; **filter-by-node = find-subgraph** (bidirectional); hover-emphasize / click-reorient. **Kanban gains connection paths.** | **NEW** (Graph tab was a facade) | *"Filter by node AKA find subgraph… it should work bidirectionally."* / *"top-to-bottom… for a big graph."* | 07, 09; §4 |
| 8 | **Mobile-first + dyslexia** | Meeting/glance surfaces are **mobile layouts** (PI persona Jackie attends on a phone/iPad mini); a11y bar gains a **dyslexia-friendly** default. | **NEW** | *"The only thing she shows up with to meetings is a phone or iPad mini."* / *"this one should be dyslexia-friendly."* | 07–11; §5 |

> **Confirmed (no change):** audience presets, Graph·Kanban·Table switch, per-study progress meter, the embedded-method
> Evidence card (§2 of v0.2), the comment thread + convert-to-request. The trainee's quarrel was **only** with the
> nudge mechanism — everything else held.

---

## 1. The reversal — why the nudge dies and what stands in for it

### The finding
The most-built feature of the Jun 10 set — a **PI → student nudge** on stale experiments — was rejected **to its
face** by the trainee it targets, for three reasons the design must respect:
1. **It breaks deep-focus modes.** *"Sometimes I chuck my phone in a drawer… everything on do-not-disturb… I just
   want to lock in."* A push is exactly the wrong thing in that mode.
2. **It reads as consumer-app pressure.** *"People hate emails… it's not like social media — we're making scientific
   sharing."*
3. **It inverts the relationship.** A poke from above feels vertical/surveilling; accountability should be
   *"bi-directional… horizontal,"* and **student-requested**.

### The replacement (three parts, all in §2–§3)
| Out (v0.2) | In (v0.3) | Why |
|---|---|---|
| `.nudge-btn` — PI pings student | **`.agenda-tag` "For next sync"** — node is *shown* as slated for the meeting | The meeting is the real checkpoint, not the phone |
| `.stale-flag` as an alert | **`.due-chip` "Next step by ⟨date⟩"** set by the *student* → lapses onto **their own** backlog | Self-imposed, self-surfaced — psychologically safe |
| (PI initiates) | **`.follow-btn` "Follow"** — *student* invites the PI to follow; PI then gets the same lapse signal | Opt-in, tunable accountability the student controls |

> **Permission inversion (load-bearing).** In v0.2 the PI could mutate/poke. In v0.3 the **only** actor who can
> set a deadline or grant a follow is the **node's owner (the student)**. The PI **receives**, never initiates.

---

## 2. Meeting-anchored agenda + the "Since last sync" diff view  *(the new checkpoint)*

### 2a. The `.agenda-tag` ("For next sync")
A node can be marked **on the agenda** for the next meeting — the calm replacement for the ping.
| Element | Spec |
|---|---|
| `.agenda-tag` | pill, `--request-wash` / `--request-ink`, calendar glyph + label **"For next sync · ⟨date⟩"**; sits in the card's status row |
| Who sets it | the **owner** marks it, **or** a followed PI proposes it (appears as *"Kate added to agenda"* — a suggestion, not a poke) |
| Where it shows | on the Kanban card (09) **and** as the top group of the diff view (11) — *"these are what we'll discuss"* |
| Empty state | no agenda tags → the meeting-prep view shows *"Nothing flagged for the next sync yet — mark studies you'll discuss."* |

### 2b. New screen — `11-meeting-prep.html` ("Since last sync")
The **meeting-prep home**: what changed since a chosen date, stale work folded beneath. This is where staleness
lives — a **pull** surface the student opens, never a push.
```
┌─ Meeting prep · Kate sync · since ⟨Jun 4⟩  [▾ change date]  [export summary] ─┐
│  ▸ ON THE AGENDA (3)        studies marked "for next sync"                     │  ← Theme 2
│      • G3BP1 recruitment order   ↑ new evidence · 2 comments                   │
│  ▸ WHAT CHANGED (5)         advanced / new result since the date              │  ← the diff, Theme 4
│      • Arsenite dose–response    ▲ advanced    • U2OS replication  ＋evidence   │
│  ▾ NOT CHANGED IN A WHILE (2)   folded by default — staleness lives here       │  ← Theme 4, folded
│      • FRAP fluidity   no update · 19 d   (set a "next step by" date ▸)         │
└────────────────────────────────────────────────────────────────────────────────┘
```
| Element | Spec |
|---|---|
| `.diff-since` header | date picker (`since ⟨date⟩`); default = **date of last sync**; `.btn.ghost.sm` "export summary" (for the phone) |
| `.diff-group` | three groups: **On the agenda** · **What changed** · **Not changed in a while** (the last is `<details>`, **collapsed**) |
| `.diff-row` | node title + a **change verb chip**: `▲ advanced` (`--study`), `＋ new evidence` (`--evidence`), `💬 n comments`, `no update · Nd` (`--muted`, **not** alarm-red) |
| stale row inline action | `.due-chip` "set next step by ▸" — converts a stale item into a self-set deadline **in place** (Theme 3) |
| Mobile | this screen is **mobile-first** (Jackie's phone/iPad mini) — single column, large tap targets (§5) |

> **Tone rule.** Nothing here is red/alarm. *"Understanding that a result hasn't changed is very important, but
> communicating how to change that… feels out of scope."* The diff **informs**; it does not chastise.

---

## 3. Self-set deadlines, the backlog, and the Follow card  *(agency, derived not duplicated)*

### Node fields (derive the backlog — don't add a parallel to-do app)
Add to the study/experiment node (so the backlog and diff are **generated**, per Theme 5 — Obligator/Notion model):
| Field | Type | Drives |
|---|---|---|
| `next_step_by` | date (owner-set, optional) | the `.due-chip`; lapse → appears on the owner's **own backlog** + the diff's stale group |
| `agenda_for` | meeting ref (optional) | the `.agenda-tag` "For next sync" |
| `state` | `active` · `parked` · `done` | **excludes `done`/`parked` from stale-flagging** (*"unless it's finished"*) — resolves a v0.2 gap |
| `followers` | people (owner-granted) | who receives the lapse signal (the PI, only if invited) |

### Components
| Element | Spec |
|---|---|
| `.due-chip` | `--question`-tint pill **"Next step by ⟨date⟩"**; owner-editable inline; on lapse → `--muted` "overdue · 3 d" (calm, not red) |
| `.follow-btn` | `.btn.ghost.sm` **"Follow"** on a card; **owner-only grants**; once followed shows `.follow-pill` "Kate follows" with avatar |
| `.backlog` (owner board) | the student's **own** dashboard view: *"my experiments,"* surfacing lapsed `next_step_by` items — *"the forgotten step-child experiment"* |
| Follow → notify | when a **followed** card's `next_step_by` lapses, the follower gets the **same** signal the owner sees — **in-board badge by default**; email/push is opt-in and deferred (see open Qs) |

### States & rules
| Action | Who | Behavior | Rule |
|---|---|---|---|
| Set `next_step_by` | **owner only** | inline date on the card; persists; drives backlog + diff | student agency |
| Grant Follow | **owner only** | invites a PI to follow; revocable | *"you can request it and tune it as much as you need"* |
| Deadline lapses | system | item surfaces on owner backlog + diff stale group; **followers** badged | **no push by default** |
| PI tries to poke | — | **not possible** — there is no PI-initiated nudge affordance | the reversal (§1) |

---

## 4. Graph view (real) + connection paths on the Kanban

### 4a. Graph view interaction model (the Graph tab stops being a facade)
| Aspect | Spec | Quote |
|---|---|---|
| Orientation | **top-to-bottom** (questions at top → claim → evidence → protocol below); **left-to-right is reserved for the Kanban** | *"Could that be top-to-bottom? … for a big graph."* / *"separation of function."* |
| Filter by creator | a creator filter surfaces **that person's nodes** | *"search by creator… if I put in Sean, bring up Sean's nodes."* |
| Filter by node = subgraph | click/hover **any** node → find its subgraph, **bidirectional** (not just from questions) | *"filter by node AKA find subgraph… bidirectionally from any node."* |
| Hover / click | **hover emphasizes** connected nodes (greys the rest); **click reorients** the layout around the node | *"gray out all the nodes not connected… click to reorganize."* |

### 4b. Kanban gains the path
The Kanban currently hides *"the path through the graph."* Add **connection lines / selective highlighting** so that
selecting a card emphasizes its connected cards across columns — the graph structure, legible in the board.

> **Status of the v0.2 "Table is a facade" caveat:** the **Graph** tab now has a real spec (4a). The Table view
> remains the lower-priority facade (still acceptance-tracked); Graph is the one to build first per this session.

---

## 5. Mobile-first meeting surfaces · personas · dyslexia-friendly a11y

- **Mobile-first (Theme 8).** The PI persona **Jackie attends meetings with only a phone or iPad mini**, so the
  **meeting-prep (11) and glance (07) surfaces must have real mobile layouts** — single column, ≥44 px tap targets,
  the diff groups stacked. Desktop is the secondary case for these two screens. (An **iOS app** later unlocks
  *consented* notifications + TestFlight device whitelisting — deferred, but it's why mobile matters now.)
- **Personas doc (separate artifact).** Author `personas.md`: each persona grounded in a **real, reachable person**
  (Jackie first), with **entry point** and **experience maturity**. Out of scope for the mockups; tracked as a doc.
- **Dyslexia-friendly default (Theme 8).** Extend the §6 (v0.2) a11y bar with a **dyslexia-friendly** option:
  increased letter/word spacing, slightly heavier weight, generous line-height — derived from the **iOS HIG**. It
  composes with the existing serif + spacing toggles; it is **not** a new theme editor (Sean's "too much
  customization is bad" caveat still holds).

---

## 6. New atoms to add  *(extension block, bottom of `components.css`)*

| Class | Role | Section |
|---|---|---|
| `.agenda-tag` | "For next sync · ⟨date⟩" pill (replaces the nudge as the meeting signal) | §2 |
| `.due-chip` | owner-set "Next step by ⟨date⟩"; calm "overdue · Nd" on lapse | §3 |
| `.follow-btn` + `.follow-pill` | owner-granted "Follow" → "Kate follows" | §3 |
| `.diff-since` + `.diff-group` + `.diff-row` + change-verb chips (`.chg.advanced/.evidence/.comments/.idle`) | the "Since last sync" meeting-prep view | §2b |
| `.backlog` | owner's personal "my experiments" board | §3 |
| `.pref-dyslexia` (body mode) + `.a11y-opt` row | dyslexia-friendly spacing/weight | §5 |
| (Graph) `.gnode`, `.gedge`, `.g--emphasis`, `.g--dim` | top-to-bottom graph nodes/edges + hover-emphasis states | §4 |

**Removed:** `.nudge-btn` (and the PI-poke affordance). `.stale-flag` is **retired as an alert** and **re-homed**
as the calm `no update · Nd` row inside the diff view's folded group (§2b).

---

## 7. Updated acceptance criteria  *(delta to v0.2 §9)*

- **Supersede #10.** ~~"…stalled in-progress cards surface a nudge."~~ → **10′. Self-directed staleness.** The
  **owner** sets `next_step_by`; on lapse the item surfaces on the **owner's own backlog** and in the diff view —
  **never** as a PI-initiated push. No `.nudge-btn` exists. *(§1, §3)*
- **18. Meeting-anchored agenda.** Nodes can be marked **"For next sync"**; the meeting-prep view groups
  *on-the-agenda* first. *(§2)*
- **19. Opt-in follow.** A **student** can grant a PI **Follow** on a card; the PI then receives the **same** lapse
  signal — and cannot initiate one otherwise. *(§3)*
- **20. Diff view.** A **"Since last sync"** view lists what changed since a date, with stale nodes **folded**
  beneath, in **calm** (non-alarm) styling, and works on **mobile**. *(§2b, §5)*
- **21. Derived, not duplicated.** Deadline/agenda/stale are **node fields**; the backlog and diff are generated —
  no parallel to-do entry. `done`/`parked` nodes are **excluded** from staleness. *(§3)*
- **22. Real Graph view.** The Graph tab supports **top-to-bottom** layout, **filter-by-creator**, and
  **filter-by-node = subgraph** with hover-emphasis/click-reorient; the Kanban shows **connection paths**. *(§4)*

## 8. Mockup work implied

| Mockup | Action |
|---|---|
| `09-consortium-view.html` | **Remove** the `.stale-flag` + **Nudge** button. Add the **`.agenda-tag`** ("For next sync"), the owner-set **`.due-chip`**, and the **`.follow-btn`**. Reframe the PI affordance from *poke* to *follow / add-to-agenda (suggest)*. |
| *new* `11-meeting-prep.html` | The **"Since last sync" diff view**: on-the-agenda → what-changed → folded *not-changed*; calm styling; **mobile-first** layout. The literal answer to *"which nodes are slated for the next meeting?"* |
| `07-shared-dashboard.html` | (Later) real **Graph** interactions (top-to-bottom, creator filter, node→subgraph); **paths on the Kanban**; mobile glance layout. |
| `index.html` | Add the **11** card; note the nudge reversal. |
| `personas.md` (new doc) | Jackie-first personas with entry points + experience maturity. *(doc, not a mockup)* |

## 9. Open questions  *(carried into design)*

- **Notification channel of last resort.** A *followed* card that lapses — does the PI get an in-board badge only,
  or (opt-in) email/iOS push? Session wanted **no push by default**, allowed **consented** ones; channel unspecified.
- **Meeting date source** the agenda anchors to — shared consortium calendar field vs. per-collaborator.
- **`done`/`parked` semantics** — needs an explicit owner-set state so the diff never nags finished work
  (*"unless it's finished"*).
- **Mobile scope for v1** — responsive web now + iOS app later, or iOS sooner (it's what "unlocks notifications")?
- **Kanban paths without the noise** the graph view exists to tame.

---

*MIRA × Discourse Graphs · Sean → Kate **dashboard** · UX handoff delta · draft **v0.3** (supersedes v0.2 §4 + acceptance #10 where they differ; research layer [`ux-research-synthesis-2026-06-11-trainee.md`](./ux-research-synthesis-2026-06-11-trainee.md)) · from the Jun 11 2026 "Graph filtering ideas with Sean" session*
