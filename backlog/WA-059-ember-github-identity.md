---
id: WA-059
status: todo
size: S
phase: 1-game-readiness
priority: 30
---
# Give Ember its own GitHub identity for PRs / reviews

## Why
Right now Ember (the assistant) opens PRs and posts review replies using **your** personal access token, so everything shows up as **you**. That makes it impossible for you to formally review/approve Ember's PRs — you can't approve your own PR, and the authorship is misleading. A separate identity fixes both: Ember's PRs come from "Ember," and you review + sign off as yourself.

## Options (pick one)
1. **Machine/bot user account** — create a second GitHub account (e.g. `ember-wildcard`), invite it as a collaborator on the repo, generate a PAT for it, store it as a separate env var (e.g. `EMBER_GH_TOKEN`). Ember uses that token for pushes/PRs/comments. Simplest; behaves like a normal user you can request reviews from. (GitHub allows machine users.)
2. **GitHub App / bot** — more "correct" for automation (commits attributed to a bot, finer-grained permissions), but heavier setup (app registration, installation token minting). Overkill for a solo repo.
3. **Co-author only** — keep using your token but attribute commits to Ember via `Co-Authored-By` (already done). Doesn't solve the PR-review-as-yourself problem; rejected as the primary fix.

**Recommendation:** option 1 (machine user). Low effort, directly enables "Ember opens the PR, you approve it."

## What Ember needs from you
- The machine account created + added as a collaborator (write access) on `Taylor-Incorvia/Forge`.
- Its PAT saved to a new User env var (e.g. `EMBER_GH_TOKEN`) via `setx`, same as the current `GH_TOKEN` flow.

## What Ember will change
- Use `EMBER_GH_TOKEN` (not `GH_TOKEN`) for `git push`, PR creation, and PR comments, so they attribute to Ember.
- Update the memory note [[reference-github-pr-workflow]] to reference the new token + identity.

## Acceptance criteria
- [ ] Ember's PRs and comments show the Ember account as author.
- [ ] You can open the "Files changed" tab and submit an approving review as yourself.
- [ ] Pushes still work (collaborator write access).
