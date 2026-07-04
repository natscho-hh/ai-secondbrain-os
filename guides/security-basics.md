# Security Basics

Five habits that keep your vault and your machine safe while you work with an AI agent. None of these are optional — they're the guardrails the whole setup is built around.

## 1. Never put secrets in notes or commits

API keys, tokens, and passwords never belong in a vault note or a git commit — anything committed to git is, in practice, permanent and can end up copied, synced, or pushed somewhere you didn't intend. Agent configuration — where API keys for MCP servers actually live — stays **outside the vault**, in the agent's own local config file or your shell environment, never in a file the vault tracks.

## 2. The security gate for third-party skills

Core skills that ship with the template are already vetted. Anything else — a community skill, something you found in a directory, anything not from `skills/` in this repo — has to pass this exact check before it gets installed (this is the same check `SETUP.md` runs in Phase 4, word for word):

> Before installing, I check: (1) Is the source a public repo with real usage? (2) I read the skill's files for commands that touch things outside the vault (network calls, deletions, credential access). (3) I summarize what it does in one sentence and wait for your OK.

If any part of that check raises a concern — an unknown or private source, no real usage you can verify, commands that reach outside the vault without a clear reason — your agent should tell you plainly and let you decide with that information in hand. Nothing third-party installs without your explicit OK for that specific skill.

## 3. MCP servers run code on your machine

An MCP (Model Context Protocol) server isn't a passive config file — it's a real process that runs on your computer and can make network calls, read files, or call external APIs on the agent's behalf. Install MCP servers only from official sources (the vendor's own repo, npm package, or documentation) and check `manifest/mcp.md` before pointing an agent at one you found elsewhere.

## 4. The dedicated-folder principle

Your agent only ever operates inside the vault folder — never your home directory, never a system folder, never a folder with unrelated projects mixed in. This is the same rule `SETUP.md` Phase 0 enforces before anything else gets built: a dedicated folder means an agent can only ever affect what's actually part of your second brain, so a mistake stays contained instead of reaching the rest of your computer.

## 5. Reversibility: commit before risky operations

Before any operation that's hard to undo by hand — a large restructuring, a bulk rename, deleting several notes at once — make sure there's a clean git commit first. Every commit is a checkpoint you can return to with `git revert` or `git reset`, so as long as you commit before the risky step, nothing you do to the vault is ever truly permanent.
