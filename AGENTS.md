<!-- asbos-template-version: 0.1.0 -->
# AGENTS.md — Vault Rulebook

This file is the single source of truth for how any AI agent works in this vault. `CLAUDE.md` and `GEMINI.md` (and any other agent-specific file) only point here — the rules themselves live in exactly one place.

## Vault structure

The vault follows a PARA-style layout: eight top-level folders, each with one clear job. Every agent and every skill assumes these exact names and this order.

| Folder | Purpose |
|---|---|
| `00 Context` | Personal profile and reference material the agent should read before doing content or writing tasks — who the user is, how they work, how they write. |
| `01 Inbox` | Unprocessed capture: quick thoughts, brain dumps, anything without a home yet. |
| `02 Projects` | Active work with a concrete goal and an end date. New projects start as a single file directly in this folder. |
| `03 Areas` | Ongoing responsibilities with no end date — the standing parts of life and work that keep running. |
| `04 Resources` | Knowledge base: reference material, documentation, anything worth keeping for later lookup. |
| `05 Daily Notes` | Daily log, one file per day, named `YYYY-MM-DD.md`. Gives continuity between sessions. |
| `06 Archive` | Completed projects and inactive areas, moved here only on explicit request. |
| `07 Attachments` | Images, PDFs, and other media referenced from notes. |

## Vault rules

- Use `[[wikilinks]]` to connect notes to each other.
- Keep notes atomic: one idea per note. The one exception is daily notes, which are a running log.
- Every note's YAML frontmatter includes `tags`, `status` (`active` / `completed` / `paused`), and `date`.
- File names use normal spelling and spaces — no forced kebab-case or underscores.
- A new note with no clear place goes into `01 Inbox/`.
- New projects start as a single `.md` file directly under `02 Projects/`; split into a folder only once a project genuinely needs multiple files.
- Areas and Resources are always folders, not single files.
- Move completed work into `06 Archive/` only when the user explicitly asks for it — never automatically.
- Always ask before deleting or overwriting a note.
- When the user says "remember this," file the information in the topically correct place — a writing-style note goes to the relevant style guide, project knowledge goes into the project file, general reference goes into Resources, and vault-wide rules go here, in `AGENTS.md`.

## Session routines

**Session start:**
1. Pull the latest changes: `git pull --no-edit origin main`.
2. Show any new commits since the last session.
3. Check `01 Inbox/` for new notes and offer to triage them.

**On request** ("where was I?" or "what's active right now?"): read the last 2–3 daily notes plus the currently active projects, then give the user a short briefing.

**Session end:** offer to (1) create today's daily note, (2) save any new insights from the session as notes, and (3) clean up the inbox.

## Git sync (mandatory)

After EVERY change to the vault, commit and push automatically:

```bash
git add -A && git commit -m "short description" && git push
```

Do not batch multiple unrelated changes into a single silent commit at the end of a session — sync as you go, so the vault stays recoverable at every step.

## Skill reflex (mandatory)

Before ANY task — not just complex ones — check whether a skill in this vault's `skills/` folder already covers it. Each skill is its own folder with a `SKILL.md` entry point. If a skill matches the task, read its `SKILL.md` and follow it step by step instead of improvising a fresh approach.

Claude Code loads skills from this folder natively. Every other agent (Codex, Gemini CLI, Cursor, Copilot) must do this check manually: list `skills/`, scan for a matching `SKILL.md`, and read it before starting work.

## Model strategy

Plan and brainstorm with the strongest model available and in your agent's plan mode; implement with a cheaper model. When a plan is approved, the agent proposes the switch itself and names the exact command (Claude Code: `/model` + plan mode; Codex: `/model`; Gemini CLI: `-m` flag). Details: `guides/model-strategy.md`.

## Maintenance

On the first session of a new month, compare today's date with the date recorded in `.maintenance-log.md`. If a month or more has passed, offer to run the maintenance check described in `MAINTENANCE.md` — it keeps the rulebook, skills, and adapters current as agents and tools evolve.

## Agent-specific notes

Every agent reads its rules from the same place, `AGENTS.md`, but gets there through a different file:

| Agent | Entry file | Notes |
|---|---|---|
| Codex CLI | `AGENTS.md` | Reads this file natively — no adapter needed. |
| Cursor | `AGENTS.md` | Reads this file natively — no adapter needed. |
| GitHub Copilot | `AGENTS.md` | Reads this file natively — no adapter needed. |
| Claude Code | `CLAUDE.md` | Thin adapter that points to `AGENTS.md`. |
| Gemini CLI | `GEMINI.md` | Thin adapter that points to `AGENTS.md`. |

MCP server configuration is agent-specific and lives separately in `manifest/mcp.md`, not in this file.
