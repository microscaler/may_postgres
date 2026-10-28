# Contributing to this fork

This repository is a fork with two long-lived branches:

- **`master` tracks upstream.** It mirrors upstream's default branch; nothing of ours is committed to it.
- **`integration/microscaler-fork` carries every change made in this fork** on top of `master`. It is the
  default branch and the only branch that projects depending on the fork should consume (a dependent can
  follow just one branch).

## Making a change

- Every fix or feature goes to `integration/microscaler-fork` first, through a pull request merged with a
  merge commit.
- The pull request for the upstream project is cut separately, fresh from upstream's default branch, as a
  single-purpose change (one concern, one commit, following upstream's conventions). It is not merged
  from the integration branch.
- Before an upstream pull request merges or closes, check that its content is on the integration branch
  (by content, not commit hashes: upstream branches are re-cut).

## After an upstream update

1. Update `master` to upstream's default branch.
2. **Before rebasing, tag the current integration head and push the tag**, e.g.
   `git tag integration-2026-10-28 origin/integration/microscaler-fork && git push origin integration-2026-10-28`.
   Lockfiles pin integration commits; the tag keeps those commits reachable after the rebase, so older
   releases still rebuild.
3. Rebase `integration/microscaler-fork` onto `master`, dropping changes upstream now contains, run the
   tests, and force-push the integration branch (the only branch that is ever force-pushed).
4. Re-lock every consumer to the new integration head and check each has a single copy of this crate in
   its dependency graph.

Never reset or rewrite `master` while consumers still pin commits that exist only on it; move them to the
integration branch (or tag those commits) first.
