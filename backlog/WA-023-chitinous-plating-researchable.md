---
id: WA-023
status: done
size: S
phase: 1-game-readiness
priority: 3
---
# Make Chitinous Plating researchable instead of free (Ultralisk nerf)

## ✅ Done 2026-07-16
Chitinous Plating is now a **rolled** count-upgrade at the Armory (Factory slot 3), not free. Wired per the CLAUDE.md count-upgrade recipe:
- `upgradeInitializers.galaxy`: commented out `grantUpgrade(player, "ChitinousPlating")` in `grantInitialUpgrades()`; added `addUpgradeToUpgrade("ChitinousPlating","ChitinousPlating")` + `AnyOf Ultralisk`.
- `AbilData.xml`: `CAbilResearch ChitinousPlating3` (Upgrade="3", 150/150, 120s).
- `ButtonData.xml`: `CButton ChitinousPlating3` (`Column="2"`, stock `btn-upgrade-zerg-chitinousplating.dds` icon).
- `GameStrings.txt`: Abil/Name + Button/Name ("Chitinous Plating") + Tooltip ("Increases the Ultralisk's armor by 2").

Verified against reference: Ultralisk = Factory slot 3 (`initializeFactorySlot3Pool`); `ChitinousPlating` = `Unit,Ultralisk,LifeArmor +2`. Count/stat upgrade → locally testable (no publish needed).

**Effect:** Ultralisk starts with **no** free armor and only *sometimes* rolls Chitinous Plating (real nerf + variance, as intended).

**Follow-up (same session):** applied the fallback lever proactively — re-enabled `grantUpgrade(player, "AnabolicSynthesis")` so Ultras now start with **speed instead of free armor**. It was previously disabled (3/9) as "too strong," but that was while it stacked *with* free armor; with armor moved to a roll, speed-only is the intended baseline.

Ultralisks currently start with Chitinous Plating for free (`grantInitialUpgrades()`). Since the ~40% faster build times (PiG feedback), you can tech to Ultras and mass them too fast — the free armor makes them oppressive. Move Chitinous Plating into the rollable upgrade pool so it must be researched.

## Why
It was balanced before the build-time cut. Rather than undo the (good) speed change, make the Ultra's power come at a cost. Ultralisk = Factory slot 3 (researched from the Armory).

## Design note — this makes it a *rolled* upgrade, not guaranteed
In this mod, pool upgrades are randomly rolled per slot. So after this change an Ultralisk will only *sometimes* roll Chitinous Plating — not always have access. That's a real nerf plus added variance. Confirm that matches intent (vs. "always available to research").

## Implementation — follows the CLAUDE.md "Count Upgrade" recipe, slot 3
- [x] `upgradeInitializers.galaxy`: removed free `grantUpgrade(player, "ChitinousPlating");` (commented).
- [x] `upgradeInitializers.galaxy`: added `addUpgradeToUpgrade` + `AnyOf Ultralisk`.
- [x] `AbilData.xml`: `CAbilResearch id="ChitinousPlating3"` with `Upgrade="3"`.
- [x] `ButtonData.xml`: `CButton id="ChitinousPlating3"` with `Column="2"` + stock icon.
- [x] `GameStrings.txt`: Abil/Name, Button/Name, Button/Tooltip.
- [x] Verify in game: Ultralisk no longer starts with the armor; appears as a researchable upgrade at the Armory when rolled. **Confirmed working 2026-07-16** (armor upgrade researches + applies). Visibility polish → WA-046.

## Fallback if this over-nerfs
If Ultras become worst-in-slot, start them with the Ultralisk speed upgrade instead:
`grantUpgrade(player, "AnabolicSynthesis");` — note this was previously removed as "too strong" (comment in `grantInitialUpgrades`), so it's a known lever. Speed-but-no-free-armor may land in a better spot than free armor.

## Notes
Ultralisk slot confirmed: `initialize.galaxy` — Factory slot 3 (with ThorAP, Colossus). Cost/time for the research: mirror the other count upgrades (150/150, 120s) unless you want it cheaper.
