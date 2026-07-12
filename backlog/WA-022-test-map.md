---
id: WA-022
status: todo
size: M
phase: 1-game-readiness
priority: 1
---
# Dev-mode test setup (triggers only — no .sc2map)

A `devMode`-gated setup that, on game start, spawns every structure + one of every unit and hands you resources — so testing a unit/upgrade is instant instead of "build the tech tree, build the unit" every time. **No terrain editor, no separate map file** — runs on whatever melee map you launch with the mod.

## Why
Kills the repeated build-up loop on nearly every gameplay ticket, and makes you more likely to actually test before shipping. Force multiplier.

## Approach (revised): triggers-only, hooked to the existing devMode flag
Ditches the earlier `.sc2map` idea — you don't want to touch the terrain editor, and you don't need to. The scaffolding already exists:
- `devMode` flag: `nativeHelpers.galaxy:3` (toggle via the commented line 4).
- `enableTestingCheats()` in `test2.galaxy` — already `devMode`-gated, already grants 50k/50k to all players. **This is the hook point.**
- The mod already runs on any melee map ("Create with Mod"), so a devMode-gated spawn fires wherever you play.

## ⚠️ Safety — the one rule
`devMode` MUST stay `false` in committed code. If it ships `true`, real games spawn everything. Keep the toggle a local-only comment swap; never commit it flipped.

## Acceptance criteria
- [ ] Extend the devMode path (in/alongside `enableTestingCheats()`) to spawn, near the testing player's start location:
  - [ ] All production structures (Barracks, Factory, Starport) + their tech lab addons.
  - [ ] All upgrade facilities (Ghost Academy, Armory, Fusion Core) so rolled upgrades are researchable.
  - [ ] One of every rollable unit — ideally looped off the existing slot pools in `initialize.galaxy` so it stays current as the roster changes.
- [ ] Units/structures placed in a grid/offset from the start point so they don't stack (main new trigger work).
- [ ] Resources granted (existing 50k/50k is plenty).
- [ ] `devMode = false` verified before commit.
- [ ] Test: flip devMode on locally → launch any melee map with the mod → everything's there, select any unit and test immediately.

## Known limitation (possible follow-up)
Pre-spawning units lets you test unit stats/abilities instantly, but a *specific* upgrade still depends on what the roll system assigned to each slot. To reliably test any given upgrade, a small devMode helper to force-grant a named upgrade (building on the existing `grantUpgrade`) would close the gap. Note it and split into a follow-up if you want it.

## Notes
- Per CLAUDE.md, wire the trigger in the editor UI and have it call the galaxy function; keep everything behind `if(devMode)`.
- Confirm the existing devMode init trigger actually calls `enableTestingCheats()` (or add a `setupTestEnvironment(player)` it calls) so the spawn fires.
- `natives.galaxy` (now in `reference/`) has the exact unit-create / point / start-location functions for the placement loop.
- Tradeoff vs. the old .sc2map idea: gives up the speculative "fresh map dodges the WA-014 caching bug" side-benefit, in exchange for zero terrain work. Worth it.
