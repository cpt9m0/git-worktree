---
name: git-worktree
license: MIT
description: Create, reuse, and work inside an isolated Git worktree with a coding agent. Load when the user asks to use a worktree, work in a worktree, create a worktree, isolate a coding task on a separate branch, or manage an existing worktree. Applies to worktree setup and the associated coding task, not ordinary branch switching.
---

# Git worktrees

When the user asks to do a task in a worktree, set up or reuse the worktree and continue the requested task there. Creating the directory alone does not finish an associated coding request.

## Choose the checkout

1. Identify the intended repository with `git rev-parse --show-toplevel`. Read its applicable repository instructions. Inspect `git status --short --branch` and `git worktree list --porcelain` before making changes.
2. Honor a requested worktree, branch, or base ref. Reuse an existing checkout when it matches the user's task; do not take over an unrelated dirty worktree. If already in the intended worktree, continue there.
3. For a new task, choose a short descriptive branch and a sibling directory outside the repository, unless repository instructions specify another location. Check for existing branch and path collisions. If the base is unspecified, start from the current checkout's HEAD and report that choice. Local uncommitted and untracked files are not carried into the new worktree. Preserve them; ask only if the task depends on including them.

## Create or reuse

Use standard Git commands for portability across coding agents. Substitute actual paths and refs, quote paths, and use an explicit repository directory:

```bash
# New branch from the chosen base
git -C "/absolute/path/repo" worktree add -b "feature/task" "/absolute/path/repo-task" "HEAD"

# Existing branch that is not already checked out elsewhere
git -C "/absolute/path/repo" worktree add "/absolute/path/repo-task" "feature/task"
```

If the branch is already checked out in another worktree, reuse that checkout when appropriate. Do not force the same branch into two worktrees. Do not overwrite an existing directory or reset a branch to resolve a collision. If a command fails, inspect the reason before retrying.

An available native agent worktree or workspace-switch tool is also suitable if its behavior matches the requested branch and base. Verify the returned directory and branch. Do not assume a particular UI control or native tool exists.

## Work in the right directory

Record the worktree's absolute path, branch, and base commit. Verify with:

```bash
git -C "/absolute/path/repo-task" rev-parse --show-toplevel
git -C "/absolute/path/repo-task" branch --show-current
git -C "/absolute/path/repo-task" status --short --branch
```

A `cd` inside a shell tool call may affect only that call; do not assume it changes the agent workspace or the defaults of subsequent tools. A Git worktree also does not automatically retarget the session.

- If the session can switch directory through a supported agent tool, switch it and verify before editing.
- Otherwise, use absolute paths for file operations and the worktree directory for every command. Use a shell tool's working-directory parameter when supported; otherwise prefix each command with `cd "/absolute/path/repo-task" && ...` or use `git -C` for Git operations.
- Check that relative patches, searches, formatters, and test runners resolve inside the worktree. Read applicable instructions in the target checkout. Never edit the original checkout by accident.
- If tools cannot target the new directory, explain how to open the worktree as the agent's workspace or relaunch the actual agent from that directory. Use a verified command for that agent, not an invented generic launcher. Do not claim the session has moved.

Honor the agent environment's directory permissions; use its normal approval mechanism if outside-directory access needs approval. Do not broaden global permissions as part of worktree setup.

Install dependencies or perform local setup only as needed using the repository's established commands. Untracked dependencies and environment files are not automatically copied to worktrees. Avoid blindly copying credentials. Continue implementation and run appropriate checks in the target worktree.

## Finish and cleanup

Report the absolute worktree path, branch, completed work, and verification results. Leave the checkout available for review and follow-up. Creating a worktree does not authorize committing, merging, pushing, or opening a pull request; follow the user's requested scope and repository instructions.

Remove a worktree only when requested. Inspect tracked and untracked changes first and preserve unfinished work. Once safe, run from another checkout:

```bash
git -C "/absolute/path/repo" worktree remove "/absolute/path/repo-task"
```

This removes the checkout but retains its branch and commits. Do not use `--force`, delete the branch, or discard changes without explicit authorization. Do not remove the checkout hosting the active session before moving or exiting that session.

## References

- [Git worktree commands](https://git-scm.com/docs/git-worktree)
