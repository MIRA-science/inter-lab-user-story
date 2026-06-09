# Push results to a shared web interface

*A MIRA user story — how one lab's result reaches the people who need it on a **shared web interface**, readable without any discourse-graph tooling: from a **collaborator's dashboard** to the **fully public** web. The shared-web-surface end of the [inter-graph exchange](lab-to-lab/).*

---

## The central use case — Sean → Kate's dashboard

Sean, a PhD student in the **Vogel Lab**, has a result living as markdown discourse-graph nodes in his Obsidian vault: in stressed HeLa cells, two stress-granule proteins are recruited in a fixed order — **G3BP1 climbs before PABP1** as granules assemble. He wants **Kate's lab** — a collaborating PI and her students — to **see it, trust it, and dig into it on their own time.**

Not by installing a discourse-graph app, not on a scheduled Zoom call, and **without** shipping his 80 GB of raw images or exposing his private notes. He pushes a **selected subgraph** through KOI to a **read-only shared web dashboard** — **public** for the lab website, or **password-protected** for the consortium. Kate (the PI) **glances at the summary plot and trusts it**; her **grad student traverses back to the segmentation threshold** and pulls the curated CSV; and when they want more, they **send a `Request` back** for a follow-up experiment. *Two currents: results out, requests back.*

> This is **distinct from the [lab-to-lab push/pull story](lab-to-lab/)** (now a subfolder of this repo), which stars the *same* Sean → Kate cast — but there the result is **imported into Kate's own graph**. **Here the surface is a read-only dashboard** Kate's lab simply *views in a browser* — "legible to people with zero discourse-graphs background."

## The public extreme — QBI → the whole world

Take the same machinery and open it all the way up. Brian, at the **Quantum Biology Institute**, publishes a MagLOV2 magnetic-field-effect result to a **public repository or database** so that **anyone** — a reviewer, a future collaborator, or his own future self, with **no relationship and no shared tooling** — can read it cold at a URL, follow the pointers, **request access** to what's gated, and **cite the exact version**. One direction: producer → the public.

**One spine, two worked examples.** Both push a result to a **shared web interface** — trustworthy, addressable at a URL, readable with no DG app — sliding from a *consortium dashboard* (Sean → Kate, gated, with a request-back) to the *fully public* (Brian → world). Same machinery (KOI → MIRA JSON-LD → web rendering), same rules.

## What's in here

| File | What it is |
|---|---|
| [`AGENTS.md`](./AGENTS.md) | **Start here.** The shared brief + spec: scope, actors, data model, twelve normative rules, the two lifecycles (share-to-dashboard + publish), acceptance criteria, and open questions. Written so coding agents in different labs can build interoperable pieces from one source of truth. |
| [`artifacts/`](./artifacts/) | **The fleshed-out UX.** The **central** [`ux-user-story-dashboard.md`](./artifacts/ux-user-story-dashboard.md) (Sean → Kate) and its context pack [`_grounding-dashboard.md`](./artifacts/_grounding-dashboard.md); the retained [`ux-user-story.md`](./artifacts/ux-user-story.md) (QBI) and [`_grounding.md`](./artifacts/_grounding.md); and **nine** interactive HTML [`mockups/`](./artifacts/mockups/) — **06–09** the dashboard (central), **01–05** the public extreme. See [`artifacts/README.md`](./artifacts/README.md). |
| [`./low-context-user-story.png`](./low-context-user-story.png) | The user-story canvas — **Sean's notebook → Kate's lab → a shared web interface** ("KOI DG nodes to dashboard/kanban"), carrying a `Request for study` node — across location / format / feel / features / schema / tasks. |

## The shape of the data

We use the **same MIRA grammar as the [inter-lab story](lab-to-lab/)** — the destination changes, the grammar does not. Six node types carry this work:

```
Question ←addresses← Claim ←supports← Evidence ←grounds← Study →follows→ Protocol
                                                              ↑
                       Request →request_for→ a follow-up Study / Analysis   (Kate's lab → Sean)
```

`Evidence` is the publishable/shareable hub — the key figure, the observation, the pointers. `Study` and `Protocol` are the activities behind it. At the **dashboard tier the `Request` node is first-class** (the reverse current); at the fully-public tier it is peripheral. The canonical, machine-readable definitions live in **<https://github.com/MIRA-science/schema>**.

> The pilot sketched a richer reader-facing ladder (`Evidence → Analysis → ELN → Data`). For v1 we **keep the canonical `Evidence`/`Study`/`Protocol`** and defer that mapping — see [`AGENTS.md` §10](./AGENTS.md#10-open-questions--not-decided).

## The principles, in one breath

- **Pointers, not payloads** — the analysis is a git+commit pointer, the data a curated-CSV pointer, the 80 GB raw stacks a *request-access* pointer; never embedded.
- **A shared web interface** — addressable at a **URL**, readable as a **web page / dashboard** with no DG app (markdown → HTML is fine for v0). The dashboard is **read-only**: a viewing/discovery surface, not an authoring tool.
- **Permissions, even when shared** — **public OR password-protected (consortium)**; a node can gate its raw data behind request-access; start closed; never leak an un-shared node.
- **Summary up front, lineage on demand** — the PI glances and trusts; the grad student traverses to the method (the segmentation threshold) that actually moves the result.
- **Two currents at the dashboard tier** — results flow out *and* a `Request` flows back; the fully-public tier has only the one.
- **Trust through provenance** — the page/card shows who shared it, which lab, and the terms.
- **Freeze the cited version** — a citer gets the exact version they saw, immutably (the public tier).
- **Specs instead of papers** — publish/share bundles as you go; compile them into a narrative later; the bundles stay independently addressable.

The normative versions (with the MUSTs and SHOULDs) live in [`AGENTS.md` §5](./AGENTS.md#5-rules--constraints-normative).

## Building from this

If you're an engineer or a coding agent picking up a piece of this: **read [`AGENTS.md`](./AGENTS.md) first.** It is the source of truth. Treat its §5 rules as acceptance constraints (cite them in PRs), and treat its §10 open questions as genuinely open — don't implement them as if they were settled.

## Status

Draft v0.1. The **central** use case is the **Sean → Kate dashboard** — pushing a selected subgraph to a read-only shared web interface (public or consortium-gated) with a request-back current, grounded in the Vogel × Kate stress-granule sessions and the user-story canvas. The **retained** public extreme is **QBI MagLOV2 → the world** (Obsidian → public repo/DB; the bronze tier of the shared-context ladder). Built as the shared-web-surface counterpart to the [`lab-to-lab/`](lab-to-lab/) push/pull story (now a subfolder of this repo). Both worked examples are real; the data is **unpublished** (mockup figures are labelled *illustrative · unpublished — workshop prototype*).

---

🔬 [mira.science](https://www.mira.science) · 🧠 [discoursegraphs.com](https://discoursegraphs.com) · 📐 [MIRA-science/schema](https://github.com/MIRA-science/schema) · ⚛️ [quantumbiology.eco](https://www.quantumbiology.eco)
