---
id: WA-084
status: todo
size: M
phase: 2-polish
priority: 70
---
# Corsair attack visual — add a punchy laser (Neutron Flare has no beam)

## What
The Corsair's attack reads as "does nothing" — it fires with a launch **sound** but no visible beam or projectile. Add a punchy laser beam (and impact flash) so the attack lands with weight. Pure-visual, no gameplay change.

## Why it looks broken (root cause)
`CActorAction id="CorsairMPAttack"` (parent `GenericAttack`, `effectAttack="NeutronFlare"`) specifies only `LaunchAssets Sound="CorsairMP_AttackLaunch"` + an impact sound — **no launch/beam model.** So you hear it, see almost nothing. (Base Void data ships it this way.)

## Approach (beam)
Add a `CActorBeamSimple` keyed to the Neutron Flare attack — created when the weapon fires, drawn from the Corsair's weapon attach point to the target, **one beam per shot**.
- Model it on the **Colossus attack beam** (`ColossusAttackBeamAir`, actordata ~2318) — a *per-shot* beam, the right fit for the Corsair's discrete attacks (the Oracle's `OracleWeaponAttackBeam` ~2737 is a *continuous* channel — wrong shape). `VoidRaySwarmAttackBeam` ~2985 is another template.
- Reuse a base Protoss beam **model** (e.g. `RipFieldAttackBeam`, the Colossus lance, or the Void Ray beam) — **no new art**; those assets resolve from the shared game files regardless of dependencies. Pick a bright one for punch.
- Optional extra punch: a bright impact flash model on hit + a muzzle glow at the launch attach point.
- Weapon is `NeutronFlare` (CWeaponLegacy); it attacks **air**, so use an air-capable beam (Colossus/Void Ray beams handle air).

## Cheaper alternative (if a literal beam isn't required)
Add a **projectile `LaunchModel` + impact flash** to the existing `CActorAction` instead of a beam. Less iteration; gives a visible bolt that *lands* with weight, just not a laser line.

## Effort / risk
Moderate. The actor is small + self-contained (~15–30 lines of ActorData) and **pure-visual → zero gameplay risk**. Cost is iteration: attack visuals must be seen live, and tuning the beam's timing / attach point / duration (so it doesn't linger) is trial-and-error — same publish-and-eyeball loop as the Firebat flames. Budget a couple of test-publish cycles.

## Acceptance
- [ ] Corsair's attack shows a visible, punchy beam (or bolt+flash) from unit to target, per shot.
- [ ] Beam appears/disappears cleanly with each attack — no lingering/stuck beams.
- [ ] No gameplay change (visual only). Verify on a published build.

## Notes
Post-launch polish. Corsair is `CorsairMP` (restored BW unit, not WoL) — stays regardless of the WoL-dependency decision. Icon was already fixed (real `btn-unit-protoss-corsair.dds`); this ticket is the remaining "weapon looks weak" half.
