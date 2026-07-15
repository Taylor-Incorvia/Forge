---
id: WA-003
status: done
size: M
phase: 1-game-readiness
priority: 3
---
# Verify unit costs

Do a full pass over every unit's cost in `UnitData.xml` and confirm it's intentional, not a leftover from ladder or an old balance experiment.

## Why
Costs drive everything in a random-unit game — a mispriced unit is either a trap pick or an auto-win. The balance journal in the galaxy files shows costs have been hand-tuned repeatedly (marine 50/25, hydralisk bumped, firebat bumped for Bearclaw). Worth one deliberate sweep.

## Acceptance criteria
- [ ] List every playable unit with its current mineral/gas/supply cost.
- [ ] Flag any cost that looks like a stock-SC2 default that was never revisited.
- [ ] Flag units whose granted starting upgrades (see `grantInitialUpgrades`) justify a price that isn't reflected.
- [ ] Produce a short table of proposed changes for your sign-off (I won't change costs without you).

## Notes
`UnitData.xml` is the source of truth and is currently modified in your working tree. Starting upgrades that affect value: ChitinousPlating (Ultra), ExtendedThermalLance (Colossus), BearclawNozzles (Firebat), WraithCloak, BlinkTech (Stalker), PsiStormTech (HT).
