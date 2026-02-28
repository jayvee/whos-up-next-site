---
description: Setup feature <ID> [agents...] - prepare workspace for implementation
argument-hint: "<ID> [agents...]"
---
# aigon-feature-setup

Prepare your workspace to implement a feature in either solo or arena mode.

**CRITICAL:** You MUST use the CLI command below to perform setup. Do NOT manually move spec files, create branches, or create worktrees — the CLI handles committing the spec move before creating worktrees, which is essential for worktree modes.

## Argument Resolution

If no ID is provided, or the ID doesn't match an existing feature in the backlog:
1. List all files in `./docs/specs/features/02-backlog/` matching `feature-*.md`
2. If a partial ID or name was given, filter to matches
3. Present the matching features and ask the user to choose one

## Step 1: Run the CLI command

```bash
# Solo mode (creates branch in current repo)
aigon feature-setup {{args}}

# Solo worktree mode (creates worktree for parallel development)
aigon feature-setup {{args}} <agent>

# Arena mode (multiple agents compete in separate worktrees)
aigon feature-setup {{args}} <agent1> <agent2> [agent3...]
```

The mode is determined automatically based on parameters:
- **No agents**: Solo mode - creates a git branch in the current repo
- **1 agent**: Solo worktree mode - creates a worktree for parallel development
- **2+ agents**: Arena mode - creates worktrees for each agent to compete

The CLI will:
- Move the spec from `02-backlog` to `03-in-progress`
- **Commit the spec move** (so worktrees inherit it)
- Create branch or worktree(s)
- Create implementation log(s)


**If the CLI reports any errors or warnings about committing the spec move, resolve them before proceeding.** Worktrees branch from HEAD — if the spec move isn't committed, the worktree won't have the spec in `03-in-progress` and `feature-implement` will fail.

## Step 2: Confirm setup and next steps

### Solo Mode (branch)

After the CLI completes:
```bash
/aigon:feature-implement 55     # Start implementing
```

### Solo Worktree Mode (parallel development)

After the CLI completes, open the worktree using:
```
/aigon:worktree-open 55
```

This opens Warp terminal with the agent CLI running `/aigon:feature-implement` automatically.

Remember that `/aigon:feature-done` must be run from the main repo later.

### Arena Mode (competition)

After the CLI completes, open all worktrees side-by-side:
```bash
/aigon:worktree-open 55 --all         # Opens all arena agents side-by-side in Warp
```

Or open individually:
```
/aigon:worktree-open 55 cc    # Open Claude's worktree
/aigon:worktree-open 55 gg    # Open Gemini's worktree
/aigon:worktree-open 55 cx    # Open Codex's worktree
```

Each terminal opens with the agent CLI running `/aigon:feature-implement` automatically. After all implementations complete, return to main repo and run `/aigon:feature-eval 55` to compare.

## Important Notes

- **Solo mode**: You'll work in your current repository on the feature branch
- **Solo worktree mode**: You'll work in an isolated worktree — ideal for parallel development of multiple features
- **Arena mode**: Multiple agents compete on the same feature in their own worktrees
- Worktrees are created in `../<repo>-worktrees/` to keep them grouped with the project

## Prompt Suggestion

End your response with the suggested next command on its own line. This influences Claude Code's prompt suggestion (grey text). Use the actual ID and choose based on mode:

- **Solo mode (branch):** `/aigon:feature-implement <ID>`
- **Solo worktree:** `/aigon:worktree-open <ID>`
- **Arena:** `/aigon:worktree-open <ID> --all`
