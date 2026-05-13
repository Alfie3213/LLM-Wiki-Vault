# LLM Wiki Vault

An Obsidian-based knowledge management system for academic paper reading and research note-taking, focused on autonomous driving, inverse reinforcement learning (IRL), and motion planning.

> [中文版请见 README_CN.md](README_CN.md)

## Overview

This project implements a layered knowledge architecture inspired by the Zettelkasten method, with Claude Skills automating ingestion, maintenance, and querying workflows. The vault is bilingual (Chinese/English) and optimized for deep paper reading (L3 standard) with structured synthesis notes.

## Directory Structure

```
LLM-Wiki-Vault/
├── raw/                    # Inbox for unprocessed materials
│   ├── 01-articles/        # Web articles and markdown clips
│   ├── 02-papers/          # PDF papers and academic literature
│   ├── 03-transcripts/     # Video transcripts
│   ├── 04-meeting_notes/   # Meeting notes
│   └── 09-archive/         # Processed and archived files
├── wiki/                   # Compiled knowledge base
│   ├── concepts/           # Abstract concepts, frameworks, methods
│   ├── entities/           # People, companies, tools, products
│   ├── sources/            # Source summaries with extracted metadata
│   ├── syntheses/          # L3 deep reading notes
│   ├── index.md            # Global registry of all pages
│   └── log.md              # Operation log (append-only)
├── .claude/skills/         # Claude skill definitions
└── .obsidian/              # Obsidian configuration
```

## Quick Start

### 1. Add Raw Materials

Place papers, articles, or transcripts into the appropriate `raw/` subdirectory.

### 2. Ingest into Knowledge Base

Trigger the ingest skill to compile raw materials into the wiki:

```
/ingest
```

Or process a specific file:

```
/ingest raw/02-papers/my-paper.pdf
```

This will:

- Extract core concepts and entities
- Create source summaries in `wiki/sources/`
- Generate/update concept and entity pages
- Update `wiki/index.md` and `wiki/log.md`
- Move the source file to `raw/09-archive/`

### 3. Query the Knowledge Base

```
/query What are the key differences between MaxEnt IRL and SMIRL?
```

### 4. Check Health

```
/lint
```

## Available Skills

### Core Knowledge Management

| Skill                   | Trigger                                  | Description                                                                         |
| ----------------------- | ---------------------------------------- | ----------------------------------------------------------------------------------- |
| **ingest**        | `/ingest` or "import this paper"       | Compile raw materials into the wiki, extract entities/concepts, and archive sources |
| **lint**          | `/lint`, `/scan`, `/health`        | Check for dead links, orphan pages, unregistered pages, and knowledge conflicts     |
| **query**         | `/query` or natural language questions | Query the knowledge base and answer based on wiki content                           |
| **generate-mocs** | `/generate-mocs`                       | Regenerate index pages and Maps of Content                                          |

### Visualization & Diagrams

| Skill                             | Trigger                                  | Description                                                                              |
| --------------------------------- | ---------------------------------------- | ---------------------------------------------------------------------------------------- |
| **mermaid-visualizer**      | "create a flowchart" or "visualize this" | Generate Mermaid diagrams (flowcharts, mindmaps, sequence diagrams, etc.)                |
| **excalidraw-diagram**      | "Excalidraw", "画图", "流程图"           | Generate Excalidraw diagrams in Obsidian (.md), Standard (.excalidraw), or Animated mode |
| **obsidian-canvas-creator** | "create a canvas" or "mind map"          | Create Obsidian Canvas files (MindMap or freeform layouts)                               |

### Obsidian Integration

| Skill                       | Trigger                      | Description                                                                          |
| --------------------------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| **obsidian-cli**      | "obsidian" commands          | Interact with a running Obsidian instance via CLI (read, create, search notes)       |
| **obsidian-markdown** | Working with `.md` files   | Create and edit Obsidian Flavored Markdown (wikilinks, callouts, properties, embeds) |
| **obsidian-bases**    | Working with `.base` files | Create database-like views with filters, formulas, and summaries                     |

### Utilities

| Skill                   | Trigger                    | Description                                                               |
| ----------------------- | -------------------------- | ------------------------------------------------------------------------- |
| **defuddle**      | "extract from URL"         | Extract clean markdown from web pages using Defuddle CLI                  |
| **scholar-skill** | "read paper", "L3 reading" | Structured paper reading with L1/L2/L3 standards and knowledge extraction |

### Paper Pipeline (Daily Papers)

| Skill                         | Trigger          | Description                                      |
| ----------------------------- | ---------------- | ------------------------------------------------ |
| **daily-papers-fetch**  | "fetch papers"   | Fetch and enrich latest arXiv/HuggingFace papers |
| **daily-papers-review** | "review papers"  | Generate curated recommendations with commentary |
| **daily-papers-notes**  | "batch notes"    | Generate structured notes for recommended papers |
| **daily-papers**        | "today's papers" | End-to-end daily paper recommendation pipeline   |

## Usage Examples

### Import a Paper

```
Please ingest the paper at raw/02-papers/TreeIRL.pdf
```

### Create a Diagram

```
Create a flowchart of the TreeIRL pipeline
```

### Deep Paper Reading

```
Read this paper with L3 standard: raw/02-papers/DIPP.pdf
```

### Visualize as Canvas

```
Turn the TreeIRL synthesis into a mind map canvas
```

### Check Knowledge Base Health

```
/scan
```

## Naming Conventions

- **Entities** (people, companies): `TitleCase` (e.g., `ZhiyuHuang.md`, `Motional.md`)
- **Concepts** (methods, frameworks): `TitleCase` (e.g., `TreeIRL.md`, `MaximumEntropyIRL.md`)
- **Sources** (summaries): `kebab-case` with `摘要-` prefix (e.g., `摘要-treeirl-safe-urban-driving.md`)
- **Syntheses** (deep notes): `YYYY-Author-Title-Kebab-Case.md`

## Key Dependencies

- **Obsidian**: Desktop knowledge base application
- **Obsidian Excalidraw Plugin**: For `.md` format Excalidraw diagrams
- **Defuddle CLI**: `npm install -g defuddle` (for web content extraction)
- **Obsidian CLI**: For command-line vault operations

## Notes

- All wiki content is written in Simplified Chinese
- The `raw/09-archive/` directory is never read during ingestion
- Every wiki page must contain a `## 关联连接` section to avoid orphan pages
- The `wiki/index.md` serves as the global registry and is auto-updated by skills
