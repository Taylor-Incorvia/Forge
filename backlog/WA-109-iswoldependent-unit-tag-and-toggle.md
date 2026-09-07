---
id: WA-109
status: todo
size: M
phase: 2-post-launch
priority: 2
---
# `isWolDependent` unit tag + toggle to exclude WoL units from the pools

Part of [[WA-108]]. Items 3 + 4 (one feature).

## Goal
A clean, single-switch way to pull every WoL-dependent unit out of the production slot pools — for fast
A/B ("does the game play fine without WoL units?") and to stage the removal experiment. **Reversible,
no data deletion** — it just gates pool membership.

## Recommended approach
Units are added via `addUnitToSlotPool(facility, slot, unitId)` (initialize.galaxy). Cleanest options:

**Option A (wrapper — simplest, mirrors the existing add* helpers):**
- Add `bool excludeWolUnits` near `devMode` in nativeHelpers.galaxy (commit `false`).
- Add `void addWolUnitToSlotPool(facility, slot, unitId)` that calls `addUnitToSlotPool` **only if
  `!excludeWolUnits`**, and also records `unitId` in a `wolDependentUnits` list.
- Convert the WoL units' pool lines to the wrapper: Goliath, Wraith, Diamondback, DuskWing, Firebat,
  Medic, Vulture (the 7 from [[WA-078]]). Everything else stays on `addUnitToSlotPool`.

**Option B (real tag — more reusable):** give each WoL unit an `isWolDependent` unit tag (the mod
already has a unit-tag system), and filter tagged units at pool-population when the toggle is on. Pick
this if you want WoL-ness queryable elsewhere (modal labeling, future logic). More plumbing than A.

**Recommendation:** Option A now (least code, does the job); promote to a tag (B) only if a second
consumer of "is this unit WoL?" appears.

## ⚠️ Scope caveat
This gates **rolls**, not command-card DATA. It's for gameplay isolation, NOT the escape-key experiment
(that needs the card data gone — see [[WA-112]]). Don't expect this toggle to affect the escape bug.

## Acceptance
- [ ] `excludeWolUnits = true` → no WoL unit ever rolls in any slot; every slot still fills from the rest of its pool (`testCaseNumber` sweep).
- [ ] `excludeWolUnits = false` → identical to today.
- [ ] Toggle committed `false`.
