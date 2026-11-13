# Sequential Rebase Before Merge

Use when the user asks to integrate multiple branches into one trunk in a defined order. This is a **per-branch update before merging**, not a one-time rebase of all branches at the start.

## Example: A → B → C into `dev`

All branches began from `dev`, and the intended integration order is A, B, C.

1. Confirm authorization, merge order, clean/preserved worktree state, and the actual integration branch. Fetch the current remote state when authorized and needed.
2. Prepare A against the current target (rebase A first if the target advanced and project policy requires it). Verify A, confirm the PR/CI requirements, then merge A through the project's approved merge method.
3. Refresh the target after A is merged. **Rebase B onto the latest target containing A.** Resolve authorized conflicts, inspect the resulting diff/commit range, rerun tests and required CI, update B's PR if needed, then merge B.
4. Refresh the target after B is merged. **Rebase C onto the latest target containing A and B.** Repeat conflict review, checks, PR update, and merge.
5. Synchronize the local trunk and remove local/remote branches or stale tracking refs only when cleanup is specifically authorized.

Illustrative local sequence (not permission to execute):

```bash
git fetch origin
git switch B
git rebase origin/dev
# Review differences, run checks, update PR when authorized; then merge B.
git fetch origin
git switch C
git rebase origin/dev
# Review differences, run checks, update PR when authorized; then merge C.
```

**Important:** `origin/dev` above must already contain the preceding completed merge. If the integration branch is not `dev`, replace it with the project's actual branch. `git rebase` affects the current local branch; updating a previously pushed feature branch may require a separately authorized `git push --force-with-lease` and can invalidate prior reviews or checks.

## Safeguards

- Never rebase all remaining branches once and treat them as current after further merges.
- Re-check that the target has not advanced again before each integration.
- If conflicts require business/design decisions, pause that branch and ask for a decision; do not guess.
- Respect repository PR, branch-protection, and merge-queue policy. A merge queue can impose a different approved integration mechanism; do not bypass it to force this manual sequence.
- Do not interpret `rebase` as permission for an automatic merge or force-push.
