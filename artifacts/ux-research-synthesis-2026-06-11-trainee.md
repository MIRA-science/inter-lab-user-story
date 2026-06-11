# Research synthesis — trainee perspective on the dashboard (the "nudge" rejection & what replaces it)

**Method:** Post-workshop debrief / contextual group interview (transcript) · **Participants:** 2 trainee-side voices (Sean — grad student; an experimentalist) within a 5-person session · **Date:** Jun 11, 2026 · **Source:** [`MIRA-transcripts/Graph filtering ideas with Sean - sectioned.md`](../../MIRA-transcripts/Graph%20filtering%20ideas%20with%20Sean%20-%20sectioned.md) (local-only) · **Synthesis:** Claude (`/research-synthesis`)

> **Read this as the user-research layer under [`ux-spec-update-2026-06-11-trainee-feedback.md`](./ux-spec-update-2026-06-11-trainee-feedback.md).** It separates **observation** (what was said) from **interpretation** (what it implies), keeps the verbatim quotes so the spec changes are auditable, and ranks opportunities. The headline: **the PI "nudge" we built into mockup 09 was rejected to its face by the trainee it targets** — and the session handed us a concrete, gentler replacement.
>
> **N is small and role-skewed.** Real product signal comes from two trainee-side participants (Sean, Speaker D; an experimentalist, Speaker E); the others (A·Matthew, B·product/UX, C·Martin) are the building team. Treat the trainee quotes as **strong directional signal from the exact target user**, not as a survey. Speaker map and caveats per the transcript's own legend.

---

## Executive summary

The most-built-up new feature from the Jun 10 session — a **PI "nudge"** that pings a student when an experiment goes stale — is the one the trainee **least** wants. He rejected it directly ("*I don't like that one*") and explained why: it breaks deep-focus working modes, it feels like social-media/email pressure foreign to a *scientific* tool, and it inverts the supervisor relationship into something vertical and surveilling. Crucially, the participants did **not** reject the underlying need (don't let experiments silently rot) — they re-specified **how** to meet it: a **self-directed staleness signal** the student sees on *their own* board, an **opt-in "follow card"** by which the student (not the PI) invites supervision, and — the cleanest idea — surfacing stale/active work **anchored to the next meeting** ("*which of these are slated for us to talk about in the meeting?*") rather than as a push notification. A secondary but concrete thread re-specified the **Graph view** (top-to-bottom, filter-by-creator, filter-by-node-as-subgraph) and named the **primary PI persona** (Jackie — attends meetings on a phone/iPad mini) and a **dyslexia-friendly** accessibility bar.

---

## Key themes

### Theme 1 — The PI→student "nudge" is rejected: it breaks focus and inverts agency
**Prevalence:** 2 of 2 trainee-side voices, unprompted and emphatic.
**Summary:** A supervisor-initiated ping ("no result in 18 days — nudge the student?") is the wrong mechanism. It collides with the deep-focus modes scientists deliberately enter, reads as consumer-app pressure, and makes the supervisor relationship feel top-down.
**Supporting evidence (observation):**
- Sean, on seeing the feature in the mockup: *"I don't like that one."*
- *"Sometimes I like chuck my phone in a drawer and I have like everything on do not disturb and I'm just using like my Obsidian and I just want to like lock in — and this is something that I would really hate to receive if I'm in that mode."* — Sean
- *"It feels a little bit out of scope to add this nudge function … I know that people hate emails."* — Sean
- *"It's not like a social media. That's not what we're trying to make. We're making … scientific sharing."* — Sean
- *"It would be good to have something like 'you have not updated this node beyond the time that you thought the next step would be' … I don't think it needs to involve a trigger from the supervisor."* — experimentalist (E)
**Interpretation:** Remove the PI→student nudge as a default mechanism. The need it served (surfacing stalled work) is real; the **delivery** (a push from above) is what fails. (Directly supersedes Jun-10 spec §4's `.nudge-btn` and acceptance #10's "surface a nudge.")

### Theme 2 — Replace the ping with a *meeting-anchored* signal: "what's slated for the next sync"
**Prevalence:** Proposed by product/UX, endorsed by Sean as the right framing.
**Summary:** The natural checkpoint is the **regular meeting**, not a notification. Nodes should carry a calm, in-place indication that they're **on the agenda** / expected to be updated **by** the next sync — so the student can prepare, not be startled.
**Supporting evidence:**
- *"There's a natural checkpoint when you have your … Zoom meetings."* — product/UX (B)
- *"There should be a feature that … bring[s] nodes to attention so that they can be discussed during our normal meetings."* — Sean
- *"Which of these are slated for us to talk about in the meeting?"* — product/UX (B), which Sean affirmed
**Interpretation:** Introduce an **agenda/"for next sync" state** on studies and a meeting-anchored grouping. This is the literal replacement for the nudge: *the system shows what we'll discuss; it does not poke you.*

### Theme 3 — Self-directed deadlines + an *opt-in, student-initiated* supervisor "follow card"
**Prevalence:** 2 of 2; the experimentalist specified the mechanism, Sean affirmed it.
**Summary:** The student sets their own "expected next step by" date. When it passes without an update, the item surfaces **on the student's own backlog** — psychologically safe because it's self-imposed. Accountability with a supervisor is **opt-in and student-initiated**: a "follow card" the student attaches, which lets the PI receive the *same* signal when the self-set date passes.
**Supporting evidence:**
- *"If I was able to create my own dashboard … showing my personal experiments, I can see the forgotten stepchild, the middle-child experiment."* — Sean
- *"[In Notion] I have multiple dates … seeing it appear in a list of backlog … psychologically feels safer … than getting a ping from your supervisor saying 'hey, are you done with this yet?'"* — experimentalist (E)
- *"You could … add a follow card. … you would assign your supervisor to follow this card. 'I'm setting myself this deadline and I want Matt to know.' So then Matt will also get the notification when the deadline I set has passed and this card has not been updated."* — experimentalist (E)
- *"The student having the agency to be accountable … and the supervisor being involved … It has to be a bi-directional thing, horizontal … if what you need is accountability, you can request it and tune it as much as you need."* — experimentalist (E)
**Interpretation:** Model = **student-owned deadline → student-owned backlog → optional follow**. The PI never initiates the watch; the student grants it. This adapts to "the diverse learning and working styles of your students" (E) instead of imposing one.

### Theme 4 — The meeting-prep **diff view**: "what changed since [date]," with stale work *folded* beneath
**Prevalence:** Sean (primary), experimentalist (affirm).
**Summary:** The surface that makes meetings work is a **diff since a date**: which projects advanced, which got new results. Staleness belongs **here**, as a *folded, secondary* section — not as an alert.
**Supporting evidence:**
- *"Maybe a diff — like what has been updated since this date?"* — Sean
- *"A summary of … this project advanced, this project advanced, this project had a new result attached to it."* — Sean
- *"In that page, maybe there could also be like a folded … section: 'these ones haven't been changed,' in case there's something that's been sitting for a while … particularly useful for looking at updates and planning for meetings."* — Sean
**Interpretation:** Build a **"Since last sync" diff view** as the meeting-prep home; put the stale-node list there, collapsed. This re-homes the staleness signal from a push (Theme 1) into a pull surface the user opens when *they* are preparing.

### Theme 5 — Reduce duplicated record-keeping; derive obligations from the nodes
**Prevalence:** 2 of 2.
**Summary:** Trainees already run task-sweeping systems (Obsidian **Obligator**; Notion **study cards** with date spans). The dashboard must **derive** the backlog/obligations from the existing nodes, not make them re-enter it.
**Supporting evidence:**
- *"I have this plugin in Obsidian … called Obligator … it procedurally creates a note with the date and copies … all [the unchecked tasks] over if they're not checked off yet."* — Sean
- *"I am kind of duplicating my reporting efforts — in the node and then in my Obligator page."* — Sean
- *"It is very important to reduce the amount of effort and time spent trying to keep up with everything … when it occupie[s] too much space and time, it's too much."* — experimentalist (E)
**Interpretation:** The "expected next step" date and the staleness/agenda state should be **fields on the node**, so the backlog and the diff view are generated, not maintained. Echo the Obligator/Notion mental model the users already trust.

### Theme 6 — Layout is context-dependent; presets are audience-shaped
**Prevalence:** Sean (explicit), aligns with Jun-10 audience presets.
**Summary:** The "right" layout depends on *who you're presenting to*. For the Kate sync: **top-to-bottom, question → claim → evidence → protocol**, leading with studies. For a funder/public surface: **requests at the top**, trickling down to experiments and progress.
**Supporting evidence:**
- *"If I was presenting it to Jackie, I would want it more temporally laid out … it's definitely context-dependent."* — Sean
- *"We can quickly put the question at the top, the claim … you want it top-to-bottom … probably studies, to keep focused, and then the claim and the evidence that supports the claim … then if we need to, go deeper to the protocol."* — Sean
- *"For a funder dashboard … prioritize requests to see where the gaps in the literature are … puts all the requests at the top, [then] trickle down."* — Sean
**Interpretation:** Confirms and sharpens the Jun-10 audience presets; adds a concrete **default ordering per preset**.

### Theme 7 — Graph view: top-to-bottom, filter-by-creator, filter-by-node = find-subgraph
**Prevalence:** Whole-room design discussion (Section 1).
**Summary:** The Graph view (currently a facade) gets a concrete interaction model: **filter by creator** to surface a person's nodes; **filter by node = find subgraph**, bidirectional from *any* node (not just questions); **hover to emphasize** connected nodes, **click to reorient** the layout around the selected node. Orientation is **top-to-bottom** (questions at top) — left-to-right is reserved for the **Kanban**.
**Supporting evidence:**
- *"You should be able to search by creator … if I put in Sean here, ideally it should bring up Sean's nodes, and if I click a question, zoom into that, orient around it."* — Matthew (A) / product (B)
- *"Gray out all the nodes that are not connected in the subgraph … by hover it would emphasize the other nodes … ideally you'd be able to reorganize them."* — product (B)
- *"Could that be top-to-bottom? Because scrolling might be [better] for a big graph."* — Martin (C); *"separation of function — the graph is naturally questions at the top, and Kanban for left-to-right."* — product (B)
- *"Filter by node AKA find subgraph"* … *"it should work bidirectionally from any node."* — Matthew (A)
- *"The thing that's missing from this Kanban right now is the path through the graph."* — product (B)
**Interpretation:** Spec a real Graph view (top-to-bottom; creator + node filters; hover/click); and add **connection paths** to the Kanban so the graph structure is legible there too. (Relieves the Jun-10 "Table is a facade" caveat by giving the *Graph* tab teeth.)

### Theme 8 — Personas grounded in a real person; Jackie is mobile-only; dyslexia-friendly by default
**Prevalence:** Matthew + product/UX; Sean supplies the persona detail.
**Summary:** Start each persona from a **real, reachable person**, then generalize. The **primary PI persona is Jackie** (Sean's PI), who **shows up to meetings with only a phone or iPad mini** — so the meeting surfaces must be **mobile-first**. An **iOS app** (vs. web) unlocks notifications and TestFlight device whitelisting. Accessibility: point Claude at the **iOS Human Interface Guidelines** for **dyslexia-friendly defaults**.
**Supporting evidence:**
- *"The only thing she shows up with to the meetings is a phone or … an iPad mini."* — Sean / Matthew
- *"Start with … Jackie is the person we are first trying to solve for, then Jackie turns into a persona … always grounding it in somebody we can reach."* — Matthew (A)
- *"This one in particular should be dyslexia-friendly."* — product/UX (B); *"point … Claude … at the human design manual from iOS … that'll probably handle a lot of accessibility by default."* — Matthew/Sean
**Interpretation:** (1) Author a **personas doc** (entry points + experience maturity), Jackie first. (2) Treat the **meeting-prep + glance surfaces as mobile layouts**, not just desktop. (3) Extend the existing a11y bar (serif/spacing) with a **dyslexia-friendly default** (spacing, weight, letter-spacing per the HIG).

---

## Insights → Opportunities

| Insight (observation) | Opportunity | Impact | Effort |
|---|---|---|---|
| The PI nudge is rejected by the target user (Theme 1) | **Remove `.nudge-btn`**; never let the PI initiate a poke | High | Low |
| The meeting is the real checkpoint (Theme 2) | **"On the agenda / for next sync" node state** + meeting-anchored grouping | High | Med |
| Self-set deadlines feel safe; PI pings don't (Theme 3) | **"Expected next step by" date** on a node → student's own backlog; **opt-in "Follow card"** for the PI | High | Med |
| Meetings need a "what changed" view (Theme 4) | **"Since last sync" diff view**; stale nodes **folded** beneath it | High | Med |
| Trainees already sweep tasks elsewhere & resent duplication (Theme 5) | Make deadline/agenda/stale **node fields**; **derive** the backlog (Obligator/Notion model) | Med | Med |
| Layout is audience-dependent (Theme 6) | **Default ordering per preset** (Kate: studies→claim→evidence→protocol; funder: requests-first) | Med | Low |
| Graph view is an empty tab (Theme 7) | Real **top-to-bottom Graph**: filter-by-creator, filter-by-node=subgraph, hover-emphasize/click-reorient; **paths on the Kanban** | Med | High |
| PI attends on a phone/iPad mini (Theme 8) | **Mobile-first** meeting/glance layouts; later an **iOS app** for (consented) notifications | High | High |
| Dyslexia named explicitly (Theme 8) | **Dyslexia-friendly toggle/default** extending the serif/spacing a11y bar | Med | Low |

---

## User segments identified

| Segment | Characteristics | Needs | Rough size |
|---|---|---|---|
| **Focus-protective trainee (Sean)** | Power user (Obsidian + Obligator); enters do-not-disturb "lock-in" modes; resents duplicated reporting; wants agency | Self-directed staleness on *their own* board; no top-down pings; derive obligations from nodes | The producer — 1 of the core cast, but represents many trainees |
| **Self-organizing experimentalist (E)** | Notion study-cards with date spans; values psychological safety & bidirectional accountability; mentors students | Opt-in follow/accountability she controls; low record-keeping overhead | A second producer archetype; also a willing feedback-giver |
| **Mobile, glance-only PI (Jackie/Kate)** | Attends meetings with phone/iPad mini; wants the big picture + what to discuss; trusts the summary | Mobile-first meeting-prep & glance; "what's on the agenda"; async comments/requests, not live pings | The consumer persona (primary) |

---

## Recommendations (ranked)

1. **Kill the PI nudge; ship the meeting-anchored + self-directed model instead.** Replace mockup 09's `.nudge-btn`/stale-flag with: an **"Expected next step by" date** the *student* sets, a **"For next sync"/agenda** state, and an **opt-in "Follow card."** Staleness becomes self-surfaced, never a poke. *(Themes 1–3; supersedes Jun-10 §4 + acceptance #10.)*
2. **Build a "Since last sync" diff view as the meeting-prep home.** "What changed since [date]" (advanced / new evidence), with a **folded stale-node section** beneath. This is where staleness lives, as a pull surface. *(Theme 4.)*
3. **Make deadline / agenda / stale into node fields and *derive* the backlog.** Don't add a parallel to-do system; honor the Obligator/Notion model so trainees stop duplicating reporting. *(Theme 5.)*
4. **Give the Graph view a real interaction model** (top-to-bottom; filter-by-creator; filter-by-node=find-subgraph, bidirectional; hover-emphasize/click-reorient) and **draw connection paths on the Kanban.** *(Theme 7; also retires the "facade" caveat for the Graph tab.)*
5. **Treat meeting-prep & glance as mobile-first** (Jackie on a phone/iPad mini), and **author a personas doc** (Jackie first; entry points + experience maturity). *(Theme 8.)*
6. **Add a dyslexia-friendly default** to the existing serif/spacing a11y bar (iOS HIG-derived spacing/weight). *(Theme 8.)*

---

## Questions for further research
- **Notification of last resort:** if a self-set deadline passes and a *followed* card goes stale, what does the PI actually receive, and *where* (in-board badge vs. email vs. iOS push)? The session wanted no pings *by default* but allowed consented ones — the exact channel is unspecified.
- **Who sets the meeting date / cadence** that "for next sync" anchors to — is it a shared consortium calendar field, or per-collaborator?
- **"Finished" vs "stale":** Sean noted stale-flagging should exclude finished nodes ("*unless it's finished*") — need an explicit done/parked state so the diff view doesn't nag completed work.
- **Mobile scope for v1:** is the mobile target a responsive web layout now and an iOS app later, or is the iOS app in-scope sooner (it's the thing that "unlocks notifications")?
- **Graph-on-Kanban paths:** how to render connection paths in the Kanban without recreating the visual noise the graph view is meant to tame.

## Methodology notes
Single post-workshop debrief transcript (Granola summary + diarized A–E transcript), role mapping inferred from the summary's next-steps owners and self-identifications — **best-effort, not verified.** ASR garbles persist (KOI/MIRA/Stencila/Obligator etc.); quotes lightly cleaned for readability, meaning preserved. Only **two** participants are in the target-user role (Sean; the experimentalist), so prevalence counts are small — but they are the **exact** users the rejected feature targeted, and they converged independently on the same replacement, which is strong directional signal. Team-side speakers (Matthew, product/UX, Martin) are treated as design input, not user evidence. Infrastructure and collaboration-model threads (KOI publish endpoint, validator loop, status page, 90-min syncs, pixel-art space) are captured in the transcript but **out of scope** for this dashboard synthesis.
