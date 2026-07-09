# Push results to a shared web interface

*A MIRA user story — how one lab's result reaches the people who need it on a **shared web interface**, readable without any discourse-graph tooling: from a **collaborator's dashboard** to the **fully public** web.*

---

## ▶ See the mockups — live

The eleven screens are **interactive HTML mockups**, hosted on GitHub Pages. Open them in a browser — GitHub shows `.html` files as raw source, so use the links below (not the files in [`artifacts/mockups/`](./artifacts/mockups/)).

### 🔗 [**mira-science.github.io/inter-lab-user-story**](https://mira-science.github.io/inter-lab-user-story/) — the gallery index

**Central — Sean → Kate's dashboard ★**

| | Screen | |
|--:|---|---|
| **06** | [Share a selected subgraph ↗](https://mira-science.github.io/inter-lab-user-story/06-share-to-dashboard.html) | pick the `Evidence` bundle, set per-node visibility, push via KOI |
| **07** | [The shared dashboard ↗](https://mira-science.github.io/inter-lab-user-story/07-shared-dashboard.html) | the headline read-only web view — Graph · Kanban · Table |
| **08** | [Traverse & request back ↗](https://mira-science.github.io/inter-lab-user-story/08-traverse-and-request.html) | follow lineage to the method, hit gated data, send a `Request` |
| **09** | [Consortium view & recruiting ↗](https://mira-science.github.io/inter-lab-user-story/09-consortium-view.html) | one umbrella across many labs — public vs. password-protected |
| **10** | [The evidence card ↗](https://mira-science.github.io/inter-lab-user-story/10-evidence-card.html) | one result-first card with the method folded in — shown at four states |
| **11** | [Meeting prep ↗](https://mira-science.github.io/inter-lab-user-story/11-meeting-prep.html) | a mobile-first "since last sync" diff to prep for the next meeting |

<details>
<summary><b>Public extreme — QSI → the whole world (01–05)</b></summary>

| | Screen | |
|--:|---|---|
| **01** | [Publish to the public ↗](https://mira-science.github.io/inter-lab-user-story/01-publish.html) | select the bundle, pick destinations, set visibility |
| **02** | [The Evidence node, read cold ↗](https://mira-science.github.io/inter-lab-user-story/02-evidence-node.html) | summary up front, full lineage on demand |
| **03** | [Public web view · no DG app ↗](https://mira-science.github.io/inter-lab-user-story/03-public-web-view.html) | the result as a public web page — provenance + frozen citation |
| **04** | [Micropublication ↗](https://mira-science.github.io/inter-lab-user-story/04-micropublication.html) | several `Evidence` bundles compile into a Jupyter Book |
| **05** | [Public database & cite ↗](https://mira-science.github.io/inter-lab-user-story/05-public-database.html) | the published graph as a public dashboard; cite a frozen version |

</details>

*Served from [`artifacts/mockups/`](./artifacts/mockups/) via GitHub Pages ([`pages.yml`](./.github/workflows/pages.yml)); edits to those files redeploy automatically. PNG previews of every screen are inlined in [`artifacts/README.md`](./artifacts/README.md) for reading on GitHub itself.*

---

## The central use case — Sean → Kate's dashboard

Sean, a PhD student in the **Vogel Lab**, has a result living as markdown discourse-graph nodes in his Obsidian vault: in perturbed samples, two cluster components are recruited in a fixed order — **Component A climbs before Component B** as clusters assemble. He wants **Kate's lab** — a collaborating PI and her students — to **see it, trust it, and dig into it on their own time.**

Not by installing a discourse-graph app, not on a scheduled Zoom call, and **without** shipping his 80 GB of raw images or exposing his private notes. He pushes a **selected subgraph** through KOI to a **read-only shared web dashboard** — **public** for the lab website, or **password-protected** for the consortium. Kate (the PI) **glances at the summary plot and trusts it**; her **grad student traverses back to the segmentation threshold** and pulls the curated CSV; and when they want more, they **send a `Request` back** for a follow-up experiment. *Two currents: results out, requests back.*

## The public extreme — QSI → the whole world

Take the same machinery and open it all the way up. Brian, at the **Quantum Sensing Institute**, publishes a MFX-2 magnetic-field-effect result to a **public repository or database** so that **anyone** — a reviewer, a future collaborator, or his own future self, with **no relationship and no shared tooling** — can read it cold at a URL, follow the pointers, **request access** to what's gated, and **cite the exact version**. One direction: producer → the public.

**One spine, two worked examples.** Both push a result to a **shared web interface** — trustworthy, addressable at a URL, readable with no DG app — sliding from a *consortium dashboard* (Sean → Kate, gated, with a request-back) to the *fully public* (Brian → world). Same machinery (KOI → MIRA JSON-LD → web rendering), same rules.

## What's in here

| File | What it is |
|---|---|
| [`AGENTS.md`](./AGENTS.md) | **Start here.** The shared brief + spec: scope, actors, data model, twelve normative rules, the two lifecycles (share-to-dashboard + publish), acceptance criteria, and open questions. Written so coding agents in different labs can build interoperable pieces from one source of truth. |
| [`artifacts/`](./artifacts/) | **The fleshed-out UX.** The **central** [`ux-user-story-dashboard.md`](./artifacts/ux-user-story-dashboard.md) (Sean → Kate) and its context pack [`_grounding-dashboard.md`](./artifacts/_grounding-dashboard.md); the retained [`ux-user-story.md`](./artifacts/ux-user-story.md) (QSI) and [`_grounding.md`](./artifacts/_grounding.md); and **eleven** interactive HTML [`mockups/`](./artifacts/mockups/) ([**view live ↗**](https://mira-science.github.io/inter-lab-user-story/)) — **06–11** the dashboard (central), **01–05** the public extreme. See [`artifacts/README.md`](./artifacts/README.md). |
| [`./low-context-user-story.png`](./low-context-user-story.png) | The user-story canvas — **Sean's notebook → Kate's lab → a shared web interface** ("KOI DG nodes to dashboard/kanban"), carrying a `Request for study` node — across location / format / feel / features / schema / tasks. |

## The shape of the data

We use the **standard MIRA grammar** — the destination changes, the grammar does not. Six node types carry this work:

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

Draft v0.1. The **central** use case is the **Sean → Kate dashboard** — pushing a selected subgraph to a read-only shared web interface (public or consortium-gated) with a request-back current, grounded in the Vogel × Kate cluster sessions and the user-story canvas. The **retained** public extreme is **QSI MFX-2 → the world** (Obsidian → public repo/DB; the bronze tier of the shared-context ladder). Both worked examples are real; the data is **unpublished** (mockup figures are labelled *illustrative · unpublished — workshop prototype*).

---

🔬 [mira.science](https://www.mira.science) · 🧠 [discoursegraphs.com](https://discoursegraphs.com) · 📐 [MIRA-science/schema](https://github.com/MIRA-science/schema) · ⚛️ [quantumsensing.eco](https://www.quantumsensing.eco)
