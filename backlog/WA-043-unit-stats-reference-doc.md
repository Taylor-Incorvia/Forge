---
id: WA-043
status: todo
size: M
phase: 1-game-readiness
priority: 33
---
# Documentation: full per-unit stats reference

## Goal
A single reference doc listing the **effective (merged) stats for every rollable unit** — so balance decisions (and eventually player-facing docs) don't require re-deriving numbers from the catalogs each time.

Target file: `docs/audits/unit-stats.md` (companion to `unit-costs.md` and `upgrade-pools-by-unit.md`).

## Per-unit fields
For every unit in the slot pools (all 3 facilities × their slots — see `docs/audits/upgrade-pools-by-unit.md` for the roster):
- Facility + slot
- Cost: Minerals, Vespene, Supply
- Build time (seconds)
- Life, Shields (if any), Armor
- Move speed
- Each weapon: name, damage (× attacks, + bonus vs tag), attack period, range, targets (air/ground/both), computed DPS

## How to build it (important — merge two sources)
Stats are **not** all in the mod. Merge:
1. **Mod overrides** — `ForgeModLowConfidence.SC2Mod/Base.SC2Data/GameData/` (UnitData.xml costs/HP/weapons; AbilData.xml `BarracksTrain`/`FactoryTrain`/`StarportTrain` for **build times** — these override base).
2. **Base catalogs** — the mod's deps under `reference/`: **Liberty Campaign + Void Mod + Void Multi**. voidmulti is authoritative for MP units; Wraith comes from the Liberty *campaign*. Use base only where the mod doesn't override.

Flag each mod-overridden value so future edits know what's custom (mod-repriced so far: Marine gas 25, Queen gas 50 + speed, Wraith 100/100, Archon 225/150; and all build times are mod-set).

## Caveats to note in the doc
- Weapon **Periods are pre-game-speed catalog values**; in-game runs at "Faster" (~×1.4), so effective DPS is ~1.4× the listed catalog DPS. State this once and list catalog values consistently.
- Multi-form units (Siege Tank, Viking, Lurker) — pick the relevant combat form and say which.

## Acceptance criteria
- [ ] `docs/audits/unit-stats.md` covers every unit in the slot pools with all fields above.
- [ ] Mod-overridden values are flagged vs base.
- [ ] Game-speed caveat stated; DPS computed consistently.
- [ ] Linked from the other audit docs / board where appropriate.

## Notes
A big chunk of this was already extracted 2026-07-15 for the Queen/Wraith comparison (Barracks s2 + Starport s1 + Mutalisk) — reuse those numbers as the first rows. Size M mostly because it's ~30 units × several catalogs. Feeds Phase 2 "Unit list" docs later.
