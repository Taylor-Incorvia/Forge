---
id: WA-019
status: in-progress
size: M
phase: 1-game-readiness
priority: 13
parent: WA-002
depends_on: WA-018
---
# Add Queen as the third Barracks Slot 2 unit

## 🔨 Implemented (2026-07-12) — needs in-game test
Full wiring across 6 files (mirrors the Marine/Hydralisk template). Load-bearing ids: `Queen` ↔ `barracks2queenupg` ↔ `barracks2queenreq` ↔ `BarracksTrain,Train21` ↔ button face `Queen`.
- `initialize.galaxy`: `setUnitUnlockUpgrade("Queen","barracks2queenupg")` + `addUnitToSlotPool(rax,2,"Queen")`.
- `unitInitializers.galaxy`: `tagHybridCaster("Queen")`.
- `UpgradeData.xml`: `<CUpgrade id="barracks2queenupg"/>`.
- `RequirementNodeData.xml`: count + GTE nodes. `RequirementData.xml`: `barracks2queenreq`.
- `AbilData.xml`: `BarracksTrain` → `Train21` (Time 40, tunable).
- `ButtonData.xml`: `<CButton id="Queen">` (icon `btn-unit-zerg-queen.dds`).
- `UnitData.xml`: Barracks card `LayoutButtons` (Row0/Col1) + `<CUnit id="Queen">` override — Speed 1.9, +50 gas, hides creep-tumor/spawn-larva/burrow via **card removal only** (card idx 5/6/8), keeps Transfusion.
  - ⚠️ Gotcha fixed: removing those abilities via `AbilArray index` ALSO stripped Transfusion (merge indices shift). Switched to card-button removal — abilities stay on the unit but have no button, so they're unusable.
- `GameStrings.txt`: Button name/tooltip + upgrade name.

### ⚠️ Verify in game
- **Button icon** — `btn-unit-zerg-queen.dds` path is a guess; if the train button is blank, fix the icon path.
- Queen trains from Barracks (150/50), moves ~1.9 (slower than Marine/Hydra), has Transfusion + both attacks, and has **no** creep tumor / spawn larva / burrow buttons.
- Hybrid: her slot can roll both a caster spell and a fighter buff (and NOT Transfusion — already excluded).
- `Train21` index accepted by `BarracksTrain` (should be — it's used in other train abilities).

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
