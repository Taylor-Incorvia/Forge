---
id: WA-048
status: todo
size: S
phase: 1-game-readiness
priority: 12
---
# Add a tooltip: Colossus starts with Extended Thermal Lance (9 range)

The Colossus is granted **Extended Thermal Lance** at spawn (`grantUpgrade(player, "ExtendedThermalLance")` in `grantInitialUpgrades`), which raises its weapon range from 7 → **9**. Nothing on the unit tells the player this — add a tooltip, same idea as the Marine (Combat Shield, WA-016) and Hydralisk (Grooved Spines, WA-017) free-start-upgrade tooltips.

## Why
Range 9 vs the stock 7 is a meaningful, invisible buff. A player reading the card has no way to know their Colossus out-ranges a "normal" one. Surface it.

## Findings
- Confirmed: mod grants `ExtendedThermalLance` at spawn; the effective merged range is **9** (base 7 + 2 from voidmulti's ExtendedThermalLance). See `docs/audits/unit-stats.md` (Colossus).
- No Colossus / thermal-lance string currently exists in the mod's `GameStrings.txt`.

## Approach
Mirror WA-016 / WA-017: add a tooltip line (unit or train-button tooltip) noting the Colossus begins with Extended Thermal Lance (9 range). Pick the same tooltip surface those used (check how the Marine/Hydralisk tooltips were wired — likely a `Button/Tooltip/...` or unit tooltip GameStrings entry).

## Acceptance criteria
- [ ] A tooltip visible in-game states the Colossus starts with Extended Thermal Lance / 9 range.
- [ ] Wording/placement consistent with the Marine + Hydralisk free-upgrade tooltips.

## Notes
Found while reviewing WA-043 (unit stats doc). Pure GameStrings/tooltip work — locally testable.
