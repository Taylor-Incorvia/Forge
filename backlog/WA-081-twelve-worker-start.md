---
id: WA-081
status: done
size: S
phase: 1-game-readiness
priority: 30
---
# Switch to a 12-worker start before Season 1 launch

## What
Change the starting worker count to **12** (from the current start count) so a new game opens with 12 SCVs on the main.

## Why
The broader SC2 melee scene is standardizing on 12-worker starts — several popular mods/ladders have reverted to it, and it's the community-preferred opener (skips the slow early worker ramp, gets to the interesting part faster). Wildcard should match the convention players now expect. Good to land as a pre-S1 polish item while the ruleset is being finalized.

## Resolution (done 2026-08-16)
Starting workers are data-driven via each race's `CRace` → `StartingUnitArray` (the melee-init native reads it). voidmulti sets `index="1"` (the worker slot) to `Count="8"` — the reduced retail default (Blizzard cut the start 12→8). Overrode it back to 12 at the mod level.

- **New `Base.SC2Data/GameData/RaceData.xml`** — `<StartingUnitArray index="1" Count="12"/>` for `Terr`, `Prot`, and `Zerg` (commit `d9aa767`). All three races are set because Wildcard replaces starting Drones/Probes 1:1 with SCVs, so whatever race the map assigns a player/AI, they end up with 12 SCVs.
- **Town-hall supply restored to 15** — the same 12→8 nerf also cut town-hall supply 15→13 (`Food` in voidmulti). A 12-worker start on 13 supply is supply-blocked at 0:00. Overrode `Food=15` on CommandCenter, CommandCenterFlying, OrbitalCommand, OrbitalCommandFlying, and PlanetaryFortress (commit `d47fe0e`; added a CommandCenterFlying entry so a lifted CC doesn't drop 2 supply). PlanetaryFortress included pending confirmation that the planetary morph works in-mod.

## Acceptance
- [x] New game starts each player with exactly 12 SCVs on the main. (Confirmed in-game by Taylor.)
- [x] Applies to all players (and AI test opponents). (RaceData covers all three races; replacement yields SCVs for each.)
- [x] No duplicate/leftover starting-unit grants — the override changes the count, it doesn't add a second batch.
- [x] Town-hall supply supports the larger start (15, not 13).

## Notes
Data-only change; no galaxy/trigger logic needed. Landed directly on `main`.
