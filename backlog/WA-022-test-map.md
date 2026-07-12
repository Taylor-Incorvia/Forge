---
id: WA-022
status: todo
size: M
phase: 1-game-readiness
priority: 1
---
# Build a test map (force multiplier)

A dedicated dev map that spawns every structure + unit and starts you with 9999 resources, so testing a unit/upgrade is instant instead of "turn on cheats, rebuild the tech tree, build the unit" every single time.

## Why
This is the loop you repeat constantly. Killing it saves time on nearly every gameplay ticket and makes you more likely to actually test changes (instead of shipping to prod untested). Force multiplier, like WA-021.

## Key approach: spawn via triggers, don't hand-place
You're strong with Data + Triggers and new to Terrain — so lean into that:
- **Start from a copy of an existing small melee map** so the terrain is already done. You barely open the terrain editor.
- **Spawn everything in an init trigger**, not by manually placing units. This plays to your strengths AND self-maintains: add a unit to the roster later and it appears in the test map automatically (drive the spawn loop off the same unit lists used in `initialize.galaxy`).
- Extend the existing `test2.galaxy` `enableTestingCheats()` (already gives 50k resources behind `devMode`) rather than starting fresh.

## Acceptance criteria
- [ ] A new test `.SC2Map`, copied from a small existing melee/blank map (terrain pre-done).
- [ ] Map dependency set to `ForgeModLowConfidence.SC2Mod` (+ its deps) so it uses the real mod data.
- [ ] On init: player starts with 9999 minerals / 9999 gas.
- [ ] On init: one of every production structure + upgrade facility is created.
- [ ] On init: one of every rollable unit is created (ideally looped off existing roster lists so it stays current).
- [ ] Instant/fast build enabled so tech + addons don't slow testing.
- [ ] Confirm: open map → immediately select any unit and test its abilities/upgrades.

## Decisions to make while building
- Where the map lives (keep it in the repo, e.g. a `dev/` folder, so it's versioned — it's a dev artifact, NOT part of the published mod).
- One of each unit, or a few, or grouped by facility? Start with one of each.

## Bonus: might unblock local testing (see WA-014)
You suspect a fresh map could dodge the SC2 Editor's dev-only caching bug — the "works on prod, not on dev" issue that currently blocks testing stalker blink range (WA-014) and forces prod testing generally. **Test that theory here.** If a clean test map makes dev testing reliable, that removes the "test on prod only" tax on a whole class of bugs — arguably a bigger win than the test map itself.

## Notes
Unit roster / slot pools live in `initialize.galaxy`. Existing cheat scaffold in `test2.galaxy`. Per CLAUDE.md, don't set up triggers in script — you'll wire the init trigger in the editor UI and have it call a galaxy function (e.g. an expanded `enableTestingCheats()` / new `spawnAllTestUnits(player)`).
