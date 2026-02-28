---
description: Show Aigon commands (shortcut for help)
---
# Aigon Commands

## Feature Commands (unified for solo and arena modes)

| Command | Description |
|---------|-------------|
| `/aigon:feature-create <name>` | Create a new feature spec |
| `/aigon:feature-now <name>` | Fast-track: inbox → prioritise → setup → implement, or create new + implement |
| `/aigon:feature-prioritise <name>` | Assign ID and move to backlog |
| `/aigon:feature-setup <ID> [agents...]` | Setup for solo (branch) or arena (worktrees) |
| `/aigon:feature-implement <ID> [--ralph]` | Implement feature; `--ralph` enables the autonomous retry loop |
| `/aigon:feature-submit` | Commit changes, write log, signal done for evaluation |
| `/aigon:feature-eval <ID>` | Create evaluation (code review or comparison) |
| `/aigon:feature-review <ID>` | Code review with fixes by a different agent |
| `/aigon:feature-done <ID> [agent]` | Merge and complete feature |
| `/aigon:feature-cleanup <ID>` | Clean up arena worktrees and branches |
| `/aigon:worktree-open [ID] [agent]` | Open worktree in Warp with agent CLI |

## Research (unified for solo and arena modes)

| Command | Description |
|---------|-------------|
| `/aigon:research-create <name>` | Create a new research topic |
| `/aigon:research-prioritise <name>` | Prioritise a research topic |
| `/aigon:research-setup <ID> [agents...]` | Setup for solo or arena research |
| `/aigon:research-open <ID>` | Open all arena agents side-by-side for parallel research |
| `/aigon:research-conduct <ID>` | Conduct research (write findings) |
| `/aigon:research-done <ID>` | Complete a research topic |

## Feedback

| Command | Description |
|---------|-------------|
| `/aigon:feedback-create <title>` | Create feedback item in inbox with next ID |
| `/aigon:feedback-list [filters]` | List feedback items with status/type/severity/tag filters |
| `/aigon:feedback-triage <ID>` | Run triage preview and apply with explicit confirmation |

## CLI Commands (run in terminal)

| Command | Description |
|---------|-------------|
| `aigon config init` | Create global config at `~/.aigon/config.json` |

### Agent CLI Mappings (used by worktree-open)

| Code | Agent | Command | Mode |
|------|-------|---------|------|
| cc | Claude Code | `claude --permission-mode acceptEdits` | Auto-edits, prompts for risky Bash |
| cu | Cursor | `agent --force` | Auto-approves commands (yolo mode) |
| gg | Gemini | `gemini --yolo` | Auto-approves all |
| cx | Codex | `codex --full-auto` | Workspace-write, smart approval |

**Quick-allow when prompted:** Claude `Shift+Tab` • Gemini `2` for always • Cursor "Add to allowlist" • Codex "Allow and remember"

**Override defaults:** Set `agents.{id}.implementFlag` in `~/.aigon/config.json` to use stricter permissions (e.g., `""` to require manual approval). Project config (`.aigon/config.json`) takes precedence over global config.

## Shortcuts

All commands have top-level short aliases prefixed with `a` (for aigon):

| Shortcut | Command | Shortcut | Command |
|----------|---------|----------|---------|
| `/afc` | feature-create | `/arc` | research-create |
| `/afn` | feature-now | `/arp` | research-prioritise |
| `/afp` | feature-prioritise | `/arse` | research-setup |
| `/afse` | feature-setup | `/aro` | research-open |
| `/afi` | feature-implement | `/ard` | research-conduct |
| `/afs` | feature-submit | `/ars` | research-synthesize |
| `/afe` | feature-eval | `/ardn` | research-done |
| `/afr` | feature-review | `/afbc` | feedback-create |
| `/afd` | feature-done | `/afbl` | feedback-list |
| `/ab` | board | `/afbt` | feedback-triage |
| `/ads` | dev-server | `/ah` | help |

Run `aigon help` in terminal for full CLI reference.
