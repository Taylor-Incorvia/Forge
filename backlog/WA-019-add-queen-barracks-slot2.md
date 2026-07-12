---
id: WA-019
status: todo
size: M
phase: 1-game-readiness
priority: 13
parent: WA-002
depends_on: WA-018
---
# Add Queen as the third Barracks Slot 2 unit

A near-stock Queen as the third slot-2 option alongside Marine and Hydralisk, adjusted for a mod with no creep and no hatcheries.

## Why
Rounds out slot 2 with a defensive/support-flavored pick that plays very differently from the two straightforward fighters.

## Depends on WA-018
Her upgrade pool (hybrid caster+fighter, researched from the Ghost Academy) comes from WA-018. Do the hybrid mechanism first, then tag the Queen into it here.

## Acceptance criteria
- [ ] Queen can roll into Barracks Slot 2 (add to the slot-2 pool in `initialize.galaxy`).
- [ ] Off-creep move speed raised to ~2.5–3, tuned to be clearly **slower than both Marine and Hydralisk**.
- [ ] Remove Spawn Creep Tumor (no creep) and Spawn Larva (no hatcheries).
- [ ] Keep Transfuse.
- [ ] Cost 150/50 (bump to 150/75 if she proves too strong).
- [ ] Tag her so WA-018's hybrid mechanism gives her a sensible upgrade pool.

## Confirmed from reference/ (WA-021)
Stock Queen ability ids (Swarm `abildata.xml`):
- **Keep:** `Transfusion` (CAbilEffectTarget).
- **Remove:** `CreepTumorBuild` (build creep tumor) and `LarvaTrain` (spawn larva).
- Double-check the Queen's command-card button set when implementing, in case a wrapper ability points at these.

## Notes
Off-creep speed lives on the unit's Mover / speed field — in stock SC2 queens crawl off-creep, which won't work here since there's no creep. Base Queen `<CUnit>` is in Swarm (with VoidMulti/Liberty overrides) — reference it for the current speed values before tuning.
