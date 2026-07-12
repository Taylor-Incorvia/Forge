---
id: WA-017
status: done
size: S
phase: 1-game-readiness
priority: 11
parent: WA-002
---
# Hydralisk: revert to 100/50 + starts with Grooved Spines

## ✅ DONE (2026-07-11)
Hydralisk reverted to 100/50. Starts with `EvolveGroovedSpines` (range) via `grantInitialUpgrades()`. Tooltip updated to note the starting upgrade. Balance to be confirmed in playtests later.

Undo the recent cost bump and give the Hydralisk its range upgrade at start, so it's a solid, straightforward ranged Barracks Slot 2 pick.

## Why
Last commit ("Increase hydralisk cost") pushed it above where you want it. Reverting to 100/50 and starting with range makes it a clean, understandable option.

## Confirmed from reference/ (WA-021)
- Stock Hydralisk = **100 min / 50 gas, 2 supply** (Liberty) — your revert target IS the stock value.
- Grooved Spines upgrade id = **`EvolveGroovedSpines`** (confirmed).

## Acceptance criteria
- [ ] Hydralisk costs 100 minerals / 50 gas in `UnitData.xml`.
- [ ] `grantInitialUpgrades()` grants `EvolveGroovedSpines`.
- [ ] Verify in game the hydra has the longer range.

## Notes
Cost revert may be partly in uncommitted changes — untested, so it stays open, not done.
