# Project Memory — AI Operation Spec

> AI discovers this file via `wiki/AGENTS.md`. Execute the behavior rules below immediately after reading.

## Locate Project Root

> **After reading this file, derive the project root from this file's absolute path:**
> ```
> wiki/ directory → parent = <project_root>
> ```
> Example: If this file is at `D:\Work\MyProject\wiki\AI-OPERATION-SPEC.md`, then `<project_root>` = `D:\Work\MyProject`.
>
> **All absolute paths below refer to `<project_root>`.**

## AI Agent Behavior Rules (execute immediately)

### Step 0: Long-Term Memory Registration (critical, cross-session)

> **Why?** Each agent session is independent. Without saving wiki info to long-term memory, the agent will forget the wiki exists in the next session and need to rediscover it. Long-term memory is injected at the start of every session — the only mechanism that survives across sessions.

**Execute now**: Write the following to the agent's long-term memory using the `memory` tool:

```
## Project Memory (wiki)
Path: <absolute path to wiki directory>
Entry: <absolute path to wiki directory>/AI-OPERATION-SPEC.md
Rule: Before working on this project, read the entry file first, check wiki before acting, write new knowledge back via the 7-step process
```

> **Write target**: The agent's own long-term memory (not project files). Per-agent implementation:
>
> | Agent | Method |
> |---|---|
> | Hermes | `memory` tool (target=memory), content as above |
> | Claude Code | `<project_root>/.claude/CLAUDE.md` + suggest user add `Remember: this project has a wiki memory` |
> | Codex | `<project_root>/.codex.md` |
> | Cursor | `<project_root>/.cursorrules` |
> | Others | Any persistent memory mechanism |

### Step 1: Project Config Registration (optional but recommended)

Write the "Wiki First" rule to the project config file as a backup:

> | Agent | Write target |
> |---|---|
> | Claude Code | `<project_root>/.claude/CLAUDE.md` |
> | Codex | `<project_root>/.codex.md` |
> | Cursor | `<project_root>/.cursorrules` |
> | Hermes | Done via memory tool in Step 0 |
> | Others | Corresponding config file at project root |
>
> Content to write:
> ```
> ## Wiki First
> Before working on this project, search the wiki/ directory (read AI-OPERATION-SPEC.md first, then check index.md).
>
> ## Knowledge Feedback
> New findings during tasks should be written to wiki following the 7-step process in wiki/AI-OPERATION-SPEC.md.
> ```

### Step 2: Wiki First

Every time you receive a project task, **check wiki before acting**.
   - a. Read `index.md` to understand existing entries
   - b. Search wiki directory filenames and content by keywords
   - c. Even if no results found, note in your response: "Searched wiki, no relevant entries found"

### Step 3: Knowledge Feedback

Any new information discovered should be written to wiki via the 7-step process below, committed together with the task.

### Step 4: Update Long-Term Memory

When wiki gains important entries (especially under `entities/` and `concepts/`), update the wiki summary in long-term memory to ensure fast lookup in the next session.

> **Example**: After adding `entities/project-svn-addresses.md`, update the wiki entry in memory:
> ```
> ## Project Memory (wiki)
> Path: G:\MyProject\wiki
> Entry: G:\MyProject\wiki\AI-OPERATION-SPEC.md
> Rule: ...
> Key entries:
>   - entities/project-svn-addresses: Project SVN addresses
> ```

## Directory Structure

```
wiki/
├── AI-OPERATION-SPEC.md  ← This file (AI operation spec, read first)
├── index.md              ← Master index
├── log.md                ← Changelog (append only)
├── overview.md           ← Project overview (core knowledge summary)
├── sources/              ← Source material summaries
├── concepts/             ← Concept entries (methods, terms, workflows)
├── entities/             ← Entity entries (projects, teams, components)
└── comparisons/          ← Horizontal comparisons
```

## Page Conventions

### YAML Frontmatter (required at top of every md file)

```yaml
---
title: Title
tags: [tag1, tag2]
source_count: 3         # Number of sources (for overview page)
last_updated: YYYY-MM-DD
---
```

### File Naming

`{english-slug}.md`

| Directory | Naming | Example |
|-----------|--------|---------|
| sources/ | `{english-slug}.md` | `sources/codebase-skill-search.md` |
| concepts/ | `{english-slug}.md` | `concepts/config-export-workflow.md` |
| entities/ | `{english-slug}.md` | `entities/project-overview.md` |
| comparisons/ | `{english-slug}.md` | `comparisons/planA-vs-planB.md` |

### Cross-References

Use `[[link]]` syntax (Obsidian compatible). Link targets are filenames without path prefixes.

### Path Conventions

All project-internal paths are relative to `<project_root>`. External paths (URLs, network shares) use absolute paths.

### Tag Conventions

English, comma-separated, covering multiple dimensions for searchability.

### Conflict Handling

When a contradiction with existing content is found, add a `⚠️ Conflict` section noting the contradiction and its source.

## New Information Flow (7 Steps)

```
Source → sources/ summary → concepts/ update → entities/ update → overview/ refinement → index/ update → log/ append
```

1. **Read** the source material completely
2. **sources/**: Create a summary page (frontmatter + key information + `[[related links]]`)
3. **concepts/**: Update or create all related concept pages
4. **entities/**: Update or create all related entity pages
5. **overview/**: Update `overview.md` when core insights change
6. **index/**: Add new pages to the corresponding section in `index.md`
7. **log/**: Append `## YYYY-MM-DD Ingest | Source Name` to `log.md`

> ⚠️ **overview and index must stay in sync**: New entries added to index must also be added to the overview's category lists.

## Maintenance Principles

- **Search before writing**: Check wiki before modifying to avoid duplicates or conflicts
- **Commit with tasks**: Wiki changes are committed together with tasks, not accumulated
- **Append only**: Log only appends, never modifies history
- **Traceable sources**: Every concept/entity traces back to original sources
