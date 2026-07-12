---
id: WA-021
status: done
size: M
phase: 1-game-readiness
priority: 1
---
# Extract base-game catalogs into a reference/ folder (force multiplier)

## ✅ DONE (2026-07-11)
Extracted all six deps (Core, Liberty Mod, Liberty Campaign, Swarm, Void, VoidMulti) + trigger libs + Core/Liberty UI into `reference/` (gitignored). 1,810 files. Also grabbed `starcoop` as a future asset reference — NOT a dependency, out of scope. Spot-checks passed: ShieldWall (Liberty), stock Hydralisk 100/50/2 (Liberty), EvolveGroovedSpines, Queen abilities, Core natives.galaxy. Confirmed IDs backfilled into WA-016/017/019.

Give Claude read access to the stock SC2 catalogs this mod extends, so it can look up real unit stats, ability ids, and upgrade ids instead of guessing. Speeds up nearly every gameplay ticket.

## Why
This repo contains only *our overrides*. The base data (stock units/abilities/upgrades) lives inside SC2's CASC storage (`SC2Data/`), which is binary — not readable. Every ticket where Claude wrote "confirm exact id in UpgradeData.xml" is a symptom. One extraction removes all that guesswork.

## What this mod depends on (fully-resolved chain, confirmed from the editor's dependency tabs)
Six stock mods, in merge order (Core is the base, each later one overrides):
1. `Core.SC2Mod`  — base definitions everything inherits from
2. `Mods/Liberty.SC2Mod`
3. `Campaigns/Liberty.SC2Campaign`
4. `Mods/Swarm.SC2Mod`
5. `Mods/Void.SC2Mod`
6. `Mods/VoidMulti.SC2Mod`  ← current multiplayer balance (most-used)

`DocumentInfo` only lists 3 direct deps (Void, VoidMulti, Liberty Campaign); the rest come in transitively. This mod uses many campaign/co-op units whose stock data lives in Liberty/Swarm/Void — so all six are worth extracting.

## Recommended method: CascView (no install, free)
1. Download Ladik's **CascView** (zezula.net).
2. Open CASC storage → point it at `C:\Program Files (x86)\StarCraft II`.
3. Navigate and extract these, for each of the six: `core.sc2mod`, `mods/liberty.sc2mod`, `campaigns/liberty.sc2campaign`, `mods/swarm.sc2mod`, `mods/void.sc2mod`, `mods/voidmulti.sc2mod`:
   - `.../base.sc2data/GameData/` (all XML — UnitData, AbilData, UpgradeData, ButtonData, BehaviorData, WeaponData, Requirement*Data, EffectData, MoverData...)
   - `.../enus.sc2data/LocalizedData/` (GameStrings.txt, GameHotkeys.txt)
4. Extract them into a new **`reference/`** folder at the repo root, preserving the per-mod subfolder structure, e.g. `reference/voidmulti/GameData/UnitData.xml`.

## Acceptance criteria
- [ ] `reference/` exists at repo root with extracted GameData + LocalizedData for all six: Core, Liberty (Mod), Liberty (Campaign), Swarm, Void, VoidMulti.
- [ ] `reference/` is added to `.gitignore` — do NOT commit Blizzard's files (copyright; matters once the repo/website is public).
- [ ] Claude confirms it can read stock values (spot-check: Hydralisk stock cost, `ShieldWall` upgrade, Grooved Spines id).

## Notes
- Alternative method: SC2 Editor with the dependency open — but CascView is the reliable "dump plain XML" route.
- One-time-ish: stock data for existing units rarely changes across patches. Re-extract only if Blizzard reworks something.
- Once extracted, Claude will do a first pass and can fill in the "confirm the id" gaps already sitting in WA-017 (Grooved Spines), WA-019 (Queen ability ids), WA-016 (ShieldWall).
