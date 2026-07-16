---
id: WA-032
status: done
size: S
phase: 1-game-readiness
priority: 27
---
# Fix upgrade-pool eligibility bugs (from WA-004 audit)

## ✅ Done 2026-07-16
- **D8Charge case-mismatch:** `"Thor"`→`"ThorAP"`, `"Warhound"`→`"WarHound"` in `upgradeInitializers.galaxy`. D8Charge now actually reaches ThorAP + WarHound (was silently Immortal-only).
- **Battlecruiser + Yamato:** added `NoneOf Battlecruiser` on Yamato — BC has it natively, no more duplicate roll.
- **Raven + SeekerMissile:** left as-is — this mod's Raven has no Hunter-Seeker natively (removed from MP years ago), so rolling it is fine (a nostalgia pick), not a bug.
- **Battlecruiser + Hyperjump:** no change — Hyperjump is an `AnyOf`-only list and BC isn't in it, so BC already can't roll it. Native BC Hyperjump button is intended.
- Regenerated affected rows in `docs/audits/upgrade-pools-by-unit.md` (ThorAP/WarHound +D8Charge, Battlecruiser −Yamato).

The upgrade-matrix audit (`docs/audits/upgrade-matrix.md`) surfaced concrete eligibility bugs. All are small `addUpgradeRequirementTag` fixes in `upgradeInitializers.galaxy`.

## Confirmed bugs
- [x] **D8Charge case-mismatch.** `"Thor"` → `"ThorAP"`, `"Warhound"` → `"WarHound"`. Now reaches ThorAP + WarHound.
- [x] **Battlecruiser + Yamato duplicate.** Added `NoneOf Battlecruiser` on Yamato.

## To verify (decide, then act)
- [x] **Raven + SeekerMissile.** Left enabled — Raven has no native Hunter-Seeker in this mod (removed from MP years ago); rolling it is intended.
- [x] **Battlecruiser + Hyperjump.** No change — Hyperjump is `AnyOf`-only and BC isn't listed, so BC already can't roll it. Native button stays.

## Notes
Locally testable (pure galaxy tag edits). Low risk. Found during the WA-004 verification sweep.
