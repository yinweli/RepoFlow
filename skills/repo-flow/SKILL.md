---
name: repo-flow
description: >
  Manage Git branches, scoped commits, pull requests, and safe integration.
  Use when asked to commit, create or update branches, open or merge PRs, or
  integrate multiple feature branches in sequence.
---

# RepoFlow

Keep Git history focused, changes safe, and integration predictable. Follow explicit project Git policies; do not assume a particular trunk name or merge method.

## Authorization and Safety

- Before changes, check state with one combined command such as `git status -sb && git log --oneline -5`. Check remotes only before a push or PR. Read project Git instructions only if they are not already in context.
- Read-only commands (`status`, `diff`, `log`, `show`, `branch`, `fetch`) need no authorization.
- Never commit, push, rebase, reset, merge, delete branches, or otherwise rewrite history/change remote state without the user's explicit authorization for the relevant action. Permission to use this skill is not permission to perform these operations.
- A request to commit (e.g. "commit", "提交") authorizes the commit. A request to draft or plan one (e.g. "準備", "整理", "草擬") means listing the files and proposed message only.
- Preserve unrelated changes and untracked files. Do not stage, restore, overwrite, or clean another task's work.
- Use PRs and required reviews/checks when the repository requires them; never bypass branch protection to complete a task.
- If the user requests advice or analysis only, do not modify Git state.

## Commit Rules

- Each commit covers one coherent purpose and includes only relevant files.
- Verify before committing. If verification for these exact changes already ran in this session, report that result instead of rerunning. Report any checks not run or failing.
- Message format: the first line is `Type | 說明`, where Type is English and 說明 is concise Traditional Chinese. A multi-line body is allowed when it adds useful context. Keep any `Co-Authored-By` trailers the tool requires.
- Default types (projects may add more without changing these meanings):
  - `Feature`: code changes, including new features, refactors, tests, and dependency upgrades
  - `Fix`: bug fixes
  - `Design`: design documents
  - `Doc`: other documentation (README, guides, notes)
  - `Sheet`: tabular data files, such as game skill or level-up tables
  - `Tool`: utility scripts, project-specific small tools, and development workflow such as CI, task runners, and AI instruction files (e.g. `CLAUDE.md`)
- Use only the types above and any the project defines; do not invent new types.
- Do not amend or rewrite existing commits unless specifically authorized.

## Branch and PR Workflow

1. Identify the project's integration branch (such as `dev`) and branch/PR conventions; do not invent a branching policy.
2. Create a task branch from the designated integration branch when authorized and appropriate.
3. Keep changes scoped, verify them, and prepare a PR according to repository rules.
4. Before merging each branch, verify it against the **latest** integration branch, rerun relevant checks, and confirm required PR reviews/CI.
5. When integrating multiple branches in a specified order, **rebase each remaining branch onto the latest integration branch after the preceding branch has merged**. Read `references/sequential-rebase.md` for that workflow.
6. After an authorized merge, perform only approved branch cleanup and synchronization. Do not assume all non-trunk branches are disposable; if the project expects only the trunk and current branch to remain but others exist, list them and ask before deleting.

## Rebase Precautions

- Rebase rewrites commit IDs. Never rebase someone else's shared branch or force-update remote history without explicit approval and ownership/coordination.
- A rebased remote branch may require `--force-with-lease`; never substitute plain `--force`, and do not push without authorization.
- Stop and report conflicts needing a design decision; never silently discard commits or choose a semantic resolution.
- A successful rebase does not imply successful tests or permission to merge or force-push.

## Completion

Scale the report to the task. For a single commit, give the hash and message. For multi-step work, report the branches and commits affected, actions performed, checks/PR status, unresolved conflicts, and pending authorizations. Do not claim CI or merging succeeded unless verified.
