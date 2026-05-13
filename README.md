# LLM Wiki Vault

A personal knowledge base for deep academic research, built on the idea that LLMs should **maintain** knowledge, not just retrieve it. Heavily inspired by [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

> [中文版请见 README_CN.md](README_CN.md)

## The Core Idea

Most people's experience with LLMs and documents looks like RAG: you upload files, the LLM retrieves chunks, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation. Ask a subtle question that requires synthesizing five papers, and the LLM has to find and piece together fragments every time.

The idea here is different. Instead of just retrieving from raw documents at query time, the LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files that sits between you and the raw sources. When you add a new paper, the LLM doesn't just index it. It reads it, extracts key information, and integrates it into the existing wiki — updating entity pages, revising concept summaries, noting contradictions, strengthening the evolving synthesis. The knowledge is compiled once and then *kept current*, not re-derived on every query.

This is the key difference: **the wiki is a persistent, compounding artifact.** The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read. The wiki keeps getting richer with every source you add and every question you ask.

You never (or rarely) write the wiki yourself — the LLM writes and maintains all of it via [Claude Skills](.claude/skills/). You're in charge of sourcing, exploration, and asking the right questions. The LLM does all the grunt work — summarizing, cross-referencing, filing, and bookkeeping. In practice, I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time — following links, checking the graph view, reading updated pages. **Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.**

This particular vault is focused on autonomous driving, inverse reinforcement learning (IRL), and motion planning, but the pattern applies to any domain where you're accumulating knowledge over time.

## Architecture

There are three layers:

### Raw Sources

Your curated collection of source documents — PDF papers, web articles, video transcripts, meeting notes. These are immutable. The LLM reads from them but never modifies them. This is your source of truth.

```
raw/
├── 01-articles/        # Web articles and markdown clips
├── 02-papers/          # PDF papers and academic literature
├── 03-transcripts/     # Video transcripts
├── 04-meeting_notes/   # Meeting notes
└── 09-archive/         # Processed and archived files (never read)
```

### The Wiki

A directory of LLM-generated markdown files. Summaries, entity pages, concept pages, syntheses. The LLM owns this layer entirely. It creates pages, updates them when new sources arrive, maintains cross-references, and keeps everything consistent. You read it; the LLM writes it.

```
wiki/
├── concepts/           # Abstract concepts, frameworks, methods
├── entities/           # People, companies, tools, products
├── sources/            # Source summaries with extracted metadata
├── syntheses/          # L3 deep reading notes
├── index.md            # Global registry of all pages
└── log.md              # Operation log (append-only)
```

### The Schema

[`CLAUDE.md`](CLAUDE.md) tells the LLM how the wiki is structured, what the conventions are, and what workflows to follow. This is the key configuration file — it's what makes the LLM a disciplined wiki maintainer rather than a generic chatbot. You and the LLM co-evolve this over time.

## Operations

### Ingest

Drop a new source into `raw/` and tell the LLM to process it:

```
/ingest
```

Or ingest a specific file:

```
/ingest raw/02-papers/TreeIRL.pdf
```

The LLM reads the source, discusses key takeaways, writes a summary in `wiki/sources/`, updates relevant entity and concept pages across the wiki, and appends an entry to `wiki/log.md`. A single source might touch 10-15 wiki pages. After confirmation, the source is moved to `raw/09-archive/`.

I prefer to ingest sources one at a time and stay involved — I read the summaries, check the updates, and guide the LLM on what to emphasize.

### Query

Ask questions against the wiki. The LLM searches relevant pages, reads them, and synthesizes an answer with citations:

```
/query What are the key differences between MaxEnt IRL and SMIRL?
```

Good answers can be filed back into the wiki as new pages — a comparison, an analysis, a connection you discovered. This way your explorations compound in the knowledge base just like ingested sources do.

### Lint

Periodically health-check the wiki:

```
/lint
```

The LLM scans for dead links, orphan pages, unregistered pages, and knowledge conflicts. This keeps the wiki healthy as it grows.

## Claude Skills

Skills are reusable prompts that teach the LLM how to perform specific tasks. This vault includes:

| Category | Skill | What it does |
|----------|-------|--------------|
| Core | **ingest** | Compile raw sources into the wiki, extract entities/concepts, archive |
| Core | **query** | Answer questions by reading wiki pages and synthesizing responses |
| Core | **lint** | Health-check the wiki for dead links, orphans, conflicts |
| Core | **generate-mocs** | Regenerate index pages and Maps of Content |
| Reading | **scholar-skill** | Structured paper reading with L1/L2/L3 depth standards |
| Viz | **mermaid-visualizer** | Generate Mermaid diagrams from text |
| Viz | **excalidraw-diagram** | Generate Excalidraw diagrams (Obsidian/Standard/Animated) |
| Viz | **obsidian-canvas-creator** | Generate Obsidian Canvas files (MindMap or freeform) |
| Obsidian | **obsidian-cli** | Command-line interaction with a running Obsidian instance |
| Obsidian | **obsidian-markdown** | Obsidian Flavored Markdown syntax guidance |
| Obsidian | **obsidian-bases** | Database-like views with filters, formulas, summaries |
| Utility | **defuddle** | Extract clean markdown from web pages |
| Pipeline | **daily-papers** | End-to-end daily arXiv paper fetching, review, and note generation |

Trigger a skill with `/skillname` or natural language (e.g., "ingest this paper", "create a flowchart").

## Indexing and Logging

Two special files help navigate the wiki as it grows:

**[`wiki/index.md`](wiki/index.md)** is content-oriented — a catalog of every page with a one-line summary, organized by category (concepts, entities, sources, syntheses). The LLM updates it on every ingest. When answering queries, the LLM reads the index first to find relevant pages, then drills into them.

**[`wiki/log.md`](wiki/log.md)** is chronological — an append-only record of ingests, queries, and lint passes. Each entry starts with a consistent prefix (e.g., `## [2026-04-02] ingest | Article Title`), making it parseable with simple tools.

## Naming Conventions

- **Entities** (people, companies): `TitleCase` — `ZhiyuHuang.md`, `Motional.md`
- **Concepts** (methods, frameworks): `TitleCase` — `TreeIRL.md`, `MaximumEntropyIRL.md`
- **Sources** (summaries): `kebab-case` with `摘要-` prefix — `摘要-treeirl-safe-urban-driving.md`
- **Syntheses** (deep notes): `YYYY-Author-Title-Kebab-Case.md`

## Tips and Tricks

- **Obsidian Web Clipper** is a browser extension that converts web articles to markdown. Very useful for quickly getting sources into `raw/01-articles/`.
- **Obsidian's graph view** is the best way to see the shape of your wiki — what's connected to what, which pages are hubs, which are orphans.
- **scholar-skill** supports three reading depths: L1 (quick screening), L2 (standard reading), L3 (deep reading with full method derivation, critical reflection, and knowledge extraction). I default to L3 for papers in my focus area.
- All wiki content is written in Simplified Chinese, even though triggers and skills may accept English.
- The `raw/09-archive/` directory is never read during ingestion — it's a write-only archive.
- Every wiki page must contain a `## 关联连接` section to avoid orphan pages.
