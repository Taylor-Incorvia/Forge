---
id: WA-032
status: todo
size: S
phase: 1-game-readiness
priority: 27
---
# Fix upgrade-pool eligibility bugs (from WA-004 audit)

The upgrade-matrix audit (`docs/audits/upgrade-matrix.md`) surfaced concrete eligibility bugs. All are small `addUpgradeRequirementTag` fixes in `upgradeInitializers.galaxy`.

## Confirmed bugs
- [ ] **D8Charge case-mismatch.** Registered `AnyOf[Thor, Immortal, Warhound]`, but the real unit-tags are **`ThorAP`** and **`WarHound`** — so ThorAP and WarHound can *never* roll D8Charge (it's silently Immortal-only). Change `"Thor"` → `"ThorAP"` and `"Warhound"` → `"WarHound"`.
- [ ] **Battlecruiser + Yamato duplicate.** BC can roll Yamato Cannon, which it already has natively. Add `addUpgradeRequirementTag("Yamato", logicType_NoneOf, "unitTag", "Battlecruiser")` (same pattern as Phoenix/GravitonBeam, Viper/ParasiticBomb, etc.).

## To verify (decide, then act)
- [ ] **Raven + SeekerMissile.** Raven's BuildAutoTurret / RavenScramblerMissile are guarded ("raven already has this"), but SeekerMissile is not. If this mod's Raven has a Hunter-Seeker-style ability, add `NoneOf[Raven]`; if not, leave it.
- [ ] **Battlecruiser + Hyperjump.** BC's card hard-adds a Hyperjump button (`UnitData.xml`) but BC isn't in Hyperjump's AnyOf list — it uses Hyperjump natively yet can't roll it as an upgrade. Cosmetic data inconsistency; decide if the button should stay.

## Notes
Locally testable (pure galaxy tag edits). Low risk. Found during the WA-004 verification sweep.
