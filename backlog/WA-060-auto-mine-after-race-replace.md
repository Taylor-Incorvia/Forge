---
id: WA-060
status: todo
size: M
phase: 1-game-readiness
priority: 35
---
# Auto-mine after race replacement — workers should gather at 0:00 no matter which race you picked

## Why
The mod already makes race choice cosmetic: if you pick Zerg/Protoss, `convertNonTerranUnit` swaps your starting units to Terran equivalents at 0:00. The **one** remaining tell is that your replaced workers **stand idle** (Brood-War style) instead of auto-mining — you have to hand-order them onto minerals. Fixing this makes the pick truly not matter. **Not needed before the first prod push** (players can right-click their workers), but it's the last parity gap.

## Root cause (investigated)
`convertNonTerranUnit` (`TriggerLibs/unitInitializers.galaxy:80-85`) does `UnitRemove(Drone/Probe)` then `UnitCreate(1, "SCV", ...)`. SC2's melee initialization hands the **auto-gather order to the ORIGINAL** Drones/Probes; the freshly trigger-created SCVs never receive it, so they idle. (Terran picks are unaffected — their SCVs come from melee init and auto-mine, which is why only non-Terran picks show the bug.) The Hatchery/Nexus→CommandCenter swap already gives them a valid return point, so only the gather order is missing.

## Fix approach
In the Drone/Probe branch of `convertNonTerranUnit`, after creating the SCV, grab it and send it to mine:
```galaxy
UnitRemove(u);
UnitCreate(1, "SCV", 0, owner, pos, 270.0);
newScv = UnitLastCreated();
// then order newScv to harvest the nearest mineral field (see below)
```

### Building blocks (confirmed in reference / mod)
- **Harvest ability:** `SCVHarvest` (`CAbilHarvest`, liberty `abildata.xml:1094`). The SCV carries it.
- **Order the SCV to a mineral:** `UnitIssueOrder(newScv, OrderTargetingUnit(AbilityCommand("SCVHarvest", 0), mineralUnit), c_orderQueueReplace)`. (The mod already uses `UnitIssueOrder` + `AbilityCommand` in `onBlinkUsed`, WA-015 — `OrderTargetingUnit` is the targeted variant.)
- **Get the created SCV:** `UnitLastCreated()` immediately after `UnitCreate`.
- **Mineral field unit types** (all parent `MineralFieldDefault`, liberty `unitdata.xml`): `MineralField`, `MineralField450`, `MineralField750`, `MineralFieldOpaque`, `MineralFieldOpaque900`, and rich variants. A search needs to catch all of them — filter by the resource/mineral unit filter rather than listing ids, or enumerate a `UnitGroup` in a ~12 radius around the SCV and keep the closest.

## Open questions / test first (cheapest → most work)
1. **Does an UNtargeted `SCVHarvest` order auto-pick the nearest patch?** Try `UnitIssueOrder(newScv, Order(AbilityCommand("SCVHarvest", 0)), c_orderQueueReplace)` first — if the SCV auto-finds the nearest mineral, we skip the search entirely. (Test before building the enum.)
2. **Distribution vs nearest.** For *fairness* (the whole point — race must not matter), the SCVs should spread across patches like a real Terran start, not all pile onto one patch (queuing = slower early economy for non-Terran pickers). Preferred: enumerate the ~8 mineral fields near the CC and round-robin the replaced SCVs across them. Simpler fallback: send each to its own nearest patch.
3. **Timing.** `convertNonTerranUnit` runs from `onUnitCreated` at game start; mineral fields exist then, so an immediate order should work. If the order gets dropped on the creation frame, fall back to a tiny-delay (0.0625s) trigger that sweeps the player's idle SCVs and orders them.
4. **Gas:** not needed — everyone starts all workers on minerals.

## Acceptance criteria
- [ ] Pick Zerg or Protoss → at 0:00 your (now-Terran) workers begin mining automatically, no manual input.
- [ ] Economy ramp matches a Terran pick (workers distributed, not all queued on one patch).
- [ ] Terran picks unchanged.

## Notes
Scope is isolated to the race-replacement path (`convertNonTerranUnit`) + whatever the editor trigger that calls it is wired to. Not a devMode thing — real gameplay parity. Sibling: the race-replacement feature (no ticket) and [[WA-015]] (same `UnitIssueOrder` pattern).
