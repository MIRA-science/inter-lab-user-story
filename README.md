# Inter-lab exchange of results & requests

*A MIRA user story — how one lab's discovery travels into another lab's graph, and how a request for new science travels back.*

---

Sean has a result. It lives as a handful of markdown nodes in the discourse graph on his laptop: two proteins inside a stress granule, imaged as the granule forms, one lighting up and the other catching up a beat later. Kate's lab — visiting for the week — wants it. Not just the picture: the **experiment** behind it, the **protocol** behind that, and a thread back to the 80-gigabyte microscopy stack in case a student ever needs to re-run the numbers.

And Kate has something to send the other way: *"we need an experiment that doesn't exist yet."*

Today that exchange happens over Dropbox links, emailed Python scripts, and a Zoom call where someone walks you through a slide. **This repository is the user story for making it first-class** — so a result, with just enough of its lineage, can move from one lab's graph into another's, under permissions the producer controls, and so a request can open a thread that a collaborator picks up and answers.

## The exchange, in two directions

- **Results** flow producer → consumer. A *small, self-describing subgraph* — `Evidence` + the `Study` that grounds it + the `Protocol` it followed — carrying **pointers** to the data, code, and a walkthrough video, never the raw data itself.
- **Requests** flow consumer → producer. A `Request` node — *"this experiment/analysis is needed"* — that another lab can **claim**, fulfil, and point a fresh result back at.

## What's in here

| File | What it is |
|---|---|
| [`AGENTS.md`](./AGENTS.md) | **Start here.** The shared brief + spec: scope, actors, data model, normative rules, prototype acceptance criteria, and open questions. Written so coding agents in different labs can build interoperable pieces from one source of truth. |
| [`user-story-tldraw.png`](./user-story-tldraw.png) | The user-story canvas — source → transport → destination, across location / format / feel / features / schema / tasks. |
| [`Schema diagram.png`](./Schema%20diagram.png) | The MIRA node grammar (Question · Claim · Evidence · Study · Protocol · Request), worked through the discovery of the structure of DNA. |

## The shape of the data

Six node types and the edges between them — the MIRA schema, which extends the [Discourse Graphs](https://discoursegraphs.com) core and is being adopted back into it:

```
Question  ←addresses←  Claim  ←supports/opposes←  Evidence  ←grounds←  Study  →follows→  Protocol
                          ↑                                                ↑
                          └────────── Request ──request_target────────────┘ (request_for →)
```

`Evidence` is a single type that covers both your own observations and the literature's. `Study` and `Protocol` are activities. `Request` is the collaboration primitive — a placeholder for science not yet done. The canonical, machine-readable definitions live in **<https://github.com/MIRA-science/schema>**.

## The principles, in one breath

- **Pointers, not payloads** — graphs are great at *which data produced which conclusion*, terrible at *storing data*.
- **Peer-to-peer** — each lab mediates its own bilateral sharing; no one sits in the middle holding the keys.
- **Permissions first** — it's far easier to open a closed system than to close an open one.
- **One `Evidence`** — don't split "result" from "evidence."
- **Send a small subgraph** — enough to stand on its own and trace back, no more.

The normative versions (with the MUSTs and SHOULDs) live in [`AGENTS.md` §5](./AGENTS.md#5-rules--constraints-normative).

## Building from this

If you're an engineer or a coding agent picking up a piece of this: **read [`AGENTS.md`](./AGENTS.md) first.** It is the source of truth. Treat its §5 rules as acceptance constraints (cite them in PRs), and treat its §10 open questions as genuinely open — don't implement them as if they were settled.

## Status

Draft v0.1, born at the **MIRA workshop**, *"Collaboration between labs experiment"* working session, 2026-06-08. The canonical test case is real, and the prototype goal is concrete: share Sean's stress-granule subgraph into a second lab's graph over [KOI-net](https://github.com/BlockScience/koi), and open a request that comes back answered.

---

🔬 [mira.science](https://www.mira.science) · 🧠 [discoursegraphs.com](https://discoursegraphs.com) · 📐 [MIRA-science/schema](https://github.com/MIRA-science/schema)
