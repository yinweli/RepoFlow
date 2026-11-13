---
name: repo-flow
description: >
  Manage Git branches, scoped commits, pull requests, and safe integration.
  Use when asked to prepare commits, create or update branches, open or merge
  PRs, or integrate multiple feature branches in sequence. Respect explicit
  authorization for history-changing or remote operations.
---

# RepoFlow

Keep Git history focused, changes safe, and integration predictable. Follow explicit project Git policies; do not assume a particular trunk name or merge method.

## Authorization and Safety

- Inspect the working tree, current branch, relevant remotes, and project instructions before changes.
- Never commit, push, rebase, reset, merge, delete branches, or otherwise rewrite history/change remote state without the user's explicit authorization for the relevant action. Permission to use this skill is not permission to perform these operations.
- Preserve unrelated changes and untracked files. Do not stage, restore, overwrite, or clean another task's work.
- Use PRs and required reviews/checks when the repository requires them; never bypass branch protection to complete a task.
- If the user requests advice or analysis only, do not modify Git state.

## Commit Rules

- Each commit covers one coherent purpose and includes only relevant files.
- Perform applicable verification before committing; report any checks not run or failing.
- Use the default message format `Type | 說明`, where Type is English and 說明 is concise Traditional Chinese.
- Allowed default types: `Feature`, `Fix`, `Design`, `Doc`, `Sheet`, `Tool`. Projects may add types without changing the meaning of these defaults.
- Do not amend or rewrite existing commits unless specifically authorized.

## Branch and PR Workflow

1. Identify the project's integration branch (such as `dev`) and branch/PR conventions; do not invent a branching policy.
2. Create a task branch from the designated integration branch when authorized and appropriate.
3. Keep changes scoped, verify them, and prepare a PR according to repository rules.
4. Before merging each branch, verify it against the **latest** integration branch, rerun relevant checks, and confirm required PR reviews/CI.
5. When integrating multiple branches in a specified order, **rebase each remaining branch onto the latest integration branch after the preceding branch has merged**. Read `references/sequential-rebase.md` for that workflow.
6. After an authorized merge, perform only approved branch cleanup and synchronization. Do not assume all non-trunk branches are disposable.

## Rebase Precautions

- Rebase rewrites commit IDs. Never rebase someone else's shared branch or force-update remote history without explicit approval and ownership/coordination.
- A rebased remote branch may require `--force-with-lease`; never substitute plain `--force`, and do not push without authorization.
- Stop and report conflicts needing a design decision; never silently discard commits or choose a semantic resolution.
- A successful rebase does not imply successful tests or permission to merge.

## Completion

Report the branches and commits affected, actions actually performed, checks/PR status, unresolved conflicts, and any pending authorizations. Do not claim CI or merging succeeded unless verified.
