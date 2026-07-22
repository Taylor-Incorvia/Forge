---
id: WA-063
status: todo
size: M
phase: 1-game-readiness
priority: 45
---
# Ultralisk concussive applies no slow at all — pulled from the pool, needs investigation

## Symptom
With `ConcussiveUltralisk` researched, an attacking Ultralisk (KaiserBlades) applies **no slow** to any target — not the primary, not the cleaved targets. Confirmed in-game across two attempts (see below). Every other concussive unit tested works (Marauder, VoidRay, Hellion/Hellbat). Pulled from the pool for now (`upgradeInitializers.galaxy` + `upgradeFamilyHelpers.galaxy`, both commented with this ticket id).

## What was verified (all correct — rules these out)
- **Weapon is `KaiserBlades`.** liberty Ultralisk `WeaponArray` index 0 = `KaiserBlades` (index 1 `Ram` is removed by swarm). The stock `KaiserBladesSearch` `ExcludeArray Effect="KaiserBlades"` confirms the weapon fires the `KaiserBlades` **CEffectSet** (not the bare damage).
- **Upgrade → requirement → validator chain is byte-identical to the working Hellion:** `ConcussiveUltralisk` (marker CUpgrade) → `UseConcussiveUltralisk` (CRequirement) → `CountUpgradeConcussiveUltraliskCompleteOnly` → validator `UltraliskConcussiveResearched`. Parallel to `ConcussiveHellion`, which works.
- **`UltraliskConcussiveSlow` effect is byte-identical to `HellionConcussiveSlow`** (same validators incl. `NotMassive`, `Behavior=Slow`).
- **CWeaponLegacy-fires-same-name-set convention holds** — proven by the Hellbat (`HellionTank` weapon → `HellionTank` set, concussive injection works).

## What was tried and FAILED
1. **Original WA-034 wiring** — mod overrides `CEffectSet KaiserBlades` to replace index 0 (`KaiserBladesDamage`) with a nested `KaiserBladesHit` set = {damage, slow}; search rerouted to `KaiserBladesHit`. → no slow.
2. **VoidRay-pattern rewrite** — instead of replacing index 0, *added* `UltraliskConcussiveSlow` at a new index (`EffectArray index="2"`) on the `KaiserBlades` set, leaving stock damage/search intact. This is exactly how the **working** VoidRay does it (`EffectArray index="3"`). → still no slow. (Reverted; the set override is back to the WA-034 index-0 form.)

## Leading hypothesis
The mod's `<CEffectSet id="KaiserBlades">` override **is not merging into the live catalog at all** — neither replacing index 0 nor adding index 2 had any effect, while the identical patterns work for VoidRay/Hellbat. If the override merged, at least the *added-index* attempt (#2) would have slowed the primary. Something is special about `KaiserBlades`:
- Possible the multiplayer Ultralisk's attack resolves through a **different** effect id than the `KaiserBlades` set the mod is editing (e.g. an intermediate the mod never touches), despite the ExcludeArray reference.
- Possible a **load-order / duplicate-definition** issue on `KaiserBlades` specifically.

## Next steps (for whoever picks this up)
1. **Editor merged-view check:** open the merged `KaiserBlades` CEffectSet in the editor and confirm whether the injected effect is actually present in the live catalog. This is the fastest way to confirm/deny the "override not merging" hypothesis.
2. If it IS present but still no slow: put a throwaway, un-validated `Slow` (drop the `UltraliskConcussiveResearched` validator) directly on `KaiserBladesDamage` and see if *anything* slows — isolates effect-plumbing vs. validator.
3. If the set override genuinely doesn't merge: trace the Ultralisk's *actual* live attack effect from the unit → weapon → Effect in the merged view (don't trust the extracted reference), and inject there instead.
4. Watch for the **cleave/search** being a second, separate failure once the primary works (the checklist's original "search may be inactive" note).

## Acceptance criteria
- [ ] Root cause identified (why the `KaiserBlades` injection didn't take).
- [ ] Ultralisk attack slows the **primary** target with `ConcussiveUltralisk` researched.
- [ ] Cleaved (arc) targets also slow.
- [ ] Massive targets do NOT slow (`NotMassive` preserved).
- [ ] Re-enable in `upgradeInitializers.galaxy` + `upgradeFamilyHelpers.galaxy` (uncomment).

## Notes
Related: WA-034 (concussive system), and the working reference implementations to copy — VoidRay (`EffectArray index="3"` add) and Hellion/Hellbat (set-wrap). Effect ids involved: `KaiserBlades` (set), `KaiserBladesHit`, `KaiserBladesSearch`, `KaiserBladesDamage`, `UltraliskConcussiveSlow`.
