# AI Pocketable Memory Framework

[![中文说明](https://img.shields.io/badge/README-中文说明-blue)](./README.zh.md)

> An ultra-lightweight AI memory framework for mid-size projects. One directory, plug and play, AI maintains it automatically.
>
> **Fully compatible with [Obsidian](https://obsidian.md/)** — `[[wikilinks]]`, YAML frontmatter, and directory structure work out of the box. It's both an AI memory store and a human-browsable Obsidian vault.

## Why This?

In the vibe coding era, you use AI to write code. But every new AI session has to rediscover your project from scratch. Docs scattered everywhere, AI context can't hold it all, and you don't have time to organize.

This framework means **zero maintenance for you**:

- **Plug and play** — Copy one directory to your project root, AI discovers and registers automatically
- **Zero maintenance** — No manual documentation needed, AI auto-accumulates knowledge after each task
- **Cross-session memory** — AI remembers where the wiki is, no rediscovery needed
- **Human-readable** — Obsidian compatible, open your project to visually browse AI-accumulated knowledge

> You just let AI do the work. The knowledge base maintains itself.

## Core Design: Layered Summary Pyramid

```
sources (raw material summaries)
    ↓ distill
concepts + entities
    ↓ aggregate
overview (holistic summary)
    ↓ navigate
index (master directory)
    ↓ log
changelog
```

AI gets the big picture from overview, drills down as needed. No need to load everything at once.

## Quick Start

### 1. Copy to your project

```bash
cp -r ai-pocketable-memory-framework/wiki/ your-project/wiki/
```

One command. That's it. `wiki/AGENTS.md` guides AI to automatically discover the knowledge base.

### 2. Start using

`wiki/AGENTS.md` directs AI to read `wiki/AI-OPERATION-SPEC.md`, which then:

1. Writes the wiki path to its long-term memory (survives across sessions)
2. Automatically checks wiki before every future task

## Directory Structure

```
your-project/
└── wiki/
    ├── AGENTS.md              ← Trigger file (guides AI to discover wiki)
    ├── AI-OPERATION-SPEC.md   ← AI operation spec (AI reads this first)
    ├── index.md               ← Master index
    ├── overview.md            ← Core knowledge summary
    ├── log.md                 ← Append-only changelog
    ├── sources/               ← Source material summaries
    ├── concepts/              ← Concept entries (methods, terms, workflows)
    ├── entities/              ← Entity entries (projects, teams, components)
    └── comparisons/           ← Horizontal comparisons (plan A vs B)
```

## How It Works

### Three-Layer Discovery

| Layer | Mechanism | Description |
|-------|-----------|-------------|
| 1 | Long-term memory | AI writes wiki path to its own memory after reading the spec; auto-injected each session |
| 2 | Auto-discovery | AI reads `AGENTS.md` when scanning the `wiki/` directory |
| 3 | Explicit registration | Wiki reference in project config files (optional backup) |

### Knowledge Feedback (7-Step Flow)

```
Source → sources/ summary → concepts/ update → entities/ update → overview/ refinement → index/ update → log/ append
```

AI automatically沉淀s new knowledge following this flow after each task. No manual intervention needed.

### Cross-Session Memory

```
Session 1: Read wiki → Discover SVN address → Write to wiki → Update long-term memory
                                                              ↓
Session 2: Memory auto-injected → Knows wiki location → Finds SVN address → No rediscovery needed
```

## Compatibility

### Obsidian Native

This framework's structure and Markdown conventions fully follow Obsidian standards:

- **`[[wikilinks]]`** — Concepts, entities, and sources cross-reference each other; Obsidian's graph view visualizes the knowledge network
- **YAML frontmatter** — `title`, `tags`, `last_updated` metadata parsed natively by Obsidian
- **Directory categories** — `sources/`, `concepts/`, `entities/`, `comparisons/` map directly to Obsidian folder organization
- **Zero config** — Open the project root in Obsidian and it just works

> Open your project in Obsidian and instantly get a visual AI knowledge graph.

### Agent Compatibility

| Platform | Support |
|----------|---------|
| Hermes Agent | ✅ Reads AGENTS.md via directory scan |
| Claude Code | ✅ Reads CLAUDE.md / AGENTS.md |
| Codex | ✅ Reads .codex.md / AGENTS.md |
| Cursor | ✅ Reads .cursorrules / AGENTS.md |
| Obsidian | ✅ Full compatibility (wikilinks + frontmatter) |
| GitHub / GitLab | ✅ Markdown renders normally |
| Any file-reading Agent | ✅ Works |

## Design Principles

- **Zero dependencies** — Pure Markdown, no database, no server, no toolchain
- **Zero maintenance** — AI reads and writes automatically, no manual documentation
- **AI-native** — Not "human docs with AI bolted on," designed from day one for AI agents
- **Incremental** — Start with an empty directory, knowledge accumulates with use
- **Traceable** — Every knowledge entry traces back to its original source

## License

[MIT License](LICENSE) — Free to use, modify, and distribute.
