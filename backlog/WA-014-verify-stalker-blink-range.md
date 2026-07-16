---
id: WA-014
status: in-progress
size: S
phase: 1-game-readiness
priority: 8
---
# Re-enable and test stalker blink range upgrade

## 🔨 Code re-enabled 2026-07-16 — awaiting production verification
Uncommented the registration in `upgradeInitializers.galaxy` (`addUpgradeToUpgrade("stalkerblinkrange", ...)` + `AnyOf Stalker`). Stalker pool is now 7 (doc updated).

**Root cause confirmed via git history** (answers the old "don't know why it doesn't work"): the first prod push **a001b4d** shipped the `stalkerblinkrange` CUpgrade with only `Effect,Blink,Range ×2` and **not** `Effect,Blink,PlacementRange ×2` — so the *click/placement* range never grew and the extra teleport range was unreachable. **b40c4e6** later added the missing PlacementRange line, so the data is complete now. Likely lost the second line originally by hitting Cancel instead of Save in the editor.

**Remaining:** deploy to prod, roll/force it onto a Stalker, confirm blink range visibly increases. If good → flip to done + drop the "verify" comment. If still broken → re-disable and log what was seen.

`stalkerblinkrange` is currently commented out of the pool. It was pulled via hotfix after it was confirmed broken on production (found by luck). The underlying issue was likely fixed afterward but never re-deployed or verified — because of an SC2 Editor bug that blocks local testing of this one.

## Why
It's a good, thematic Stalker upgrade that should be in the pool if it works. Right now it's in limbo: probably-fixed but unverified, so it stays disabled and nobody benefits.

## Acceptance criteria
- [x] Re-enable the registration in `upgradeInitializers.galaxy` (uncomment the `addUpgradeToUpgrade("stalkerblinkrange", ...)` + requirement-tag lines).
- [ ] Deploy to production and force/roll it onto a Stalker.
- [ ] Confirm blink range actually increases in game.
- [ ] If it works: leave enabled, note the confirmation in a comment. If it still doesn't: re-disable and log what was observed.

## Notes
This is a "verify on production" ticket, not a local-editor fix — the editor bug means it can't be validated locally. Pairs naturally with any other change you're already deploying, so you're not shipping a build just for this.
