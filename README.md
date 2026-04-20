# codebase-router

Lightweight routing network for large codebases. Builds a tiny `.routing/` folder (one `INDEX.md` + per-area/per-node files, max 15 lines each) that Claude reads at session start so it jumps straight to the relevant code instead of scanning the whole project.

## Install

Add the marketplace, then install the plugin:

```
/plugin marketplace add alexnordheim/codebase-router
/plugin install codebase-router@codebase-router
```

## Quick Start

Inside any project:

```
/codebase-router:init
```

Scans the project, builds `.routing/`, done. From the next session onwards Claude routes automatically — no manual navigation.

Ask in natural language: *"I want to work on habits"* → Claude loads `INDEX.md` + `habits/_.md` + 1 node file and confirms the context.

Other commands:
- `/codebase-router:add` — add a new area manually
- `/codebase-router:sync` — refresh routing if the codebase changed a lot

## Routing Format

Every routing file follows the same shape. Design rules: **max 15 lines**, no prose, only structured signal. If a node grows past 15 lines, split it into sub-nodes.

**`.routing/INDEX.md`** — root router
```
# ROUTER
stack: [tech-stack]

AREAS
[name]  → [area]/_.md  | [3-4 keywords]
```

**`.routing/[area]/_.md`** — area router
```
# [AREA]
root: [folder-path(s)]

NODES
[name]  → [area]/[node].md  | [key types/functions]

DEPS: [other-areas]
```

**`.routing/[area]/[node].md`** — leaf
```
# [AREA]/[NODE]
[file-path-1]
[file-path-2]

[KeyType]: {field1 field2 field3}
[KeyFn]: (params) → return

CHANGED: [YYYY-MM-DD]
DEPS: [area/node]
```

## Example

A `.routing/` for a Next.js task planner (~100 files, 8 areas):

```
.routing/
├── INDEX.md
├── changelog.md
├── tasks/     _.md + models api hooks ui
├── planner/   _.md + modes ai ui dashboard
├── habits/    _.md + models hooks ui
├── gym/       _.md + models hooks ui
├── health/    _.md + models hooks ui
├── finance/   _.md + models hooks ui
├── life/      _.md + goals journal time calendar
└── shared/    _.md + ui layout stores firebase auth utils
```

## How it works

At session start, Claude silently reads `.routing/INDEX.md`. When you mention a feature or area, it fuzzy-matches to one area, reads that `_.md`, picks the relevant node, reads it, and confirms the loaded context in one sentence. Maximum context touched: INDEX + 1 area file + up to 2 node files.

After file changes (`Write`/`Edit`), Claude silently updates the relevant node file and appends a line to `.routing/changelog.md`. No noise, no summaries unless you explicitly ask.

## License

MIT
