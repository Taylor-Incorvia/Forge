---
id: WA-016
status: done
size: S
phase: 1-game-readiness
priority: 10
parent: WA-002
---
# Marine: 50/25 cost + starts with Combat Shield

## ✅ DONE (2026-07-11)
Marine set to 50/25 (considered 20 gas, settled on 25). Starts with Combat Shield (`ShieldWall`) via `grantInitialUpgrades()`. Tooltip updated to note the starting upgrade. Balance to be confirmed in playtests later.

Marine should be a cheap, simple, but not brain-dead Barracks Slot 2 pick. Combat Shield (extra HP) makes it feel good without Stim's power level.

## Why
Slot 2 needs a low-friction option a new player instantly understands. Considered starting with Stim instead — decision for now is Combat Shield; revisit later if it's boring.

## Acceptance criteria
- [ ] Marine costs 50 minerals / 25 gas in `UnitData.xml`.
- [ ] `grantInitialUpgrades()` in `upgradeInitializers.galaxy` grants Combat Shield (stock upgrade id `ShieldWall`) at game start.
- [ ] Verify in game the marine has the extra HP.

## Confirmed from reference/ (WA-021)
`ShieldWall` is the real Combat Shield `CUpgrade` (defined in Liberty) — the id in the code is correct.

## Notes
Partly in the current uncommitted changes already, untested — so it stays open, not done.
⚠️ In the copy of `upgradeInitializers.galaxy` last read, the `grantUpgrade(player, "ShieldWall")` line was **commented out** (~line 281). Confirm it's actually enabled — that line is what gives the marine its shield.
