---
id: WA-014
status: todo
size: S
phase: 1-game-readiness
priority: 8
---
# Re-enable and test stalker blink range upgrade

`stalkerblinkrange` is currently commented out of the pool. It was pulled via hotfix after it was confirmed broken on production (found by luck). The underlying issue was likely fixed afterward but never re-deployed or verified — because of an SC2 Editor bug that blocks local testing of this one.

## Why
It's a good, thematic Stalker upgrade that should be in the pool if it works. Right now it's in limbo: probably-fixed but unverified, so it stays disabled and nobody benefits.

## Acceptance criteria
- [ ] Re-enable the registration in `upgradeInitializers.galaxy` (uncomment the `addUpgradeToUpgrade("stalkerblinkrange", ...)` + requirement-tag lines, 220-221).
- [ ] Deploy to production and force/roll it onto a Stalker.
- [ ] Confirm blink range actually increases in game.
- [ ] If it works: leave enabled, note the confirmation in a comment. If it still doesn't: re-disable and log what was observed.

## Notes
This is a "verify on production" ticket, not a local-editor fix — the editor bug means it can't be validated locally. Pairs naturally with any other change you're already deploying, so you're not shipping a build just for this.
