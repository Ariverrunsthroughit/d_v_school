# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

`C:\02.Obsidian` is **not a single vault or a code project** — it is a parent folder (not a git repo) holding several independent Obsidian vaults and loose notes. There is no build, lint, or test tooling. Work is editing Markdown notes in Obsidian-flavored syntax. The owner writes in **Korean**; keep note titles and content in Korean.

The folder itself has a `.obsidian/` (plugins: Readwise official, Templater, Zotero Desktop Connector), plus loose notes (`Claude Code by Josh.md`, `무제.md`), `Clippings/` and `Readwise/` (imported articles/books).

## Sub-vaults (each has its own `.obsidian/` config)

- `Obsidian Vault/` — personal Zettelkasten-style vault: `01. 임시 메모` → `02. 문헌 메모` → `03. 영구 메모`, plus `04. 일간노트`, `50. Inbox`, `60. Daily Note`, `70. Templates`, `Books`.
- `obsidian-goldenrabbit-main/` — downloaded companion vault for the book "세컨드 브레인은 옵시디언" (PARA + Zettelkasten example). Treat as reference material; it has its own `CLAUDE.md` (note search order: QMD MCP `search` → `vector_search` → `deep_search`, falling back to Glob/Grep) and `.claude/skills`.
- `obsidian_business/` — nearly empty PARA skeleton (`1_Project`, `2_Area`, `3_Resource`, `4_Archive`) with the Terminal plugin.
- `obsedian/` (sic) — research/consulting material:
  - `princilples/` — the "지식 서재" vault. **Read its `CLAUDE.md` before touching it**; it defines the rules summarized below.
  - `trinity/trinity/` — numbered research folders (Concept, Paper Review, Zotero, Experiment, Research Idea, Paper Work, Administrative Work, Template) with an `0. Inbox.base`.

## 지식 서재 pipeline (`obsedian/princilples`)

Claude conversation exports flow through a multi-stage pipeline:

1. `convert_claude_export.py <export.zip|folder|conversations.json> --vault <vault> [--since YYYY-MM-DD] [--dry-run]` writes one file per conversation to `raw/claude/YYYY/MM/` and regenerates `raw/claude/_대화목록.md` (do not edit). It is idempotent by conversation ID and preserves the frontmatter keys `tags`, `status`, `summary`, `triage` across re-runs.
2. **Triage** (`status: raw` → `분류완료`): fill `triage` (`보관`/`참고`/`제외`), `summary`, `tags` in one-line frontmatter form; batches of 20–30; log results in `wiki/_분류기록.md`.
3. **Note-ization** (only `triage: 보관`): create concept notes in `wiki/개념/` (when a concept appears in 2+ conversations), project notes in `wiki/프로젝트/`, MOCs in `wiki/MOC/`; then set source `status: 노트화완료`.

Hard rules from that vault: never edit `raw/` bodies (only the four frontmatter keys), never invent facts not in the source, link every wiki claim back to its raw source, summarize (don't copy) client financial/technical details, and never delete files without user confirmation.

## Editing conventions

- Preserve Obsidian syntax: `[[wikilinks]]`, `![[embeds]]`, callouts (`> [!NOTE]`), YAML frontmatter, `^block-id` references, and Templater blocks (`<% %>`, `<%* %>`).
- `.base` files are Obsidian Bases (YAML: filters, formulas, views, sort) — keep the schema valid.
- Don't hand-edit `.obsidian/workspace.json` or plugin `main.js`; they are app/plugin-managed.
