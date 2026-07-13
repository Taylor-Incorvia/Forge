---
id: WA-026
status: done
size: M
phase: 1-game-readiness
priority: 22
---
# Stim indicator doesn't show on units that don't normally stim

## ✅ DONE (2026-07-12)
Confirmed cause: the stim flash keys on the *ability* (`Abil.Stimpack.SourceCastStart`), but rolled stim casts `F_Stimpack` → `Abil.F_Stimpack.SourceCastStart`, which wasn't listened for (Marine's `F_Stim` showed no flash in the test). Fix: overrode `StimpackStartImpact` in `ActorData.xml` to append `<On Terms="Abil.F_Stimpack.SourceCastStart" Send="Create"/>`. Verified in game — start flash and end flash both fire for any unit that rolls stim. Keys on the ability, so it's general (fixes the "why is that Hydralisk so fast?!" confusion). devMode roll-forcing hacks removed.

When a rolled **Stimpack** upgrade is applied to a unit that doesn't natively use stim (e.g. a Queen, Void Ray, etc. via `F_Stimpack`), the visual "stimmed" indicator/overlay above the unit does not appear. The unit IS stimmed (gets the speed/attack boost), but there's no telegraph.

## Why
Legibility — both players should be able to see that a unit is stimmed. A hidden buff on a non-standard unit is confusing and hard to play around.

## Mechanism found
The stim visual = two stock actors (reference `liberty.../actordata.xml`):
- `StimpackStartImpact` — fires on `Abil.Stimpack.SourceCastStart` / `Abil.StimpackMarauder.SourceCastStart` (keys on the ABILITY name).
- `StimpackEndImpact` — fires on `Behavior.Stimpack.Off` (keys on the BEHAVIOR).

`F_Stimpack` applies the same `Stimpack` effect/behavior, but its cast fires `Abil.F_Stimpack.SourceCastStart` — which the start actor doesn't listen for. (`F_Stimpack` has `AbilSetId="Stimpack"`, apparently meant to redirect this; the test below checks if it works.) The END impact should still fire (behavior-keyed).

## Test set up (devMode hacks, WA-026)
`devMode` forces Marine into Barracks slot 2 + Stimpack as its upgrade (hacks in `forgeProductionFacilityHeplers.setRandomSlotUnitFromPoolForPlayer` and `upgradeInitializers.assignRandomUpgradeFromPoolToPlayerSlot`). Research it → Marine gets `F_Stimpack` → cast → does the flash show?
- Flash → AbilSetId works; missing indicator on other units is unit-model-specific.
- No flash → override `StimpackStartImpact` (mod actordata) to also listen for `Abil.F_Stimpack.SourceCastStart`.

## Acceptance criteria
- [ ] Determine (via the test) whether it's the ability-name mismatch or unit-model-specific.
- [ ] Make the indicator show for ANY unit that casts `F_Stimpack`.
- [ ] Verify on a non-standard unit.
- [ ] Remove the devMode roll-forcing hacks when done.
