# Personal Extras

This doc lives **only on the `personal` branch**. `main` intentionally excludes it.

## What gets installed

`scripts/install-personal-extras.sh` wires two things:

### 1. Handoff skills
Symlinks `handoff-system.md` + `resume.md` from the toolkit into `~/.claude/skills/`. These skills coordinate session handoff across AI CLIs (Claude Code ↔ Codex) — picking up a conversation in Codex when Claude Code hits its session limit, and vice-versa.

### 2. Codex shortcut scripts
Writes `~/.codex/scripts/{rag-recall,rag-pack,spec,aggregate-roadmap,claude-handoff}.sh`. Each is a one-line wrapper that exec's into the shared `~/.claude/rag-index/scripts/` so Codex sessions use the same RAG venv Claude Code uses.

## Why it's not on `main`

- **Governance:** cross-vendor session handoff writes to both `~/.claude/` and `~/.codex/` — at work, those directories may be audited and outbound writes to a second AI vendor raise SI flags.
- **Compliance:** automatic context transfer between different AI providers is awkward to audit. The `main` branch avoids it entirely so the pitch at work is "one vendor, one audit surface."
- **Opt-in:** on a personal machine you want the handoff; on a work machine you don't. Keeping it off `main` makes the default the work-safe one.

## How to use

On a personal machine, after `./bootstrap.sh`:

```bash
bash scripts/install-personal-extras.sh
```

Re-run any time — idempotent. Skips steps when their prerequisites are missing.

## Removing

Personal extras are symlinks + tiny shell wrappers — delete `~/.claude/skills/handoff-system.md`, `~/.claude/skills/resume.md`, and `~/.codex/scripts/*.sh` to revert. No config file touches needed.
