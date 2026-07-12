# reference/ — extracted stock SC2 catalogs (local only, gitignored)

This folder holds the **base game data** this mod extends, extracted once from SC2's CASC storage so Claude can look up stock unit stats, ability ids, and upgrade ids instead of guessing. See ticket **WA-021**.

**Not committed to git** (Blizzard copyright — see `.gitignore`). This README is the only tracked file here.

## What lives here
Extracted `GameData/` (XML catalogs) and `LocalizedData/` (GameStrings.txt, GameHotkeys.txt) from the stock dependencies:

The six stock dependencies (merge order — Core is the base, each later one overrides):

| Mod | CASC path | Why it matters |
|-----|-----------|----------------|
| Core | `core.sc2mod` | Base definitions everything inherits from |
| Liberty (Mod) | `mods/liberty.sc2mod` | WoL shared data |
| Liberty (Campaign) | `campaigns/liberty.sc2campaign` | Campaign units (War Pig, Firebat, Vulture, Medic…) |
| Swarm | `mods/swarm.sc2mod` | HotS units (Hydralisk, Viper, Swarm Host…) |
| Void | `mods/void.sc2mod` | LotV campaign/co-op units (Corsair, Arbiter, Defiler MP…) |
| VoidMulti | `mods/voidmulti.sc2mod` | Current multiplayer balance — most ladder units |

Confirmed from the editor's dependency tabs. Folder layout doesn't need to be exact — extract preserving CASC paths and Claude will adapt.

### Also present (beyond the 6 catalogs)
- **`triggerlibs/`** in each mod — base galaxy libraries. `core.sc2mod/.../triggerlibs/natives.galaxy` is the full built-in function reference.
- **`ui/`** in Core and Liberty — layout files, kept as reference for the Arsenal modal (WA-001).
- **`mods/starcoop/`** — ⚠️ NOT a dependency of this mod. Extracted only as a future reference for possibly borrowing a co-op unit/asset. Out of scope; do not treat as part of the mod's data.

### Data layering (where stock values actually live)
Liberty = foundational multiplayer unit definitions (e.g. Hydralisk cost, ShieldWall). Swarm = HotS units. Void = LotV/co-op units. VoidMulti = balance *overrides* on top (often no cost block — look to Liberty/Swarm for the base). Core = templates + natives.

## How it was made
Extracted with CascView (Ladik's, zezula.net) pointed at the SC2 install. See WA-021 for the step-by-step.
