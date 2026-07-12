---
id: WA-023
status: todo
size: S
phase: 1-game-readiness
priority: 3
---
# Make Chitinous Plating researchable instead of free (Ultralisk nerf)

Ultralisks currently start with Chitinous Plating for free (`grantInitialUpgrades()`). Since the ~40% faster build times (PiG feedback), you can tech to Ultras and mass them too fast — the free armor makes them oppressive. Move Chitinous Plating into the rollable upgrade pool so it must be researched.

## Why
It was balanced before the build-time cut. Rather than undo the (good) speed change, make the Ultra's power come at a cost. Ultralisk = Factory slot 3 (researched from the Armory).

## Design note — this makes it a *rolled* upgrade, not guaranteed
In this mod, pool upgrades are randomly rolled per slot. So after this change an Ultralisk will only *sometimes* roll Chitinous Plating — not always have access. That's a real nerf plus added variance. Confirm that matches intent (vs. "always available to research").

## Implementation — follows the CLAUDE.md "Count Upgrade" recipe, slot 3
- [ ] `upgradeInitializers.galaxy`: remove `grantUpgrade(player, "ChitinousPlating");` from `grantInitialUpgrades()` (~line 260).
- [ ] `upgradeInitializers.galaxy`: add to pool:
  - `addUpgradeToUpgrade("ChitinousPlating", "ChitinousPlating");`
  - `addUpgradeRequirementTag("ChitinousPlating", logicType_AnyOf, "unitTag", "Ultralisk");`
- [ ] `AbilData.xml`: one `CAbilResearch id="ChitinousPlating3"` with `Upgrade="3"`.
- [ ] `ButtonData.xml`: one `CButton id="ChitinousPlating3"` with `<DefaultButtonLayout Column="2"/>` (slot 3 → column 2). Needs an icon (reuse stock Chitinous Plating / an armor icon).
- [ ] `GameStrings.txt`: `Abil/Name/ChitinousPlating3`, `Button/Name/ChitinousPlating3`, `Button/Tooltip/ChitinousPlating3`.
- [ ] Verify in game: Ultralisk no longer starts with the armor; it appears as a researchable upgrade at the Armory when rolled.

## Fallback if this over-nerfs
If Ultras become worst-in-slot, start them with the Ultralisk speed upgrade instead:
`grantUpgrade(player, "AnabolicSynthesis");` — note this was previously removed as "too strong" (comment in `grantInitialUpgrades`), so it's a known lever. Speed-but-no-free-armor may land in a better spot than free armor.

## Notes
Ultralisk slot confirmed: `initialize.galaxy` — Factory slot 3 (with ThorAP, Colossus). Cost/time for the research: mirror the other count upgrades (150/150, 120s) unless you want it cheaper.
