<!-- AIGON_START -->
# Claude Code Configuration

## Agent Identity
- **Agent ID**: `cc`
- **Worktree Pattern**: `../feature-NN-cc-description`
- **Implementation Log**: `./docs/specs/features/logs/feature-NN-cc-log.md`

## Commands

### Feature Commands (unified for solo and arena modes)
| Command | Description |
|---------|-------------|
| `/aigon:feature-create <name>` | Create a new feature spec |
| `/aigon:feature-prioritise <name>` | Assign ID and move to backlog |
| `/aigon:feature-setup <ID> [agents...]` | Setup for solo (branch) or arena (worktrees) |
| `/aigon:feature-implement <ID>` | Implement feature in current branch/worktree |
| `/aigon:ralph <ID> [options]` | Run iterative implement+validate loop with fresh agent sessions |
| `/aigon:feature-eval <ID>` | Create evaluation (code review or comparison) |
| `/aigon:feature-review <ID>` | Code review with fixes by a different agent |
| `/aigon:feature-done <ID> [agent]` | Merge and complete feature |
| `/aigon:feature-cleanup <ID>` | Clean up arena worktrees and branches |

### Research Commands (unified for solo and arena modes)
| Command | Description |
|---------|-------------|
| `/aigon:research-create <name>` | Create a new research topic |
| `/aigon:research-prioritise <name>` | Prioritise a research topic |
| `/aigon:research-setup <ID> [agents...]` | Setup for solo or arena research |
| `/aigon:research-open <ID>` | Open all arena agents side-by-side for parallel research |
| `/aigon:research-conduct <ID>` | Conduct research (write findings) |
| `/aigon:research-done <ID>` | Complete research topic |

### Feedback Commands
| Command | Description |
|---------|-------------|
| `/aigon:feedback-create <title>` | Create a feedback item in inbox |
| `/aigon:feedback-list [filters]` | List feedback by status/type/severity/tag |
| `/aigon:feedback-triage <ID>` | Triage feedback with explicit apply confirmation |

### Utility Commands
| Command | Description |
|---------|-------------|
| `/aigon:help` | Show all Aigon commands |

## Modes

- **Solo mode**: `/aigon:feature-setup <ID>` - Creates branch only, work in current directory
- **Arena mode**: `/aigon:feature-setup <ID> <agents...>` - Creates worktrees for parallel implementation

## Critical Rules

1. **Read the spec first**: Always check `./docs/specs/features/03-in-progress/` before coding
2. **Work in isolation**: Solo mode uses branches, arena mode uses worktrees
3. **Conventional commits**: Use `feat:`, `fix:`, `chore:` prefixes
4. **Complete properly**: Use `/aigon:feature-done <ID>` for solo, `/aigon:feature-done <ID> cc` for arena
5. **Follow project instructions**: Check `AGENTS.md` for shared project build, test, and dependency commands

## Solo Mode Workflow

1. Run `/aigon:feature-setup <ID>` to create branch and move spec
2. Run `/aigon:feature-implement <ID>` to begin implementation
3. Read the spec in `./docs/specs/features/03-in-progress/feature-<ID>-*.md`
4. Implement the feature according to the spec
5. Test your changes and wait for user confirmation
6. Commit using conventional commits (`feat:`, `fix:`, `chore:`)
7. Update the implementation log in `./docs/specs/features/logs/`
8. **STOP** - Wait for user to approve before running `/aigon:feature-done <ID>`

## Arena Mode Workflow

1. Run `/aigon:feature-setup <ID> cc cx gg cu` to create worktrees for each agent
2. **STOP** - Tell the user to open the worktree in a separate session
3. In the worktree session:
   - Run `/aigon:feature-implement <ID>`
   - Read the spec in `./docs/specs/features/03-in-progress/feature-<ID>-*.md`
   - Implement the feature
   - Commit your changes
   - Update the implementation log
   - **STOP** - Do NOT run `feature-done` from worktree
4. Return to main repo for evaluation: `/aigon:feature-eval <ID>`
5. Merge winner: `/aigon:feature-done <ID> cx`
6. Clean up losers: `/aigon:feature-cleanup <ID> --push` (to save branches) or `/aigon:feature-cleanup <ID>` (to delete)


## Saving Permissions

When the user says "save that permission", "remember that", or asks you to persist a permission you were just granted:

1. Read `.claude/settings.json`
2. Add the tool pattern to the `permissions.allow` array
3. Write the updated file
4. Confirm what you saved

Use the narrowest pattern that covers the user's intent:
- Specific: `Bash(npm test)`, `Bash(git push:*)`
- Broad: `Bash(npm:*)`, `Bash(node:*)`

Prefer specific patterns — broad Bash allows can be chained to bypass deny rules. Ask the user if unsure whether to save narrowly or broadly.

## Before Completing a Feature

Before running `/aigon:feature-done`, always:

1. **Push the branch to origin** to save your work remotely:
   ```bash
   git push -u origin <current-branch-name>
   ```
2. **Ask the user** if they want to delete the local branch after merge (the CLI will delete it by default)
<!-- AIGON_END -->