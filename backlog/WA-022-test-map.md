---
id: WA-022
status: done
size: M
phase: 1-game-readiness
priority: 1
---
# Dev-mode test setup (triggers only — no .sc2map)

## ✅ DONE (2026-07-11) — structures-only
Shipped: `enableTestingCheats()` in `test2.galaxy` grants 50k/50k and spawns all 6 buildings (Barracks/Factory/Starport + Ghost Academy/Armory/Fusion Core) near each player's start, gated by `devMode`, called from a 1.0s delayed timer trigger. Good enough to skip the tech-tree build-up.

Descoped: the "one of every unit" spawn — pools came back empty at spawn time (a pool-timing/data-keying issue), and structures-only was deemed sufficient. Revisit if you ever want the full unit dump. ⚠️ Keep `devMode = false` in commits.

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
- [x] Extend the devMode path (in/alongside `enableTestingCheats()`) to spawn, near the testing player's start location:
  - [x] All production structures (Barracks, Factory, Starport).
  - [x] All upgrade facilities (Ghost Academy, Armory, Fusion Core) so rolled upgrades are researchable.
  - [x] One of every rollable unit — looped off the slot pools in `initialize.galaxy` (`slotPoolKey`) so it stays current.
- [x] Units/structures placed in a grid/offset from the start point so they don't stack.
- [x] Resources granted (existing 50k/50k kept).
- [ ] **YOU:** call `enableTestingCheats()` from a trigger that runs AFTER start locations exist — a `Timer - Elapsed Game Time 1.0s` one-shot trigger, NOT Mod Initialization. (Mod Init is too early: `PlayerStartLocation` returns null → null-point errors. Fixed the null case with a guard, but the spawn only works once deferred.) Also ensure `initializePools()` has run first (the spawn reads the pools).
- [ ] **YOU:** flip `devMode = true` (nativeHelpers.galaxy:3-4), launch any melee map with the mod, confirm everything spawns and is testable.
- [ ] `devMode = false` verified before commit. ⚠️

## Code written (2026-07-11)
Implemented in `test2.galaxy`: `spawnTestArsenalForPlayer(player)` + helpers, called from `enableTestingCheats()` inside the existing `if(devMode)` loop. Uses `UnitCreate` / `PlayerStartLocation` / `PointWithOffset` (verified against `reference/` natives). Tuning knobs at top of file: `c_testPerRow`, `c_testSpacing`. To spawn for yourself only, see the commented guard in `enableTestingCheats()`.

Optional tech-lab addons were left out (they attach to production buildings and complicate placement) — add later if a unit needs one to appear.

## Known limitation (possible follow-up)
Pre-spawning units lets you test unit stats/abilities instantly, but a *specific* upgrade still depends on what the roll system assigned to each slot. To reliably test any given upgrade, a small devMode helper to force-grant a named upgrade (building on the existing `grantUpgrade`) would close the gap. Note it and split into a follow-up if you want it.

## Notes
- Per CLAUDE.md, wire the trigger in the editor UI and have it call the galaxy function; keep everything behind `if(devMode)`.
- Confirm the existing devMode init trigger actually calls `enableTestingCheats()` (or add a `setupTestEnvironment(player)` it calls) so the spawn fires.
- `natives.galaxy` (now in `reference/`) has the exact unit-create / point / start-location functions for the placement loop.
- Tradeoff vs. the old .sc2map idea: gives up the speculative "fresh map dodges the WA-014 caching bug" side-benefit, in exchange for zero terrain work. Worth it.
